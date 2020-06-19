---
layout: post
title: "리눅스 인스턴스에서 PM2와 nginX를 통해 프로젝트를 배포해보자."
subtitle: ""
background: 'https://hackernoon.com/drafts/ar1wv331n.png'
---

<h2 class="section-heading">소개</h2>
<p>리눅스 인스턴스에서 PM2와 nginX를 이용해 프로젝트를 배포해보자.</p>

<h2 class="section-heading">1. 개념</h2>

<p>PM2는 자바스크립트 런타임인 Node.js의 프로세스 관리자로 우리가 만든 프로젝트가 리눅스 인스턴스에서 잘 작동할 수 있도록 해준다. 프로젝트를 실행해서 프로세스들이 생성되면, 프로세스 간 이벤트를 어떻게 처리할지, 오류나 메모리 문제가 발생했을 때 어떻게 처리할지 등을 관리해준다.</p>

<p>nginX는 웹서버의 일종인데 현재 가장 많이 사용되는 Apache보다 빠른 성능을 자랑한다.</p>

<h2 class="section-heading">2. 기본 설정</h2>

<p>GCP의 Compute Engine, 혹은 AWS EC2 등의 리눅스 인스턴스에 연결한다.</p>

<img class="img-fluid" src="../img/GoogleCloud1/4.jpg" alt="">

<p>먼저 Node.js를 설치한다.</p>
<pre>
<code>
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt-get install -y nodejs
</code>
</pre>

<p>yarn을 설치하기 위해 yarn의 저장소를 등록하고</p>
<pre>
<code>
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
</code>
</pre>

<p>yarn을 설치한다.</p>
<pre>
<code>
sudo apt update && sudo apt install yarn
</code>
</pre>

<p>다음은 프로젝트 관리자 PM2를 설치한다.</p>
<pre>
<code>
yarn global add pm2
</code>
</pre>

<p>nginX를 설치한다.</p>
<pre>
<code>
sudo apt install nginx
</code>
</pre>

<p>git을 설치한다.</p>
<pre>
<code>
sudo apt install git-all
</code>
</pre>

<h2 class="section-heading">3. 프로젝트 설정</h2>

<p>프로젝트를 불러온다.</p>
<pre>
<code>
git init
git clone {프로젝트 git repository 주소}
cd {프로젝트}
yarn install
</code>
</pre>

<p>nginX를 설정한다.</p>
<pre>
<code>
cd /etc/nginx/sites-available
rm -rf default
vi {프로젝트}.conf i
</code>
</pre>

<p>i를 눌러 insert 모드로 바꿔주고 아래의 코드를 입력한다.</p>
<script src="https://gist.github.com/chaehwanlim/58871c4293a6ceb8ac22f7a49a9845d3.js"></script>

<p>끝나면 아래의 커맨드로 저장해준다.</p>
<pre>
<code>
esc
:
wq!
</code>
</pre>

<p>nginx의 sites-enabled 설정에 똑같이 복사해준다.</p>
<pre>
<code>
cd /etc/nginx/sites-enabled
rm -rf default
sudo ln -s /etc/nginx/sites-available/[project].conf /etc/nginx/sites-enabled/[project].conf
</code>
</pre>

<h2 class="section-heading">4. 프로젝트 시작</h2>

<p>nginx와 pm2를 실행한다.</p>
<pre>
<code>
cd /home/{인스턴스 유저 네임}/{프로젝트}
systemctl stop nginx
systemctl start nginx
pm2 start server.js
</code>
</pre>

<p>혹시 오류가 발생한다면 아래의 커맨드로 오류 로그를 확인한다.</p>
<pre>
<code>
pm2 status
pm2 logs
</code>
</pre>
