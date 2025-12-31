<h1 id="context-switch">Context Switch</h1>
<p>CPU를 하나의 프로세스에서 다른 프로세스로 전환하는 행위</p>
<p><strong>Administrative overhead</strong></p>
<ul>
<li>레지스터와 메모리 맵을 저장하고 다시 로드하는 작업</li>
<li>메모리 캐시를 비우고 다시 로드하는 작업.</li>
<li>다양한 테이블과 목록 등을 업데이트하는 작업.</li>
</ul>
<p>또한 Context Switch에 드는 오버헤드는 하드웨어 지원에 따라 달라집니다</p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/718c71a6-480f-4755-932b-d9e0faf84960/image.png" />
<strong>그림은 P0 -&gt; P1으로 Context Switch하는 과정</strong></p>
<p>1.P0 정보 저장 (save state into PCB0)
2.P1 정보 불러오기(reload state from PCB1)
3.P1 작업 실행 
4.interrupt나 Systemcall에 의해 P1에서 context switch필요하게 되면
5.P1 정보 저장 (save state into PCB1)
6.다른 프로세스 실행... 및 반복</p>
<h1 id="schedulers">Schedulers</h1>
<p><strong>Long-term scheduler (또는 작업 스케줄러)</strong></p>
<ul>
<li>어떤 프로세스들을 준비 큐(ready queue)로 가져올지 선택합니다.</li>
</ul>
<p><strong>Short-term scheduler (또는 CPU 스케줄러)</strong></p>
<ul>
<li>다음에 어떤 프로세스를 실행할지 선택하고 $\text{CPU}$를 할당합니다.</li>
</ul>
<p><strong>Medium-term scheduler (또는 스와퍼)</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/a4341047-3fcb-4f86-ae08-d4795e4eee5a/image.png" /></p>
<p>swap in : 보조저장장치에 있던 게 다시 메모리에 불러와져서 실행 재개할 준비하는 과정
swap out : 주메모리에서 보조저장장치로 쫓겨나는 과정</p>
<p>ready queue에서 준비하고 있던 프로세스가 CPU에 올라가서 실행됨
I/O 장치가 필요하면 I/O wating queues로
swap out되면 다시 swap in 될때까지 보조저장장치에서 기다리게됨</p>
<p>I/O장치를 다 썼거나 swap in이 되면 다시 ready queue로 와서 CPU 할당을 기다림</p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/f24645b8-c46a-4b97-838d-396f86c2614b/image.png" /></p>
<p>CPU에서 쫓겨나는 경우는</p>
<ol>
<li>IO장치가 필요할때</li>
<li>time slice가 만료됐을때</li>
<li>fork했을때 </li>
</ol>
<ul>
<li><code>fork()</code>는 현재 실행 중인 프로세스를 그대로 복제하여 새로운 프로세스를 만드는 시스템 콜<ul>
<li><code>fork()</code> 자체가 CPU 반납을 의미하지는 않음</li>
<li>자식 프로세스 생성 이후 부모가 계속 실행될 수도 있고 스케줄러에 의해 부모/자식 중 하나가 선택될 수도 있음</li>
</ul>
</li>
</ul>
<p>4.interrupt가 발생했을때</p>
<ul>
<li>하드웨어 인터럽트(타이머, I/O 완료 등) 발생 시 현재 실행 중인 프로세스는 커널로 제어권을 빼앗김</li>
</ul>
<p>이러한 경우들은 다시 ready queue로 돌아가서 CPU 할당을 기다리게 됨</p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/41dd68c2-a41d-4691-bce5-1c93460bdc8a/image.png" /></p>
<ul>
<li>IO device(disk,terminal) 마다 waiting queue 존재</li>
</ul>