--- 
layout: post 
title: Meteor 간단 소개  
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: coursera에 있는 meteor 강의를 보고 스터디 해보다 
---  

전 부터 meteor에 관심이 많았는데, 우연하게 coursera에 들어갔다가 발견한 강의를 따라서 간단히 스터디 해 보았다.  

## How to install Meteor   
  
미티어는 node.js 기반으로 동작 된다. 설치는 공식 홈페이지 찾아보면 될텐데, 그것보다는 최신 버전 업데이트를 계속 깜빡한다. 
  
    sudo npm cache clean -f
    sudo npm install -g n
    sudo n stable
  
node.js가 작년 하반기 부터에서 격변을 맞이하고 있기 때문에 항상 최신 버전으로 업데이트해서 사용하는 것이 좋다.  

meteor 업데이트는 간단하다.  공식 홈페이지에서 리눅스/맥 설치 보면 나온거 따라하면 된다. 이전 버전도 지워주고 알아서 새버전으로 설치한다.   
  
    curl https://install.meteor.com/ | sh

정상적으로 설치가 되었다면, 버전 확인을 할 수 있다.  

    meteor -v  
  

> meteor는 몽고DB도 필요하다. 나의 맥북에는 이미 설치가 되어 문제가 없는 것으로 보이는데 해보고 문제가 있다면 설치 해두도록 하자. 

  
## Hello world  

프로젝트 생성은, 

    mkdir helloworld
    meteor create helloworld
    
실행은,  

    meteor  
  
ㅎㅎ 쉬워서 머... 할게 별로 없다. 느낌은 node.js에 무언가 두툼이 얹어서 묵직한 기분이 든다.  정상적으로 서버가 동작하면, 브라우져에서  localhost/3000로 접속 가능하다.  

> 위 시작 프로젝트는 DB에 저장할 내용이 없지만, 후에 프로젝트를 확장해 나가면 몽고DB를 따로 살펴보아야 할 일이 생긴다. 이럴 때는 **robomongo**로 포트 **3001**으로 접속해서 확인 할 수 있다.  
  
