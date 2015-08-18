---  
layout: post  
title: Nginx에 HTTPS 연결 설정  
tags: nginx https ssl reverse_proxy  
excerpt: reverse proxy로 사용을 많이하는 nginx에서 https 설정에 대해 알아본다.  
---  
  
reverse proxy로 사용을 많이하는 nginx에서 https 설정에 대해 알아본다.  
  
원래, 웹서버에 HTTPS를 설정을 하려 했으나, 요즘 대부분이 nginx를 endpoint로 설정(마치 로드밸러서 처럼)해서 쓰고 때문에 이 방법을 정리 해보았다.  
  
## HTTPS는?  
  
일반적으로 포트 443번을 이용하는 대표적인 인터넷 연결로 연결에 대한 보안 기능을 제공한다. 일반 HTTP(80포트)를 사용해서 로그인 등의 보안 정보를 전송할 경우, 해킹의 위험이 굉장히 높기 때문에 대안 기술로 생각하면 된다.  
  
### SSL (Secure Socket Layer)   
  
HTTPS는 정확히 HTTP over SSL이다. 즉, SSL을 이용한 프로토콜이다. SSL은 예전에 유명했던 netscape라는 회사에서 개발한것으로 간편하면서도 보안이 뛰어나 대부분의 인터넷 회사에서 사용한다. 요즘 oAuth2.0의 경우는 default로 사용하도록 되어있다.  
  
기본적인 동작은 다음과 같다.  
  
 - 공개키 방식으로 대칭키를 전달 후 data 를 주고 받을 때에는 대칭키 방식으로 암호화 해서 전달
 - 대칭키 노출의 위험이 없고, computing 비용도 줄일 수 있음  
  
![enter image description here](http://cfile4.uf.tistory.com/image/251EA237559F6AD70B717C)  
  
실제 HTTPS에서는 다음과 같은 Flow로 동작한다.  
   
 - Client 가 Server 에 접속 요청
 - Server 는 인증서를 Client 에게 전달
 - 인증기관(CA)의 공개키(브라우저 내장)로 인증서를 검증. Server 의 공개키를 획득
 - Client 는 Server 의 공개키로 대칭키를 암호화 하여 Server 에 전달
 - Server 는 자신의 개인키로 복호화 하여 대칭키를 획득  
    
![enter image description here](http://cfile7.uf.tistory.com/image/2548C837559F8B6F18F066)  
  
### SSL Certificate & Private key
  
이런 SSL을 하기 위해서는 Pair로 Certificate와 Private key가 필요하다. 이를 이용해서 public key를 만들고 클라이언트에 제공하여, HTTPS프로토콜이 동작할 수 있게 된다.  인증서와 개인키는 [OpenSSL](http://blog.naver.com/oxcow119/220449926380)등을 이용하여 테스트용으로 간단히 만들 수 있기는 하지만, 실제 상용화 서비스를 위해서 CA(Certificate Authority)를 통해서 발급을 받아야 하고 비용도 꽤 든다.  
  
  
### 암호화 알고리즘  
  
실제 보안을 제공하는 것은 인증서가 아니라 암호화 알고리즘이다. 인증서는 자격증에 불과 하므로, 적용시에는 어떻게 알고리즘을 적용 할 것인지 신경 써야한다. 알기 어려운 내용이 많으므로, 모질라 재단에서 [추천](https://wiki.mozilla.org/Security/Server_Side_TLS)하는 방식을  참고해서 웹서버 설정을 하면 된다. 단, IE6.0같은 오래된 브라우저에서는 지원되지 않는 알고리즘이 있다는 것을 염두해 두자.  
  
  
----------
  
## 인증서 받기     
  
조금이라도 조사해 보면 알겠지만, 인증서 가격이 만만치 않다. 결국, 대안은 두개로 좁혀 지는데, https://www.startssl.com/ 와 https://www.gogetssl.com 이다. 외국 사이트라 거부감이 들긴 하지만, 많이 싸다. :) 블로그를 통해서 조사해 보니 주로 gogetssl에서 3년 사용 가능한 인증서를 많이 구입한다.  가격은 만원 중반 대로 저렴하다. (국내는 10만원 대)  
  
구입 과정은 좀 길고 복잡해서 다음 블로그에서 참고하자. http://i-swear.com/922  
  
대략적인 과정은,
  
> 상품선택 → 결제 → CSR(인증서 서명 요청) 업로드 → (아래 설명 참조) → 인증에 사용할 메일주소 선택 → (기다리세요) 인증메일 도착 → 링크 클릭 또는 인증코드 입력 → (기다리세요) 인증서 발급완료 통지 → 첨부파일 또는 구입처 웹사이트에서 인증서 다운로드
  
이고 실제로 해보면 오래 걸리지 않는다.  비용만 나갈뿐~~ :)  
  
> CSR 작성을 할때는 몇가지 주의가 필요하다. 우선, 사용하려는 도메인(서브 도메인까지, **www**.abc.com, **blog**.abc.com) 명을 정확히 기입해야 한다. 서명 방식에 대해서도 주의가 필요하다. SHA1은 1년짜리 인증서에만 사용하고, 2~3년짜리에 사용하면 크롬에서 에러가 발생한다. 반면, 윈도우 XP SP2 이하에서는 SHA256 인증서를 이용한 서버에 접속할 없다. 이 내용은 사실 가져온 것이라서 꼭 구입 전에 자세히 확인 하자.  
  
  
### 인증서 합치기  
  
저렴한 인증서 발급 업체(startssl, gogetssl)를 이용 하면, 여러 개의 인증서가 달려온다. 웹 서버에 상요을 위해서는 chain 인증서를 만들어야 하는데, **순서를 잘 지켜줘야 한다**. 아래의 예는 gogetssl에서 발급 받은 인증서의 예이다.  
  
    cat yourdomaim_com.crt COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt AddTrustExternalCARoot.crt > yourdomain_com.pem  
  
pem 파일을 열어보면, 대단하건 없다. 순서대로 인증서 갖다 붙힌거다.  
  
  
----------
## nginx 설정  
  
준비물은 위에서 준비한 체인 인증와 private key 이다. nginx와 연결된 웹서버는 설정이 되었다고 가정한다.  
  
### 인증서 복사  
  
다음의 위치로 인증서를 복사한다.  
  
    sudo mv ./mydomain_com.* /etc/nginx/ssl
  
개인키와 인증서에 대한 보안은 매우 중요하므로, **실제 개발시에 다른 위치에 두고 소유자는 root, 퍼미션은 600**으로 해준다.  
  
### 암호화 알고리즘  
   
내용은 입맛에 맞게 변경하면 된다. 아래의 내용은 참조에서 가져온 것이다.  
  
  
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    
    ssl_prefer_server_ciphers on;  
  
### 설정 파일 전체  
  
시나리오는 다음과 같다. https://www.mydomain.com으로 외부에서 접근하여 localhost:3000으로 라우팅 된다.  
  
     # HTTPS server
     #
     
     server {
        listen       443;
        server_name  www.mydomain.com;
        
        ssl     on;
        ssl_certificate /etc/nginx/ssl/mydomain_com.pem;
        ssl_certificate_key /etc/nginx/ssl/mydomain_com.key;
        
        # Option
        # ssl_session_timeout  5m;
        # ssl_protocols <위 참조>;
        # ssl_ciphers <위 참조>;
        # ssl_prefer_server_ciphers   on;
        
        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
        
        location /public {
            root /usr/local/var/www/;
        }
    }
  
### 마무리  
      
nginx 서버를 재실행 해주면 끝난다. 외부에서 브라우저로 접근 해보면, 꿈에도 그리는 녹색 자물쇠를 보게 될 것이다. :)
  
    sudo service nginx restart
  
  
----------
  

## 참고  
  
 - http://childeye.tistory.com/354  
 - http://blog.naver.com/mogni/70184198676  
 - http://doogle.blog.me/220432826348  
 - [SSL 서버 테스트](https://www.ssllabs.com/ssltest/) 
 - https://wiki.kldp.org/HOWTO/html/SSL-Certificates-HOWTO/x70.html  