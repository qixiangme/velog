<h1 id="program-vs-process">Program vs Process</h1>
<p><strong>program</strong>
디스크에 존재하며 실행중이지않음</p>
<p><strong>process</strong>:
프로그램의 실행중인 인스턴스</p>
<ul>
<li>프로그램이 메모리에 로드되어 CPU에 의해 실행되기 시작하면 그것을 프로세스라고 부름</li>
<li>하나의 프로그램은 동시에 여러 개의 독립적인 프로세스로 실행될 수 있음. (예: 웹 브라우저를 여러 개 띄우는 경우)</li>
<li>이때, CPU 할당 및 관리의 기본 단위가 바로 프로세스</li>
<li>운영체제는 시스템 내에서 실행되는 각각의 프로세스를 프로세스 ID (PID)라는 고유한 번호로 구분하고 관리함</li>
</ul>
<h1 id="process-adress-space">Process Adress Space</h1>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/132d275e-015b-41e1-954b-c461a4599ace/image.png" /></p>
<p>하나의 프로그램이 프로세스로 메모리에 로딩되면, 그림과 같이 일반적으로 네 가지 주요 영역(세그먼트)인 Code (Text), Static Data (Data), Heap, Stack으로 구성된 프로세스 주소 공간을 갖게 됨</p>
<ul>
<li>PC는 는 현재 프로세스에서 다음에 실행할 명령어의 주소를 가리키며, 실행 흐름을 통제함</li>
<li>PID는 프로세스는 운영체제가 시스템을 관리하기 위한 단위. 프로세스를 식별하고 관리하는 데 필요한 정보를 메타데이터 형태로 가짐</li>
</ul>
<ul>
<li><p><strong>Code (Text) 영역:</strong> 프로그램의 실행 명령어가 저장되며, 일반적으로 읽기 전용 (Read-Only)으로 설정</p>
</li>
<li><p>*<em>Static Data (Data Segment) *</em>영역: 프로그램이 종료될 때까지 메모리에 유지되는 전역 변수나 정적 변수 (Static Variable)등이 저장 함수 단위의 생명주기와는 별개로 프로그램 실행 내내 유지.</p>
</li>
<li><p><strong>Heap 영역:</strong> 실행 중(런타임)에 개발자의 요청에 의해 동적으로 할당되고 해제되는 메모리 영역.(예: $\text{malloc}$이나 new 연산)</p>
</li>
<li><p><strong>Stack 영역:</strong> 함수 호출 시 생성되는 지역 변수나 매개 변수, 복귀 주소 등이 저장되며, SP (Stack Pointer)에 의해 관리되고 0xFFFFFFFF 주소 방향(보통 높은 주소)으로 자라남</p>
</li>
</ul>
<h1 id="process-state">Process State</h1>
<ul>
<li>프로세스 스케줄링은 하나의 CPU 코어에서 여러 프로세스가 동작할 때, CPU 자원의 낭비를 막기 위해 사용.</li>
<li>특정 프로세스가 입력/출력(I/O) 또는 디스크 읽기 같은 작업을 수행하며 $\text{CPU}$가 할 일이 없어지는 동안, $\text{CPU}$를 다른 프로세스에 할당하여 자원 활용률을 높임.</li>
<li>또한, 여러 프로세스가 동시에 실행되는 것처럼 보이게 하여 사용자 경험 (UX)을 향상시키는 목적도 존재.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/2eecb37c-1aff-4676-b0a9-4df5b827e35e/image.png" /></p>
<ul>
<li><p>프로세스 상태는 현재 프로세스가 CPU를 사용할 수 있는지의 논리적 상태</p>
</li>
<li><p><strong>New (생성) → Ready (준비):</strong> 새로운 프로세스가 생성되어 $\text{CPU}$를 할당받을 준비가 되면 Ready 상태로 이동합니다 (Admitted).</p>
</li>
<li><p><strong>Ready (준비) ↔ Running (실행):</strong> Ready 상태의 프로세스는 CPU 스케줄러에 의해 선택되면 Running 상태로 진입하여 $\text{CPU}$를 사용합니다 (Scheduler Dispatch).</p>
</li>
<li><p><strong>Running (실행) → Ready (준비):</strong></p>
<ul>
<li>프로세스가 시간 할당량 (Time Slice)을 모두 사용했으나 아직 작업이 끝나지 않은 경우, Trap을 통해 강제로 Ready 상태로 돌아가 다음 CPU 할당을 기다립니다.</li>
<li>이처럼 Running 상태가 너무 오래 지속되는 것을 방지하기 위해 시간을 쪼개어 사용합니다.</li>
</ul>
</li>
<li><p><strong>Running (실행) → Waiting (대기):</strong></p>
<ul>
<li>Running 상태의 프로세스가 I/O 요청이나 어떤 <strong>이벤트 대기</strong>와 같이 $\text{CPU}$를 사용하지 않는 작업을 필요로 할 경우, Waiting 상태로 전환됩니다.</li>
</ul>
</li>
<li><p><strong>Waiting (대기) → Ready (준비):</strong></p>
<ul>
<li>I/O 작업이나 기다리던 <strong>이벤트가 완료</strong>되면 $\text{CPU}$를 다시 사용할 수 있게 되므로 Ready 상태로 돌아가 CPU 할당을 기다립니다.</li>
</ul>
</li>
<li><p><strong>Running (실행) → Terminated (종료):</strong> 프로세스가 작업을 모두 마치거나 시스템 호출을 통해 종료되면 Terminated 상태로 이동합니다 (Exit).</p>
</li>
</ul>
<h1 id="process-control-block">Process Control Block</h1>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/7d30d282-0d57-4814-9e22-2d9da920591d/image.png" /></p>
<ul>
<li><p>프로세스 제어 블록 (PCB)은 운영체제가 시스템 내의 각 프로세스를 관리하기 위해 필요한 모든 정보를 담고 있는 데이터 구조.</p>
</li>
<li><p>운영체제는 이 $\text{PCB}$에 저장된 정보를 기반으로 프로세스 스케줄링을 수행하여 $\text{CPU}$를 할당.</p>
</li>
<li><p><strong>Process State (프로세스 상태):</strong> 프로세스가 현재 어떤 상태(예: New, Ready, Running, Waiting, Terminated)에 있는지 기록함.</p>
</li>
<li><p><strong>Program Counter (PC) 및 Registers:</strong> 다음에 실행할 명령어의 주소 (PC)와 프로세스가 사용하던 CPU 레지스터들의 값 등, 프로세스의 실행 문맥(Context)을 저장합니다. CPU 교체 시 이 정보를 통해 중단된 지점부터 정확히 다시 실행가능.</p>
</li>
<li><p><strong>CPU Scheduling Information:</strong> 프로세스의 우선순위, 스케줄링 큐에 대한 포인터 등 CPU 할당을 위한 정보를 포함</p>
</li>
<li><p><strong>Memory Management Information:</strong> 프로세스에게 할당된 메모리 영역의 경계(Memory Limits)와 같은 정보를 포함</p>
</li>
<li><p>$\text{PCB}$에는 해당 프로세스가 사용할 수 있는 Memory Limits 정보가 기록</p>
</li>
<li><p>사용자가 동적 할당 함수를 사용하여 메모리를 요청하는 프로그램을 작성하더라도, 이 Memory $\text{Limits}$에 도달하게 되면 더 이상의 할당이 불가능하여 프로그램이 정상적으로 동작하지 못하고 오류가 발생할 수 있습니다.</p>
</li>
<li><p><strong>List of Open Files</strong>: 프로세스가 현재 사용하고 있는 열린 파일들의 목록과 파일 포인터 등 관련 상태 정보가 PCB에 저장되어 프로세스가 파일 시스템과 상호작용하는데 사용.</p>
</li>
</ul>