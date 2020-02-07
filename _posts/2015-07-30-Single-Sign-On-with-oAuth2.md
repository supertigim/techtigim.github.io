--- 
layout: post 
title: SSO와 oAuth 
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---  

플랫폼 개발에 있어 기본이 되는 기능 중의 하나인 인증 관리에 대해 대표적인 표준화 기술인 oAuth 중심으로 살펴보고, 관련 오픈소스도 살펴 보자.   


----------

## Trend  

요즘 소프트웨어 개발의 핵심은 Open API이다. 말 그대로 오픈이 되었다는 것은 누구나 이용가능하다는 의미이지만, 시스템 안정성이나 보안 측면에서 제어는 필요하다.   
  
이를 위해 준비된 표준이 바로 oAuth이다. 요즘은 개념을 확대하여 Single Sign On을 위한 기반 기술로도 활용된다.   
  
----------
## oAuth 란?  

한줄로 표현 하자면 다음과 같다.   

>   **API 사용을 위해 Authentication + Authorization을 통합하여 제공하는 표준**  

얼핏 보면 SSO 또는 ID관리와 거리가 있어 보이지만, 요즘 시스템 내부 동작이 API 호출의 연속이라는 관점에서 보면, 결국 동일한 기능인 것이다.  

### 동작 원리   
  
간단히 설명하면 다음과 같다. 사용자가 시스템에 로그인 하면, 해당 ID가 인증서버에 의해 Verification이 되고, 이 인증을 값을 내부의 서비스(서버)들이 공유하면서 사용자의 접근에 대해 제어를 한다.  

![enter image description here](http://cfile30.uf.tistory.com/image/240E1C3E524A97530E1893)  

위 그림에서는 트위터 API(인증서버)가 인증을 처리한 후에 인증 토큰을 Consumer(서비스)에 전달하여 사용자가 이용할 수 있게 하였다.  여기에는 큰 장점이 있는데, 바로 개별 서비스 이용을 위해 사용자 패스워드 계속 보낼 필요가 없다는 것이다. 인증 토큰으로 사용자가 확인 되기 때문에 패스워드 노출이 거의 발생하지 않는다는 보안상 이점이 존재한다. 

> 인증 토큰은 주로 HTTP 헤더에 정의되어 XML또는 JSON형태로 전달  

좀더 자세한 Flow는 다음과 같다.  

![enter image description here](http://1.bp.blogspot.com/-UQXNggXW5ro/VXPjYNZtM-I/AAAAAAAADMI/02U4ZNMB4eo/s640/Screen+Shot+2015-06-06+at+11.22.44+PM.png)  
  
### oAuth 1.0a vs. oAuth 2.0

현재 버전 2.0까지 나왔으며, 시장에서는 1.0a도 함께 사용하는 중이다.  

![enter image description here](http://cfile2.uf.tistory.com/image/257D4836524AA1EA07342B)   

사실 문외한 입장에서 보면, 둘의 차이점을 명확히 알 수가 없는데 간단히 2.0의 개선점을 설명하면 다음과 같다.  

**1. 간단해졌다.**  

OAuth 1.0a에서는 signature라는 부분이 가장 큰 부분을 차지했고, 실제 테스트를 하기 위해서는 별도의 API를 사용해야만 했다. 하지만, OAuth 2.0에서는 Bearer(JWT Bearer) 토큰 인증 방식을 쓰면서 더 이상 signature가 필요 없어졌고, 그로 인해 만들기도 테스트하기도 편해졌다.  

> JWT(Json Web Token)은 JSON을 암호화 한 것이다.   

실제로 코드(node.js)를 보면 더욱 이해가 쉽다. 서버의 secret(단순 문자열)을 이용해서 json(아이디와 role)을 암호화한 후, 브라우저에게 cookie형태로 보낸다.  Secret으로 암호화 되어있어 해독이 어렵고 HTTPS(SSL 보안 채널)이용하기 때문에 통신 중에 누출될 가능성도 낮다. 

	/**
        * Returns a jwt token signed by the app secret
        */
        function signToken(id) {
        return jwt.sign({ _id: id }, config.secrets.session, {expiresInMinutes: 60*5 });
        }
        
        /**
        * Set token cookie directly for oAuth strategies
        * */
        function setTokenCookie(req, res) {
	        if (!req.user) return res.json(404, { message: 'Something went wrong, please try again.'});
		        var token = signToken(req.user._id, req.user.role);
		        res.cookie('token', JSON.stringify(token));
		        res.redirect('/');
		 }  

**2.크로스 플랫폼 지원이 가능해졌다**  

여러 인증 방식을 제공함으로써 시나리오, 플랫폼 별로 맞게 대응할 수 있게 되었다. 모바일 뿐만 아니라 TV나 게임 콘솔 등 oAuth1.0a에서 쉽지 않은 클라이언트 환경에서도 인증이 가능하게 되었다.  

아래의 4개 인증 방식이 스펙에 나와 있으며, 이중 상위 두개를 가장 많이 이용한다.  (그외에도 추가 가능성을 열어두었다.)

- Authorization Code (or Web Server) Flow  
  
웹 서버에서 API를 호출하는 등의 시나리오에서 Confidential Client가 사용하는 방식이다. 서버사이드 코드가 필요한 인증 방식이며 인증 과정에서 client_secret 이 필요하다.
로그인시에 페이지 URL에 response_type=code 라고 넘긴다.  

![enter image description here](http://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/images/oauth/oauth_web_server_flow.png)  

- Implicit Grant (or User Agent) Flow   
  
token과 scope에 대한 스펙 등은 다르지만 OAuth 1.0a과 가장 비슷한 인증방식이다. Public Client인 브라우저 기반의 어플리케이션(Javascript application)이나 모바일 어플리케이션에서 이 방식을 사용하면 된다. Client 증명서를 사용할 필요가 없으며 실제로 OAuth 2.0에서 가장 많이 사용되는 방식이다. 로그인시에 페이지 URL에 response_type=token 라고 넘긴다.  

![enter image description here](http://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/images/oauth/oauth_user_agent_flow.png)  

- Resource Owner Password Credentials Flow  
  
이 방식은 2-legged 방식의 인증이다. Client에 아이디/패스워드를 저장해 놓고 아이디/패스워드로 직접 access token을 받아오는 방식이다. Client 를 믿을 수 없을 때에는 사용하기에 위험하기 때문에 API 서비스의 공식 어플리케이션이나 믿을 수 있는 Client에 한해서만 사용하는 것을 추천한다.
로그인시에 API에 POST로 grant_type=password 라고 넘긴다.    

![enter image description here](http://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/images/oauth/oauth_username_password_flow.png)

- Client Credentials Grant Flow   
  
어플리케이션 이 Confidential Client일 때 id와 secret을 가지고 인증하는 방식이다. 로그인시에 API에 POST로 grant_type=client_credentials 라고 넘긴다.  

![enter image description here](http://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/images/oauth/oauth_client_credentials_flow.png)
  
**3. 서비스의 Scalability를 확보했다.**   

리소스 서버와 인증 서버의 분리 및 다중화가 스펙에 표시 되었다. Auth 2.0에서는 스펙 상에서는 인증 부분을 확실하게 분리해 놓으므로써, 인증 서버의 확장이 쉽게 가능하게 되었다.  이런 확장성을 바탕으로 Facebook이나 Google에서 로그인을 SaaS형태로 지원 할 수 있게 되었다. 


----------


## 오픈소스  

여러 가지 오픈소스가 있는데 대표적인 4가지를 소개한다.  

### OpenAM   
  

![enter image description here](https://forgerock.org/app/uploads/2014/09/FR_AM.png)  
  
원조격의 Solution으로 보면 된다. 2005녀도 부터 시작 되었고, OpenDJ(LDAP서버) 연계등이 처음부터 고려되어 구성 잘 되어있다. 하지만, 제품이 무겁고 튜닝을 위해서는 높은 학습이 필요하다는 단점이 존재한다.  

 - https://forgerock.org/openam/
 - https://forgerock.org/tag/openam/
 - CDDL-1.0 라이센스  

### Spring Security   
  
![enter image description here](http://www.intelligrape.com/blog/wp-content/uploads/2013/10/spring_security_login.jpg)

 Spring관련 개발을 한다면 같이 쓰면 편할 것이다. Spring 관련한 환경 구축이 필요해서 익숙치 않은 개발자에게는 진입 장벽이 존재한다.  

 - http://projects.spring.io/spring-security/
 - 아파치 라이센스  

### KeyCloak  
  
![enter image description here](http://3.bp.blogspot.com/-z5tefHLQ0qw/VOcNQ7QyLoI/AAAAAAAASn8/dPusM-qeTxo/s1600/keycloak.png)

RedHat(JBoss) 계열의 제품이다. JBoss진영에서 하던 유사 프로젝트, PicketLink와 2015년 3월에 합병되었다. JBoss 관련 기술에 적응이 되었다면 쉽게 시작 가능하다.  

 - http://keycloak.jboss.org/
 - 공식적으로 도커 지원
 - 아파치 라이센스   

### Gluu  
  
![enter image description here](http://www.gluu.org/wp-content/uploads/2014/06/Gluu-Server-Stack_no_jagger.jpg)  

2009년 부터 시작한 프로젝트로 Sun의 OpenSSO(현재 OpenAM)으로 개발이 되었다가,  Shibboleth IDP를 사용하는 등 많은 변화가 있었고 현재는 oAuth2.0을 지원하다. 

 - http://www.gluu.org/
 - MIT 라이센스

----------


## 용어 정리  

용어가 전문적이라서 알아보기 어려운데 아래 내용으로 참고  

![enter image description here](http://cfile10.uf.tistory.com/image/1156D8364D0ABF5602900A)  


----------


## 참고  

 - [Build an API Service With Oauth2 Authentication, Using Restify and Stormpath](Build%20an%20API%20Service%20With%20Oauth2%20Authentication,%20Using%20Restify%20and%20Stormpath)
 - [Building a RESTful API With Node - OAuth2 Server](http://scottksmith.com/blog/2014/07/02/beer-locker-building-a-restful-api-with-node-oauth2-server/)
 - [oauth2-restapi-server](https://vinebrancho.wordpress.com/2014/05/19/oauth2-restapi-server-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EC%95%B1%EC%9D%84-oauth2-0%EC%9C%BC%EB%A1%9C-%EC%A2%80-%EB%8D%94-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C/) (한국 사이트)
 - Node.js Module for oAuth 2.0 : [Satellizer](https://github.com/sahat/satellizer)
 - [Passport를 활용한 사용자 인증 설명](http://bcho.tistory.com/920) (한국 사이트) 
 - [full stack angular](https://github.com/DaftMonk/generator-angular-fullstack) : 일명 MEAN Stack으로 불리는 놈 중의 하나인데, 유저 인증에 대해 상당히 잘해 두었다. 코드 상에서 실제 동작 모습에 대해서 자세히 파악 할 수 있고, 응용도 가능해 보임. **강추!!!!!**  
 - http://stackoverflow.com/questions/27917004/spring-secuirty-oauth-2-multiple-user-authentication-services  
 - Implementing OAuth2 with Spring Security: https://raymondhlee.wordpress.com/2014/12/21/implementing-oauth2-with-spring-security/
 - PDF 파일 : http://www.hsc.com/Portals/0/Uploads/Articles/WP_Securing_RESTful_WebServices_OAuth2635406646412464000.pdf  
 - GitHub에 있는 Angular 에서 인증 받기: https://github.com/dsyer/spring-security-angular/tree/master/oauth2-vanilla  
 - 국내 사이트 : http://jjeong.tistory.com/589  
 - 국내 사이트 : 책내용 거들먹 : http://tmondev.blog.me/220305603654  
 - 국내 사이트 : 읽어 볼만함 : http://linuxism.tistory.com/1545  
 - oAuth.io Open Source : https://github.com/oauth-io/oauthd  
 - 가장 최근 블로그 : http://anwyesan.blogspot.kr/2015/06/restfull-authorization-service-with.html  
 - PDF 파일 : https://qconnewyork.com/ny2012/dl/qcon-newyork-2012/slides/robert-winch-springsecurity-qconny.pdf  
 - 국내 사이트 : http://goodcodes.tistory.com/entry/Spring-01-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%9D%B8%EC%A6%9D
 - Node.js 로 구축 설명: http://jlabusch.github.io/oauth2-server/
 - 국내 사이트 : KTH (정리 잘됨) : http://www.slideshare.net/tebica/oauth2-api


