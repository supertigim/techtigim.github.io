--- 
layout: post 
title: 오픈 소스 API Gateway 솔루션   
tags: api_gateway api kong api_umbrella tyk microservices
---  

Microservices 아키텍처의 Core라고 할 수 있는 API Gateway에 대해 조사해 보자.   

## Trend  

원래 API(Application Program Interface)는 글자 그대로 어플리케이션(서비스)를 사용하기 위한 인터페이스이다. 하지만, 다양한 플랫폼(iOS, 안드로이드 등)의 출현과 서비스 간의 쉬운 메쉬업의 니즈로 요즘 개발되는 대부분의 서비스는 내부 기능의 API화를 중요한 과제로 생각하고 있다.  
  
또한, 시스템 아키텍처라고 하면, 한 때는 ESB(Enterprise Service Bus) 및 SOA (Service Oriented Architecture)가 대세 였으나, 현재는 Microservices라는 사상이 인기를 끌면서 각각의 작은 서비스를 독립적으로 개발하고 이를 향후에 통합하는 방향으로 설계를 하고 있다.  

![enter image description here](http://martinfowler.com/articles/microservices/images/decentralised-data.png)  

![enter image description here](http://cfile8.uf.tistory.com/image/27374E4C53FDD48E358EF6)

결과적으로 동작이나 형태, 구성등은 예나 지금이나 유사하지만, 철학과 개발 환경이 변했다고 볼 수 있다. 


----------


## Microservices란?  

다음의 상황이 필요하다면, 마이크로서비스를 염두 해 두자.  

 - 현재 시스템의 변경이 어렵다. 
 - 새로운 기술과 Mesh-up이 필요하나, 현재 개발 시스템과 호환에 문제가 있다. ([Polyglot 프로그래밍](http://blog.naver.com/code1st/220136904869)) 
 -  부서 간 이기주의로 전체 시스템의 통합이 불가 : 다른 시스템 간의 연계성을 최소화 하고, 현 개발 시스템에 집중을 하고 싶다. 
 - 서비스의 통합/제거/융합/확장 용이성이 절실하다.  

기술적으로는,  

 - 오픈소스
 - REST/JSON
 - Agile 사상
 - 클라우드 환경    

등을 활용할 수 있는 장점이 있다.  


----------


## API Gateway  

결국, 마이크로서비스 및  서비스 API([Restful 형태](http://blog.remotty.com/blog/2014/01/28/lets-study-rest/))화가 대세가 된 시장 환경에 맞추어 API Gateway는 자연스레 통합을 위한 Core로 인식 되었다.   

![enter image description here](http://www.developer.com/imagesvr_ce/8186/OpenSource2.png)

### 필요 기능  

 - 유저별 API별 인증/인가 
 - 대표 URL (예, api.myservice.com)에서 내부 API Resources(서버) 요청 라우팅 
 - 로드 밸런싱  
 - Logging : API 사용 오류 분석 및 Scalability, Metering에 이용  
 - QoS 조정 (Throttling or Rate Limiting) 
 - API Transformation : 요청 API를 내부 사용에 알맞은 형태로 변환 or vice versa  


----------

## 오픈소스 API Gateway 후보 3선

어짜피 위의 대대분의 얘기는 구글링으로 찾을 수 있다. 중요한 것은 여기!! ㅎㅎ 며칠 간의 각고의 노력으로 쓸만한 후보를 찾았다. (유료 및 국내 사이트에서 본 정체 불 분명한 것들은 제외)  

 - [KONG](http://getkong.org/) 
 - [API Umbrella](http://apiumbrella.io/) 
 - [tyk](https://tyk.io/)  

### 1. KONG  

![enter image description here](http://getkong.org/assets/images/homepage/intro-illustration.png)

[mashape](https://www.mashape.com/)라는 회사에서 개발 및 사용하는 솔루션 

**특징**  

 - NGINX + Cassandra 조합으로 [lua Script](http://hueji.tistory.com/69)로 프로그래밍  
 - MIT 라이센스   
 - Docker 지원 
 - Plug-in 형태의 기능 지원 

**한줄 평**  

지인의 추천으로 처음부터 살펴본 솔루션으로 관심을 굉장히 많이 받고 있다. (GitHub에서 150이상 Fork) 또한, 업데이트도 매일 되고 있어서 향후 더 발전 가능성이 높다고 볼 수 있다. 

### 2. API Umbrella   

![enter image description here](http://kinlane-productions.s3.amazonaws.com/screen-capture-api/nrel-github-ioapi-umbrella.png)  

미국 정부(일개?)에서 사용하다고 되어 있으며, 따로 개발 및 관리 주체가 없는 것으로 보임  

**특징**   

 - NGINX + node.js + [varnish](http://egloos.zum.com/repository/v/5849937) + MongoDB + Redis + [Elastic Search](http://d2.naver.com/helloworld/273788) 조합이며, Ruby on Rails로 개발한 포털 제공
 - MIT 라이센스  

![enter image description here](http://apiumbrella.io/images/docs/router@2x-026d1c87.png)

**한줄 평**  

업데이트 간격이 매우 길기 때문 향후 지원에서 약간 의문이 남는다. (어디다 정부 프로젝트 개떡?! -_-;;) 하지만, 미국 정부에서 사용 중이고 Minor fix는 계속 이루지고 있으므로, 크리 걱정할 필요하는 없을 것으로 보인다. 관련 기술에 익숙하다면 쉽게 사용도 가능해 보인다.  

### 3. tyk.io   

![enter image description here](https://tyk.io/assets/img/app-bg-3.png)

아시다시피 가장 나중에 올리는 것이 개인적으로 제일 관심있고 밀고 있는 기술이다. ㅎㅎ

**특징**  

 - MongoDB + Redis + NGINX 조합으로 **[golang](http://www.badayak.com/3469)**으로 프로그래밍
 - MPL v2.0 라이센스 
 - API Documentation을 위한 Swagger 지원
 - 사용자 및 API Key 인증/권한 관리가 매우 좋음 
 - [Docker 지원](https://tyk.io/blog/tyk-1-6-released/)

**한줄 평**  

다른 서비스와 달리 구글의 최신 프로그래밍 언어 golang으로 메인 Functionality가 개발 되었다는 것이 굉장히 매력적이다. (개발자 입장에서? ㅎㅎ) 시간당 업데이트가 되고 있으며, 사용자 요청에 대한 반영이 적극적으로 향후 발전 가능성이 굉장히 높다. 

----------


## 참고  

**마이크로서비스 소개**  

 - http://skccblog.tistory.com/2317 : SK Planet에서 발표한 자료  
 - http://bcho.tistory.com/948  

**기타**  

 - http://bcho.tistory.com/808 : Swagger 소개 

