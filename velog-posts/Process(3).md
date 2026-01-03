<h1 id="process-creation-unixlinux">Process Creation: Unix/Linux</h1>
<p><code>int fork()</code></p>
<ul>
<li>새로운 PCB (프로세스 제어 블록)를 생성하고 초기화</li>
<li>새로운 <strong>주소 공간</strong>을 생성하고 초기화합니다.</li>
<li>부모의 주소 공간 내용 전체를 복사하여 자식의 주소 공간을 초기화합니다.</li>
<li>커널 자원들을 부모가 사용하던 자원(예: 열린 파일)을 가리키도록 초기화합니다.</li>
<li>새로운 $\text{PCB}$를 준비 큐 (Ready Queue)에 넣습니다</li>
<li>부모에게는 자식의 PID를 반환하고, 자식에게는 <strong>0</strong>을 반환합니다.</li>
</ul>
<h3 id="sharing-of-open-files-between-parent-and-child-after-fork">Sharing of open files between parent and child after fork</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/046d2a89-01a1-42aa-bd0e-21d210877356/image.png" /></p>
<p>parent Process Table의 ptr을 Child Process Table로 복사</p>
<h3 id="fork">fork()</h3>
<pre><code class="language-c">#include &lt;stdio.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;unistd.h&gt;

int main(void)
{
    int pid;

    if ((pid = fork()) == 0) {
        /* child */
        printf(&quot;Child of %d is %d\n&quot;, getppid(), getpid());
    } else {
        /* parent */
        printf(&quot;I am %d. My child is %d\n&quot;, getpid(), pid);
    }

    return 0;
}
</code></pre>
<p>pid(<code>fork()</code>의 return값)가 0이면 child process
<code>fork()</code>가 자식의 PID를 return하면 parent process.</p>
<p>이를 통해 역할분기를 나눌 수 있음.    </p>
<h3 id="why-fork">why fork?</h3>
<p>다음과 같은 상황에서 부모–자식 프로세스 구조는 매우 효과적.</p>
<ul>
<li>자식 프로세스가 부모 프로세스와 협력(cooperating) 해야 하는 경우</li>
<li>자식 프로세스가 작업을 수행하기 위해 부모 프로세스의 데이터에 의존하는 경우</li>
<li>대표적인 예시: 웹 서버(Web Server)</li>
</ul>
<p><em><strong>그냥 하나의 프로세스가 필요하면 새로 만들면 되지, 왜 굳이 복사(fork)할까?</strong></em></p>
<p>유닉스 철학의 핵심 원칙 중 하나는 다음과 같다.</p>
<p>하나의 프로그램은 한 가지 일을 잘 하도록 만든다.
그리고 여러 프로그램(또는 프로세스)을 조합하여 복잡한 작업을 수행한다.</p>
<p>프로세스를 새로 만든다는 것은 생각보다 복잡</p>
<ul>
<li>코드 메모리 배치</li>
<li>힙, 스택 초기화</li>
<li>권한 정보</li>
</ul>
<p>이 모든 것을 처음부터 다시 설정해야 한다.</p>
<p>반면 <code>fork()</code>는:</p>
<ul>
<li><p>현재 프로세스의 실행 상태</p>
</li>
<li><p>열린 파일 디스크립터</p>
</li>
<li><p>메모리 구조</p>
</li>
</ul>
<p>를 그대로 복사한다.→ 이미 검증된 실행 환경을 즉시 재사용할 수 있다.</p>
<h1 id="process-execution-unixlinux">Process Execution: Unix/Linux</h1>
<p><code>exec()</code></p>
<ul>
<li>현재 프로세스의 실행을 중단한다</li>
<li>프로그램 prog를 해당 프로세스의 주소 공간에 로드한다</li>
<li>새로운 프로그램을 위해 하드웨어 실행 컨텍스트와 인자들을 초기화한다</li>
<li>프로세스 제어 블록(PCB)을 준비 큐(ready queue)에 넣는다</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/66c7a35d-2172-4306-8e78-44d9694169a5/image.png" /></p>
<pre><code class="language-c">int main()
{
    while (1) {
        char *cmd = read_command();
        int pid;

        if ((pid = fork()) == 0) {
            exec(cmd);
            panic(&quot;exec failed!&quot;);
        } else {
            wait(pid);
        }
    }
}
</code></pre>
<p>child process면 cmd를 exec해서 자신의 주소공간을 <code>cmd</code>가 가리키는 새로운 프로그램으로 교체한다.
따라서 <code>panic()</code> 명령어가 실행된다면, 그건 exec이 실패한것</p>
<p>부모프로세스는 wait queue에 집어넣어서 자식 프로세스가 종료될 때까지 대기한다.</p>
<h1 id="process-termination">Process Termination</h1>
<p>Normal termination</p>
<ul>
<li>main() 함수에서 return하는 경우</li>
<li>exit()를 호출하는 경우</li>
<li>_exit()를 호출하는 경우</li>
</ul>
<p><code>exit()</code>은 출력버퍼에 등록된 정리작업을 수행하고 Termination
<code>_exit()</code>은 커널 시스템콜로, 즉시 프로세스를 종료함</p>
<p>Abnormal termination</p>
<ul>
<li>abort()를 호출하는 경우</li>
<li>시그널(signal)에 의해 종료되는 경우</li>
<li>자식 프로세스 종료 대기 (Wait for termination of a child process)</li>
</ul>
<p>wait()</p>
<ul>
<li>부모 프로세스가 wait()를 호출하지 않으면, 해당 프로세스는 zombie process가 된다</li>
<li>부모 프로세스가 wait()를 호출하지 않은 채 종료되면, 해당 프로세스는 orphan process가 된다
<img alt="" src="https://velog.velcdn.com/images/qixiangme/post/b75d9385-e24d-4832-823d-900e87e66313/image.png" /></li>
</ul>
<h1 id="inter-process-communication-ipc">Inter-Process Communication (IPC)</h1>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/3fec6267-9b59-42ec-8080-99c2c834a55a/image.png" /></p>
<p>왼쪽은 커널에 의해 관리되는 message queue에 접근하여
Prcoess A와 B가 Communication을 함</p>
<ul>
<li>동기화 단순</li>
<li>분산환경에 적합</li>
<li>시스템 콜 오버헤드가 존재</li>
<li>속도 느림</li>
</ul>
<p>오른쪽은 Process A와 Process B가 공유하는 메모리에  서로 읽고 쓰고하면서 Communication</p>
<ul>
<li>속도 빠름</li>
<li>커널 개입이 적어서 개발자가 직접 관리</li>
<li>동기화 문제 가능성</li>
</ul>