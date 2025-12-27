<h1 id="operating-system-structure">Operating System Structure</h1>
<p>운영체제가 어떻게 구성되어있고 연결되어있는지에 대한 것</p>
<h3 id="monolithic-structure">Monolithic structure</h3>
<p>모놀리틱 커널
<img alt="" src="https://velog.velcdn.com/images/qixiangme/post/3cf4bcb7-3f86-4070-b7a5-bb91da105770/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/aeb8cf4f-bb0a-4dcd-91e5-f6aca2ba49fc/image.png" /></p>
<p>커널에 다양한 기능이 포함됨
H.W와 user programs 사이 OS kernel이 모든 서비스를 다룸 file system, process control 등등</p>
<ul>
<li>메모리 용량 차지하는 게 많음</li>
<li>다른 곳에서 호출안하고 커널안에 다 들어가니까 빠름</li>
<li>종속성이 많이 복잡하게 묶여있음 -&gt; 유지보수 힘듦</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/cf162944-dc1e-40d1-b280-7b6791b4557d/image.png" /></p>
<h5 id="layered-approach">Layered approach</h5>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/c4eb8dbb-fd00-47cb-b51e-edd57e1fcd6e/image.png" /></p>
<ol>
<li>유지보수를 위해 각각의 기능별로 Layer을 정해서     </li>
<li>윗레이어로 갈때는 한번에 한 단계만 갈 수 있게 하자는 주장 나옴(Layered approach)</li>
<li>그러나 명확하게 Layerd approach로 분리할수없다는 한계가 존재했음</li>
</ol>
<h3 id="microkernel-structure">Microkernel Structure</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/f367eb73-bae5-4ce2-b866-9a059743ef39/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/06d20a23-872d-4ccc-8012-cf572c114c31/image.png" /></p>
<p>protection처럼 핵심적인 기능만 커널에 저장하고 그 외는 따로 저장함</p>
<ul>
<li>부가적인건 user Mode에 올려놓음</li>
<li>메모리 용량 가벼움</li>
<li>유지보수 쉬움</li>
<li>user &lt;-&gt; 커널 분리되어있기때문에 호출하느라 오래 걸림</li>
</ul>
<h3 id="modular-approach">Modular approach</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/913c7963-6acd-49c8-8a86-77d7334ab72e/image.png" /></p>
<ul>
<li>모놀리틱 스트럭쳐에서, 쓸모없는 게 하드디스크 뿐만아니라 메모리에도 올라가는 게 문제다</li>
<li>따라서 커널에 다 포함 시키되, 메모리에는 선택적으로 올리자는 주장.</li>
<li>Linux, Solaris, etc...</li>
</ul>
<h3 id="hybrid-approach">Hybrid approach</h3>
<p><img alt="" src="https://velog.velcdn.com/images/qixiangme/post/43c1fc19-eff4-48da-bdb9-cf991b59c2fa/image.png" /></p>
<ul>
<li>마이크로의 철학을 따름.</li>
<li>기존 3protection 같은 핵심적인 부분뿐만 아니라 fileSystem처럼 자주 쓰이는 것도 안에 넣자는 주장</li>
<li>Mac OS X, Windows, iOS, Android</li>
</ul>