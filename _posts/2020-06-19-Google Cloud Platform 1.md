---
layout: post
title: "GCP의 Compute Engine을 이용해보자"
subtitle: ""
background: ''
---

<h2 class="section-heading">소개</h2>

<p>GCP의 Compute Engine을 이용해보자</p>

<h2 class="section-heading">1. 기본 설정</h2>

<p>https://cloud.google.com 에 접속해 "무료로 시작하기"를 눌러 결제 카드 등록 등 초기 설정을 해준 뒤 프로젝트를 생성한다.</p>

<img class="img-fluid" src="../img/GoogleCloud1/1.jpg" alt="">
<p>기다리면 Compute Engine 생성이 완성되고 인스턴스 만들기를 누른다.</p>

<img class="img-fluid" src="../img/GoogleCloud1/2.jpg" alt="">
<p>인스턴스 초기 설정을 한다.</p>

<p>https://cloud.google.com/free/docs/gcp-free-tier?hl=ko (구글 클라우드 무료 등급)</p>

<p>위 링크에 따르면 us-west1(오리건), us-central1(아이오와), us-east1(사우스캐롤라이나) 지역을 선택해야 무료로 f1-micro를 사용할 수 있다고 한다. 따라서 세 지역 중 한 지역을 선택해준다.</p>

<p>무료로 사용하기 위해 머신 유형을 f1-micro로 설정한다.</p>

<p>방화벽도 설정해준다.</p>

<p>참고로 우측은 월 4달러씩 부과될 예정이라고 나오지만 실제로 f1-micro로 사용해보면 요금이 전혀 부과되지 않는다.</p>


<img class="img-fluid" src="../img/GoogleCloud1/3.jpg" alt="">
<p>만들고 나서 잠시 기다리면 인스턴스가 완성된다. 브라우저 창에서 열기를 누른다.</p>

<img class="img-fluid" src="../img/GoogleCloud1/4.jpg" alt="">
<p>잠시 뒤에 인스턴스에 연결된 터미널 창이 생성된다.</p>

<p>이전에 AWS EC2를 사용했을 땐 PuTTY를 설치하여 사용했었는데 GCP는 바로 브라우저로 접근이 가능해서 더 편리한 느낌을 받았다.</p>

<h2 class="section-heading">2. IP 고정</h2>

<img class="img-fluid" src="../img/GoogleCloud1/5.jpg" alt="">
<p>인스턴스 메뉴에서 "네트워크 세부정보 보기"를 누른다.</p>

<img class="img-fluid" src="../img/GoogleCloud1/6.jpg" alt="">
<p>외부 IP 주소에서 IP 주소를 고정으로 바꿔준다.</p>

<p>참고로 고정 주소를 예약하고 인스턴스와 연결해두지 않으면 요금이 오히려 부과된다고 한다.</p>
<p>이 부분은 AWS의 정책과 비슷한 것 같다.</p>

<h2 class="section-heading">3. 방화벽 규칙 만들기</h2>
<p>2번에서처럼 네트워크 세부정보에 접속하고 좌측의 방화벽에 접속하자.</p>

<img class="img-fluid" src="../img/GoogleCloud1/7.png" alt="">
<p>방화벽 규칙 만들기를 누르자.</p>

<img class="img-fluid" src="../img/GoogleCloud1/8.png" alt="">
<p>이름을 적당히 만들어주고 아래로 스크롤해 대상, 소스필터, 소스IP범위, 프로토콜 및 포트를 수정해준다.</p>

<p>트래픽방향 수신, 송신 총 두 번의 규칙을 만들어주어야 한다.</p>

<p>지금은 간단한 토이프로젝트라서 모든 포트의 접속을 허용하는 취약한 정책을 설정했지만 실제로 서비스되는 프로젝트에서는 절대 그래선 안 될 것 같다.</p>
