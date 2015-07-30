--- 
layout: post 
title: SSO와 oAuth 2.0 
tags: sso oauth2.0 oauth1.0 saml jwt ldap
---  

시스템 Integration을 하는데 있어 먼저 필요한 것 Single Sign On이어서 관련하여 조사하여 보았다.  

## Trend   

시장 트렌드는 JWT, oAuth2.0이다. SAML과 oAuth는 올드한 느낌이 있지만, 현재도 사용하고 있기는 하다. (이런 정의가 맞는지조차 모름. 이 분야는 약간 생소해서... -_-;;)   

그리고, LDAP을 활용한 oAuth 2.0 지원 인증 시스템이나 오픈 소스를 찾아보았지만, 최근 1~2년 동안에는 인기가 좀 줄어든게 아닌가 생각된다. 굳이 LDAP을 써야할 이유도 많이 줄어 들었고... (LDAP에 불 필요한 요소가 너무 많다?) 


## 참고 튜토리얼  

node.js 를 활용한 예제가 많은데 그 중에서 눈여겨 볼만한 내용을 정리  

 - [Build an API Service With Oauth2 Authentication, Using Restify and Stormpath](Build%20an%20API%20Service%20With%20Oauth2%20Authentication,%20Using%20Restify%20and%20Stormpath)
 - [Building a RESTful API With Node - OAuth2 Server](http://scottksmith.com/blog/2014/07/02/beer-locker-building-a-restful-api-with-node-oauth2-server/)
 - [oauth2-restapi-server](https://vinebrancho.wordpress.com/2014/05/19/oauth2-restapi-server-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EC%95%B1%EC%9D%84-oauth2-0%EC%9C%BC%EB%A1%9C-%EC%A2%80-%EB%8D%94-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C/) (한국 사이트)
 - Node.js Module for oAuth 2.0 : [Satellizer](https://github.com/sahat/satellizer)
 - [Passport를 활용한 사용자 인증 설명](http://bcho.tistory.com/920) (한국 사이트) 
 - [full stack angular](https://github.com/DaftMonk/generator-angular-fullstack) : 일명 MEAN Stack으로 불리는 놈 중의 하나인데, 유저 인증에 대해 상당히 잘해 두었다. 코드 상에서 실제 동작 모습에 대해서 자세히 파악 할 수 있고, 응용도 가능해 보임. **강추!!!!!**  


