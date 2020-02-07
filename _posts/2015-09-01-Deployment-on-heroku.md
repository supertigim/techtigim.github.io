--- 
layout: post 
title: Heroku에 node.js 웹서버 올리기  
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: 로컬에서 깨작거리던 node.js를 heroku에 올려 보기로 했다.
---  

로컬에서 깨작거리던 node.js를 heroku에 올려 보기로 했다.  


----------


## Heroku  

혼자 노는 개발자의 희망이라고 할 수 있는 해외 웹호스팅이다. 간단하게 사용할 수 있는 웹서버를 Free로 제공하기 때문이다.  단, 곳곳이 유료로 가는 지뢰밭이라서 조심해서 설치하도록 하자. (https://www.heroku.com/home)  

히로쿠의 가장 큰 장점은 Dropbox, Github, CLI Tool을 이용한 서버 Deployment 기능이다. 이중에서 나는 CLI를 사용했는데, 이유는 보안상 중요한 시크릿키, 이메일 암호등을 깃이나 드랍박스에 올리기 거북했기 때문이다. [사이트](https://devcenter.heroku.com/articles/heroku-command)에 가면, 사용하는 OS별 인스톨러가 있기 때문에 다운 받으면 된다. 


----------


## MongoDB  

웹서버 처음과 끝인 DB를 heroku에서는 add-on형태의 서비스로 제공한다. 하지만, 막상 붙일려고 하면 카드정보를 넣어야 한다.  
![enter image description here](http://s3.amazonaws.com/info-mongodb-com/_com_assets/partners/mongolab-logo-onwhite-rgb-transparent-384x100_0.png)  
그래서 생각한 것이 mongodblab의 sandbox 무료 서비스였는데, 하루 써보고 그냥 접었다. 정말... 너무 느리다!! 느리고 느리고 느려도 그렇게 느릴 수가 없다. timeout으로 브라우저가 피를 토한다. :( 결국, 생각한 방법은 개인적으로 사용하는 서버에 mongdb를 설치해서 사용하는 것이었다.  
  
### 우분투에 설치  
  
설치는 간단하기에 [이런 블로그](http://zzaps.tistory.com/226)를 참고하면 되겠다.  
  
그런데 문제는 약간의 셋팅이 필요하다. 먼저 DB를 생성해야하고, 유저가 필요하다. 설치가 정상적으로 되었다면 다음 명령어로 local에서 접속 가능하다. 
  
    me@server~$ mongo 
    MongoDB shell version: 2.6.11
    connecting to: test
    >
  
> 나의 경우는 --dbpath가 없다고 나와서 설정해준 후에 접속 가능했다. 구글링하면 Fix하는 방법은 많이 나옴 
> Failed global initialization: BadValue Invalid or no user locale set. Please ensure LANG and/or LC_* environment variables are set correctly. Error의 경우에는 **export LC_ALL=C** 으로 해결 
  
### database와 user 생성  
  
database는 웹 서버 마다 생성해야하지만, user의 경우는 local에서 돌릴경우 사실 필요 없다. 어째든 둘다 db쉘에서 가능하다. 먼저 db를 만들자.  위에서 처럼 처음 접속하면 test db로 붙는데 아래 명령어로 바꾼다. 
  

    >use myapp (없으면 새로 생성 된다)
    switched to db myapp
    >  

user 만드는 것은 좀 더 까다롭다. 

    >db.createUser(){ 
    user: "user1",
    pwd: "1234",
    roles: [
    { role: "userAdmin", db: "mynewdb" } | "<role>",
    ...
    ]}  

이걸 입력해야 한다. 잘 생성되었다면, "add successfully"라고 뜨면서 "show users"로 확인 가능하다. 

### 외부 접근 설정  

외부에서 접근해서 보려하면, 계속 안될 것이다. ㅎㅎㅎ 두가지를 해결해야한다. 바로, configuration 파일의 bind_ip옵션과 firewall 셋팅이다. 

    me@server~$vi /etc/mongod.conf
    
     Listen to local interface only. Comment out to listen on all interfaces. 
     #bind_ip = 127.0.0.1

default 셋팅은 로컬에서만 접근 가능하도록 되어있다. 다음으로 firewall 설정이다. firewall을 안쓰고 있다면 물론 상관없으니 먼저 그것부터 확인하자. :)  
  
    me@server~$sudo ufw allow 27017  
  
### Robomongo 로 확인   
  
제일 확실하게 접속 여부를 체크할 수 있다. 새로운 conneciton을 생성하고 IP를 넣은 다음, authentication 탭에서 DB, user, password를 셋팅하면 된다.  
![enter image description here](https://cask.scotch.io/2014/03/mongodb-modulus-connect-with-robomongo-authentication.jpg)  
  


----------


## Augular fullstack web server 설정  
  
node로 뭘 하려고 한다면, 기본으로 사용하는 scafford인데 괜찮다. [사이트](https://github.com/DaftMonk/generator-angular-fullstack)에 가면 설치 방법은 나와있으니 살펴보면 도움 될 것이다.  
  
기본 설정은 local mongodb를 이용해서 local에서 서비스 하는 것이 때문에 몇가지 설정을 해주어야 한다.  

### mongodb 설정 
  
db path를 변경해야 한다.  server/config/environment에 가면, development.js와 production.js가 있는데 이곳에서 path 변경 가능하다. 이름 보면 용도는 알 것이라고 믿는다. -_-+  
  

    //'mongodb://localhost/myapp' <- 기존 내용 주석
    'mongodb://user1:1234@myserver.com:27017/myapp'  
   
제대로 설정 되었다면, **grunt serve**로 로컬에서 실행(development mode) 했을때, 정상적으로 웹페이지가 보인다. robomongo를 통해서 생성된 collection을 확인 할 수 있다.  

### heroku deployment  

웹 서버 폴더에서 아래의 명령어를 실행하면 **/dist** 폴더가 생긴다. 
  

    me@server~$grunt build  
  
시간이 조금 걸리는데, 압축하고 정리하는 것이 때문에 기다면 된다. 성공했다는 메시지를 봤으면 dist 폴더로 이동한다.  아! 그전에 heroku cli와 git이 설치되어 있어야한다. 그리고 아래의 명령을 입력하자.  
  
    me@server~$ heroku login

히로쿠에 가입한 이메일과 암호를 넣으면 된다. 정상적으로 되었다면 아래 명령어를 차례로 입력하자. 
  
    me@server~$ git init
    me@server~$ heroku git:remote -a myapp
  
로컬 리포지토리를 생성한거다.  
  

    me@server~$ git add .
    me@server~$ git commit -am "make it better"
    me@server~$ heroku create
    me@server~$ git push heroku master  
  
시간 꽤 걸린다. 정상적으로 올라갔다면, 메시지 중에 **myapp.heroku.com**으로 되었다고 나온다.   

브라우저에서 열리는 나의 웹 서비스를 보고 기뻐하자!!! That's it!!! :)  


  

  
