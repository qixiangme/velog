<h1 id="os-service">OS service</h1>
<p>운영체제가 제공해주는 서비스
CPU I/O MEMORY 장치들을 관리해주고 사용할 수 있게 제공하는 서비스들</p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/590c64be-7471-4022-acec-f3fa38fafcf3/image.png" /></p>
<p><strong>0.UI</strong>
GUI,CLI 등으로 유저에게 보여줄수있는 화면제공</p>
<p><strong>1.System calls</strong> 
커널에 직접 접근하면 공격에 취약함
그래서 커널은 User부분과 System부분이 분리되어있는데
이 두 부분을 연결해주는게 system calls</p>
<p><strong>2.program execution</strong> :
program을 실행할때, 함수마다 메모리를 어디부터 어디까지 어떻게 써야하는지 정해야함.
보통 프로그램이 시작되면 main부터 시작하는데 이때 메모리할당을 커널이 해줌.</p>
<p><strong>3.I/O operation</strong>:
input을 입력받고 output을 보여줌.
프로그램은 위에서부터 아래로 실행되지만 입력은 예측불가능.
어플리케이션이 이런 I/O를 직접다루게 되면 오류 많아짐.
app -&gt; System -&gt; I/O 순으로 입출력주고받음.</p>
<p>ex) 정수값을 기대하는 cin에 String을 넣음
-&gt; 원래라면 system에 맛이 가서 작동조차안하지만, error 뜨는 걸로 끝남</p>
<p><strong>4.file systems</strong>:
한글파일이 문서폴더에 있다면, 컴퓨터는 구조화를 해야 그 파일을 찾을 수 있음.
한글파일은 어떤 폴더에 있고 그안엔 뭐가 있고 등등등을 구조화.
즉, 파일에 대한 데이터 &amp; 파일이 어디에 있는 지도 기술해줘야함
파일시스템은 각각의 표준에 따라 어떻게 구조화되는지 명시함. 
OS에 기본적으로 탑재되어 파일을 구조화해줌</p>
<p><strong>5.communication</strong>:
application 끼리는 메모리 공유가 되지않음(memory protection)
program 끼리 데이터 주고받고 싶다 -&gt; 전역변수, 공유 메모리를 통해 공유하면 되지만
application끼리 데이터를 주고받고 싶다 -&gt; communication을 통해 공유</p>
<p><strong>6.resource allocation</strong>
시스템을 사용할때 얼마만큼의 CPU 얼마만큼의 Memory를 OS에서 결정해줌</p>
<p><strong>7.accounting</strong>
시스템 관리자 열면 현재 CPU 사용량,GPU 돌아가고있는 프로그램등등이 보임
현재 상황 이런 정보를 관리 제공해주는 것(OS)</p>
<p><strong>8.error detetction</strong>
프로그램에 Error -&gt; 이 프로그램을 죽일지 말지 결정을 해줌 </p>
<p><strong>9.protection and security</strong>
컴터 키고 계정에 따라 로그인하는 것  </p>
<h1 id="system-call">System call</h1>
<p>system call이 user과 kernel을 어떻게 연결시켜주는 지에 대해</p>
<h3 id="function-call">Function Call</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/8e668d9a-d3c8-4f41-9efd-d2338721fe6b/image.png" /></p>
<p>1.<code>read()</code>
user영역에서 system에게 파일을 읽고싶다 명령하는것. 
커널영역에서 그 파일을 읽어줌. </p>
<p>2.<code>trap handler</code></p>
<p>운영체제에서 예외가 발생하면 처리하는 방식이 3개
interrupt : 시스템 잠깐 멈춰
trap: 시스템에게 뭐 좀 해줘
exception : 분모를 0으로 나눠 -&gt; ???, 그때 나오는 것</p>
<p>read()로 명령을 받았으니 trap handler가 작동</p>
<p>3.<code>read() kernel routine</code>
커널에서 <code>read()</code> 명령 수행 후 결과를 usermode에게 전달</p>
<p>user에서 kernel로 바로 접근 하는 것이 아니라 system call을 통해 요청후 받아옴</p>
<h3 id="handling-in-os">Handling in OS</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/0ec4b1f2-24df-4db1-ba4c-aeff248497a9/image.png" /></p>
<p>1.<code>open()</code>
유저 영역에서 오픈을 호출. 이때의 메모리 점프 위치가 커널영역에 있기에 System call 필요</p>
<p>2.레지스터에 저장
CPU 레지스터에 시스템콜 번호를 넣고 시스템콜 실행
그럼 CPU가 커널 모드로 전환되어 해당 넘버를 보고 맞는 시스템콜을 수행함.</p>
<p>커널은 <code>open()</code> 시스템콜을 확인하고 파일을 열며, 그 결과 값(fd)을 다시 레지스터에 넣음.
사용자 모드로 돌아오면 프로그램은 이 레지스터 값을 읽어 파일을 다룸.</p>
<h3 id="parameter-passing">parameter passing</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/96b0c889-0141-40fe-939c-2a4b765c45a3/image.png" /></p>
<p>시스템 호출 시 파라미터는 CPU 레지스터에 저장되어 커널 영역으로 전달되며, 커널은 이 파라미터를 받아 실행.</p>
<p>파라미터의 개수가 많아 레지스터만으로 전달하기 어려울 경우, 파라미터들은 주로 스택 메모리에 저장</p>
<p>스택 메모리에 파라미터가 너무 많이 쌓일 경우 시스템 메모리의 다른 영역을 침범할 수 있으므로, 기본 규약으로 최대 4개 정도의 파라미터만 레지스터로 직접 전달하는 경우가 일반적</p>