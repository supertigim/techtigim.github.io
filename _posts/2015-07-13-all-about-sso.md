--- 
layout: post 
title: 플랫폼 개발을 위한 Single Sign On (SSO)  
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---  

SSO도 결국 스스로 보게 되는 현실... -_-;;  아는게 힘이라고 애써 긍정론을 펼칠뿐... 그냥 화가난다!! ㅎㅎ    

 1. Node.js 용 [LDAP 모듈](http://ldapjs.org/guide.html): 서버/클라이언트 생성 모두 가능한데, 잘 안된다는 글도 보이고 일단 심층 분석 필요  
 2. Redis 서버를 Session Manager 로 활용하는 [방법](http://dejanglozic.com/2014/10/07/sharing-micro-service-authentication-using-nginx-passport-and-redis/): Node.JS(정확히는 Passport와 Express의 세션)와 NginX를 짬뽕해서 잘도 만들었는데 효용가치는 잘 모르겠지만, 구조 이해하는데 도움 됨  
 3. Heroku에서 공개한 [SSO](https://blog.heroku.com/archives/2013/11/14/oauth-sso): MIT라이센스인게 제일 마음에 든다. 실제 상용 서비스잖아 ㅎㅎ 수작업으로 그려놓은 그림도 현실감 넘쳐~ 근데 이해가 잘 안된다. Basic knowledge의 부족!!! -_-;;   
 4.   [LemonLDAP](http://lemonldap-ng.org/start) ??: Perl로 구현된 Web SSO란다. Apache기반이래... 거기다 GPL라이센스?!! ㅋㅋㅋ  
 5. NginX LDAP 모듈 사용 [방법](https://calvin.me/nginx-ldap-http-authentication/): Reverse Proxy로 LDAP서버에 붙이는 방법? 정확히 용도는 모르겠다. Further research should be required to use this.  
 6. SSO [베이직](http://bcho.tistory.com/755): 블로거가 시스템 아키텍트인데 어지간히 도움됨. 찬찬히 다 읽어보면 좋음  


