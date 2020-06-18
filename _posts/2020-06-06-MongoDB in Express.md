---
layout: post
title: "Express에서 MongoDB 데이터를 다루는 REST API를 구축해보자."
subtitle: ""
background: ''
---

<h2 class="section-heading">소개</h2>

<p>Express에서 MongoDB 데이터를 다루는 REST API를 작성해보자.</p>

<h2 class="section-heading">1. 개념</h2>

<p>MongoDB는 도큐먼트 지향 DB시스템이며, 관계형 구조가 아니므로 NoSQL로 분류된다. JSON과 같은 동적 스키마형 도큐먼트를 다루는 데 주로 사용된다.</p>

<p>Mongoose는 Node.js 환경에서 MongoDB를 다룰 수 있게 해 주는 라이브러리이다.

<h2 class="section-heading">2. 기본 설정</h2>

<pre><code>
yarn add express body-parser mongoose
</code></pre>

<pre><code>
/server.js
</code></pre>

<script src="https://gist.github.com/chaehwanlim/587b8f998f72fd44f45d18f1f49ae8d4.js"></script>

<p>만들어둔 DB에 접근하는 데 사용하는 스트링은 secret.json에 따로 저장해두기 때문에 먼저 해당 파일을 불러온다.</p>

<p>mongoose를 선언해주고 스트링을 이용해 연결, 라우터도 연결해준다.</p>

<h2 class="section-heading">3. 스키마 정의</h2>

<pre><code>
/models/bill.js
</code></pre>

<p>MongoDB는 동적 스키마형이라 아무 데이터나 넣어도 에러가 발생하지 않기 때문에 자료형 혼동 방지를 위해 Mongoose에서 도입한 스키마를 설정해야 합니다.

Mongoose는 데이터를 실제로 DB에 넣기 전에 작성된 스키마를 기준으로 데이터를 검사한다고 합니다. 우선 타입을 정의하고 기본값도 설정해줄 수 있습니다. </p>

<script src="https://gist.github.com/chaehwanlim/e93a73d211ada61f9785ede6007f1c5e.js"></script>

<p>Schema의 생성자의 두 번째 인자로 넘겨준 versionKey 객체는 새로 생성할 때 _vid 항목의 생성을 방지하기 위해 넣습니다.</p>

<p>스키마를 설정하면 mongoose의 model함수를 통해 export 해줍니다. model 함수의 첫 번째 인자로 스키마의 이름, 두 번째 인자로 만들어 두었던 스키마 객체, 세 번째 인자로는 구축해두었던 실제 MongoDB에서의 컬렉션 이름을 넣습니다.</p>

<h2 class="section-heading">4. REST API 작성</h2>

<pre><code>
/router/bills.js
</code></pre>

<script src="https://gist.github.com/chaehwanlim/09bb2c1417bbceb3c3a030319a19a776.js"></script>

<p>방금 정의한 모델을 불러옵니다.</p>

<script src="https://gist.github.com/chaehwanlim/8478ff17cf3ac0adf5a6fd01292b706f.js"></script>

<p>첫 번째로 특정 기업의 모든 계산서를 받아오는 get을 작성합니다.</p>

<p>불러온 모델 Bill의 find 함수를 이용합니다.
첫 번째 인자는 [filter] 기능을 담당하므로 어떤 스키마(속성)을 가진 도큐먼트(예시: 계산서)를 찾을 것인지에 대한 정보를 객체로 전달합니다. 
두 번째 인자는 [projection] 기능, 즉 찾은 도큐먼트의 어떤 스키마를 가져올지 담당하므로 역시 해당 정보를 객체로 전달합니다. 예시에서는 모든 스키마를 불러올 것이므로 빈 객체를 전달합니다.</p>

<p>그리고 find 함수 뒤에 sort 함수를 사용하여 찾은 도큐먼트를 정렬합니다. 인자로 객체를 전달하며 오름차순은 1, 내림차순은 0 값을 넣어줍니다.</p>

<script src="https://gist.github.com/chaehwanlim/644746f7de3f3dd71617740504d3a680.js"></script>

<p>두 번째로 계산서를 추가하는 post를 작성합니다.</p>

<p>먼저 모델 Bill의 생성자를 이용해 bill 인스턴스를 만들어줍니다.
body에 담긴 데이터로 bill의 데이터를 설정해줍니다.
bill의 save함수를 이용해 저장합니다.
불러온 모델 Bill의 find 함수를 이용합니다.</p>

<script src="https://gist.github.com/chaehwanlim/d9777a3b3200688359ee7029ed1cdc2b.js"></script>

<p>세 번째로 계산서를 수정하는 put을 작성합니다.</p>

<p>모델 Bill의 updateOne 함수를 이용합니다.
첫 번째 인자는 [filter] 기능을 담당하므로 어떤 속성을 가진 도큐먼트를 수정할 것인지에 대한 정보를 객체로 전달합니다.
두 번째 인자는 [update] 기능을 담당하므로 수정할 내용을 객체로 전달합니다.</p>

<script src="https://gist.github.com/chaehwanlim/dcd8288bd344e3ae50d1fdf11ed70fd3.js"></script>

<p>네 번째로 계산서를 삭제하는 delete를 작성합니다.</p>

<p>이번 프로젝트에서는 실제로 도큐먼트를 삭제하지 않고 isDeleted 속성을 1로 수정할 것이므로 updateOne 함수를 이용합니다.
put과 동일하게 작성합니다.</p>

<p>끝으로 라우터를 export 해줍니다.</p>

<pre><code>
module.exports = router;
</code></pre>