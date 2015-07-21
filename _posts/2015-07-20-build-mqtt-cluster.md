--- 
layout: post 
title: (번역) IoT용 MQTT 클러스터 구축 -1/2편-
tags: iot mqtt node.js redis haproxy nscale mosca
---
메시징 브로커 클러스터 구축인데, 서버 Scaling out 관련 지식 쌓는데 유용할 것 같다.   ([출처](https://medium.com/@lelylan/how-to-build-an-high-availability-mqtt-cluster-for-the-internet-of-things-8011a06bd000))

## 개요  

IoT 관련 상용화 서비스에서 서비스 클러스터링 (or 확장성)은 간과할 수 없는 중요한 요소이다. MQTT를 활용하는 서비스에서는 서비스 사용 중에도 인프라 스트럭쳐의 업그레이드가 가능해야 하며, 지속적으로 안정적인 연결성을 제공해야 한다는 의미로 해석 가능하다.  

![enter image description here](https://d262ilb51hltx0.cloudfront.net/max/800/1*O1Gunc-vIbcJJ0prKIVHgw.png)

이렇게 시스템을 구축했을 때, 가장 큰 이득은 당연하겠지만 MQTT서버 중에 장애가 발생하여도 전체 서비스에는 문제가 생기지 않는다는 것이다. 즉 가용 서버를 통해서 서비스 제공이 지속적으로 가능하며, 클라이언트 사이드에 어떠한 문제도 유발하지 않게 된다.  

튜토리얼에서는 이 과정을 간단히 하고 싶은데, nscale을 사용하면, 2대의 MQTT서버를 배치하는데, 간단히 다음과 같은 명령어로 해결할 수 있다.  

    $ nsd cont buildall
    $ nsd rev dep headenter code here  


----------


## 사용하는 오픈 소스  

###[Mosca](https://github.com/mcollina/mosca)  

Node.js 모듈로 MQTT Server/Broker를 서비스 할 수 있다. MQTT 3.1과 호환되며, QoS 0 과 1을 지원한다. 오픈라인 packet/subscription을 위한 storage option도 같이 지원한다. 

*참고 국내 블로그: [Mosca를 사용한 MQTT연습](http://spectrumdig.blogspot.kr/2015/03/mosca-mqtt.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed:%20SpectrumLearnsToDig%20%28spectrum%20learns%20to%20dig%29)  

###[HAProxy](http://www.haproxy.org/)  

높은 가용성과 로드 밸런싱을 제공하는 TCP / HTTP기반의 솔루션이다. 많은 곳에서 사용중이며, 트래픽이 매우 많은 웹사이트에 적합하다.   

*참고 국내 블로그: [Haproxy, Keepalived, nginx 를 이용한 고가용성(High Availablity) 웹서비스 환경 구현](bryans.tistory.com/243)   

###[Docker](https://www.docker.com/)  

도커는 오픈 플랫폼으로 개발과 서비스 환경의 차이를 최소화하여 분산 솔루션의 빠른 개발, 빌드, 공유, 운영을 가능하게 한다. 즉, 동일 환경 하에 솔루션을 추가 배치가 쉽고 빠르게 되는 것이다. 보통 Docker는 VM 기술의 혁신으로 알려져 있지만, 실제적으로 Configuration Management 시장의 대안으로도 부상중이다. ([참고](http://thomason.io/2015-year-of-the-whale-and-other-disruptive-trends-in-it/))  

###[Nscale](http://nscale.nearform.com/)  

분산 시스템의 배치 관리를 쉽게 하는 오픈소스 툴이다. [microservice](https://en.wikipedia.org/wiki/Microservices) 기반 시스템에서 쉽게 활용할 수 이다.  국내 활용 예는 찾기 어렵다. -_-;;  


----------


##목차  

아래 순서로 진행할 것이다. 

 1. MQTT 서버 구축
 2. MQTT 서버를 Docker에 올리기 (Dockerization)
 3. HAProxy 추가 
 4. SSL로 MQTT 보안 설정
 5. 자동 deployment를 위한 nscale 설정
 6. 마무리  


----------


##MQTT Broker(서버) 구축  

MQTT는 아시다시피 M2M/IoT용을 개발된 프로토콜이다. 그래서 매우 가볍게 설계 되었으며, 결과적으로 네트워크 대역폭 사용에 있어 매우 큰 장점이 있다.  

![enter image description here](https://d262ilb51hltx0.cloudfront.net/max/1600/1*H2Xs_wmVT1eH-rjp3rKfyQ.png) 
 
많은 MQTT broker 가 있는데, Mosca는 보안이 좋으며, REST API지원이 좋다는 점과, node.js라는 경량 툴에서 개발 되기에 빠르게 개발할 수 있다. 물론, 표준 MQTT를 지원하기 때문에 좀 더 체계화된 Apache Kafka, Rabbit MQ로 마이그레이션도 후에 가능하다. ☺  

> 서버별 표준 지원 조금씩 틀린데, 필요에 맞게 선택하면 된다. 다음 [사이트](https://github.com/mqtt/mqtt.github.io/wiki/Server%20support)에서 화인 가능하다.  

이 튜토리얼에서는 모든 코드를 설명하지 않고, 필요한 최소한의 내용만 소개할 것이다. 프로젝트 전체 소스 코드는 [깃허브](https://github.com/lelylan/mqtt/)에서 확인가능하다.   

###MQTT Broker 셋팅  

아래의 코드로 서비는 구동된다. 먼저, Redis 서버를 사용하여 pub/sub를 설정하였다.  
  
  

    var server = require('./lib/server') , debug  = require('debug')('lelylan');
    
    var ascoltatore = {
	      type: 'redis',
	      redis: require('redis'),
	      db: 12,
	      port: 6379,
	      host: process.env.REDIS_HOST }
      , settings = {
	      port: process.env.NODE_PORT || 1883,
	      backend: ascoltatore };
	  
	var app = new server.start(settings);
	  
	app.on('published', function(packet, client) { 
		debug('ON PUBLISHED', packet.payload.toString(), 'on topic', packet.topic);
	});
	  
	app.on('ready', function() {
		debug('MQTT Server listening on port', process.env.NODE_PORT)
	});  

> 뜬금없이 Redis가 나와서 의아해 하겠지만, [FAQ](https://github.com/mcollina/mosca/wiki/FAQ-%28Frequently-asked-questions%29)에 잘 나와있다. (정말? -_-;;) 간단히 말해서, MQTT서버와 마이크로 서비스간의 통신 채널용으로 Redis를 사용한다.  

###클라이언트 Authentication   

Mosca로는 3가지 방식의 인증 방법이 있다. 인증이 되면, device_id가 client object에 저장되고, 후에 다른 용도로 사용 가능하다. (ex. authorization)  

    var mongoose = require('mongoose')
	    , debug    = require('debug')('lelylan')
	    , Device   = require('../app/models/devices/device');
	
	module.exports.authenticate = function(client, username, password, callback) {
		Device.findOne({ _id: username, secret: password }, function (err, doc) {
			if (doc) client.device_id = doc.id callback(null, doc);
		});
	}
	
	module.exports.authorizePublish = function(client, topic, payload, callback) {
		callback(null, client.device_id == topic.split('/')[1]);
	}
	
	module.exports.authorizeSubscribe = function(client, topic, callback) {
		callback(null, client.device_id == topic.split('/')[1]);
	}


----------


  
##MQTT 서버를 Docker에 올리기 (Dockerization)  
  
[Docker](https://www.docker.com/)는 상용 시스템 구축에 굉장히 훌륭한 기능을 제공한다. 최적 환경 설정에서 독립적으로 운영되는 서버를 [Dockerfile](https://docs.docker.com/reference/builder/)이라는 설치 가이드(recipe) 파일에 기록하여 빠르게 시스템을 운용(복사,복구,배치 등)할 수 있다. 
  
![enter image description here](https://d262ilb51hltx0.cloudfront.net/max/1600/1*7a8Qffxkg7WuePBZUebYSw.png)  
  
###Container 설정  

아래 처럼, Dockerfile을 만들자. 

    # node image
    FROM node:0.10-onbuild
    
    # docker mantainer
    MAINTAINER reggie
    
    # add the files to load
    ADD ./ .
    
    # install all needed packages
    RUN npm install
    
    # expose port
    EXPOSE 1883
    
    # execute app.js
    ENTRYPOINT ["node", "app.js"]

Dockerfile을 분석하면 다음과 같다. 사용하는 node.js의 버전(FROM node:0.10-onbuild)을 명시하고, 특별한 디렉토리 내의 모든 파일을 포함(ADD ./.), npm으로 dependency파일을 설치, Port 1883을 열고, 마지막으로 node.js를 실행한다. 이게 다다.  

###Docker Image 구동  
  
일단, Dockerfile을 만들었으면, docker container를 빌드할 수 있다. (물론, docker가 이미 설치 되어 있어야한다 -_-;; [참고](http://docs.docker.com/installation/#installation))  

    # building a container
    $ docker build -t lelylan/mqtt  
  
성공적으로 빌드되면 다음과 같이 출력된다.   

    Successfully built lelylan/mqtt

Docker Image 실행은 다음과 같다.  

    docker run -p 1883:1883 -d lelylan/mqtt  
  
이제 MQTT서버와 통신이 가능하다. 

> container와 image 차이가 궁금하다면, Docker사이트의 [container](https://docs.docker.com/articles/basics/)와 [image](https://docs.docker.com/articles/dockerfile_best-practices/)를 설명한 사이트를 참조하자. (링크가 다깨졌다 ㅋㅋ [한글블로그참조](http://kiros33.blog.me/130189000288))   

###많이 사용하는 Docker 명령어  

아래 커맨드는 기초적인 것이지만, 매우 유용하다. 참고하면 좋을 것이다. 

    ## Container related commands ##
    
    # build and run a container without a tag
    $ docker build .
    $ docker run -p 80:1883 <CONTAINER_ID>
    
    # build and run a container using a tag
    $ docker build -t <USERNAME>/<PROJECT_NAME>:<V1>
    $ docker run -p 80:1883 -d <USERNAME>/<PROJECT_NAME>:<V1>
    
    ## Image related commands ##
    
    # Run interactively into the image
    $ docker run -i <IMAGE_ID> /bin/bash
    
    # Run image with environment variables (place at the beginning)
    $ docker run -e "VAR=VAL" -p 80:1883 <IMAGE_ID>
    
    # list all running images
    $ docker ps
    
    # List all running and not running images
    # (useful to see also images that exited because of an error).
    $ docker ps -a
    
    ## Kill images ##
    
    # Kill all images
    docker ps -a -q | xargs docker rm -f
    
    ## Log related commands ##
    
    # See logs for a specific image
    docker logs <IMAGE_ID>
    
    # See logs using the tail mode
    docker logs -f <IMAGE_ID>enter code here  


----------


##HAProxy 추가  

여기서는 최고의 오프소스 로드밸런서인 HAProxy를 추가하는 방법을 알아보자. 참고로 이 솔루션은 C로 개발 되어, 빠르고 효율적이다.  

###용어  

HAProxy의 동작을 이해하기 위해서는 먼저 알고 있어야 하는 몇 가지 개념이 있다.  

> 자세히 알고 싶으면 Mitchell Anicas가 작성한 [아티클](https://www.digitalocean.com/community/tutorials/an-introduction-to-haproxy-and-load-balancing-concepts)에 자세히 잘 설명되어 있다. (윽! 이것만큼 길게... ㅠ.ㅠ)  
  
###Access Control List (ACL)  

ACL은 테스트 결과를 기반으로 접속 테스트와 action(서버 선택이나 request 블로킹 등)을 수행하는 용도로 사용한다. ACL로 패턴 매칭이나 백엔드 서버 구성에 따라 탄력적인 네트워크 운용이 가능하게 해준다.    

    # This ACL matches if the path of user’s request begins with /blog
    # (this would match a request of http://example.org/blog/entry-1)
    acl url_blog path_beg /blog

###Backend(백엔드)    
  
백엔드란, request를 수행할 수 있는 서버의 집단이다. 이론적으로 서버수가 증가할수록 로드가 분산되어 전체 서비스의 안정성과 용량이 증가한다. 예로, 80포트를 통해 접속을 받는 두대의 설정한다면 다음과 같다.  

    backend web-backend
	    balance roundrobin
	    server web1 web1.example.org:80 check
	    server web2 web2.example.org:80 check

###Frontend(프론트엔드)  

프론트엔드는 어떻게 request를 백엔드로 넘길 것인지를 정의하나다. 내용은 HAProxy 설정의 frontend 섹션에 넣는다. 물론, IP어드레스, ACL,  백엔드도 같이 입력한다. 아래 예를 보면, 유저가 example.com/blog요청하면, 내용은 블로그 백엔드 전달된다. 여기에는 블로그를 운영하는 서버 군이 있다.  다른 요청은 web-backend로 전달된다. 

    frontend http
	    bind *:80
	    mode http
	    
	    acl url_blog path_beg /blog
	    use_backend blog-backend if url_blog
	    
	    default_backend web-backend

여기서 사용하는 설정은 Dockerfile와 요청 처리를 기술한 설정 파일에 정의되어있다.   

> 본 튜토리얼의 HAProxy 설정은 [깃허브](https://github.com/lelylan/haproxy-mqtt)에서 참고 가능

###Docker Hub에서 [HAProxy  container](https://registry.hub.docker.com/_/haproxy/) 얻기   

우선, Docker Hub Regstry에서 컨테이너를 받는다.  

    $ docker pull dockerfile/haproxy  

수정 없이, 실행하려면 다음과 같이 한다.  

    $ docker run -d -p 80:80 dockerfile/haproxy  

HAProxy 컨테이너는 데이터 볼륨 옵션으로 설정 파일을 셋팅할 수 있다. 여기서 <override-dir>은 절대 경로로 haproxy.cfg와 errors/ 디렉토리가 있는 곳이다.  

    # Run HAProxy image with a custom configuration file
    $ docker run -d -p 1883:1883 \
	    -v <override-dir>:/haproxy-override dockerfile/haproxy

###HAProxy configuration  

다음의 설정은 MQTT서버 동작을 정의한다. 포트 1883으로 오는 모든 요청은 leastconn balance 모드에 따라 2대의 MQTT 서버(mosca1, mosca2)로 전달된다.  

    # Listen to all MQTT requests (port 1883)
    listen mqtt
    
	    # MQTT binding to port 1883
	    bind *:1883
	    
	    # communication mode (MQTT works on top of TCP)
	    mode tcp
	    option tcplog
	    
	    # balance mode (to choose which MQTT server to use)
	    balance leastconn
	    
	    # MQTT server 1
	    server mosca_1 178.62.122.204:1883 check
	    
	    # MQTT server 2
	    server mosca_2 178.62.104.172:1883 checkenter code here

> ACL, Backend, Frontend 설명 해놓고선 내용이 없다. ㅋㅋ 그렇게 한 설정에 문제가 있었단다. :)

새로운 설정을 적용해 보자. (개발에서는 괜찮다. 제대로 할꺼면 새로 만들고...) 다음 예제에서, /root/haproxy-override/에 있는 설정 파일을 오버라이드해서 구동하였다. 

    $ docker run -d -p 80:80 1883:1883 \
	    -v /root/haproxy-override:/haproxy-override
	    dockerfile/haproxyenter code here

일단, 동작되는 설정을 셋팅하였다면, 그것으로 새로운 HAProxy container를 만들수 있다. 필요한 것은 컨테이너를 로딩할 수 있는 Dockerfile이다. 여기서, /etc/haproxy/haproxy.cfg의 내용을 변경하고, 포트(80/443/1883/8883)를 설정하여 **재시작**을 하면된다. 

    # Container to start from with a working HAProxy definition
    FROM dockerfile/haproxy
    MAINTAINER Andrea Reginato <andrea.reginato@gmail.com>
    
    # Add personalized configuration
    ADD haproxy.cfg /etc/haproxy/haproxy.cfg
    
    # Add restart commands
    ADD restart.bash /haproxy-restart
    
    # Define working directory
    WORKDIR /etc/haproxy
    
    # Define default command
    CMD ["bash", "/haproxy-start"]
    
    # Expose ports
    EXPOSE 80
    EXPOSE 1883

> 주의해야 할 것은 이미 실행되고 있다면, 그냥 start를 하는 것이 아니라 **restart**를 해야 한다. 그래야 변경된 설정이 적용된다.   

###그외 꿀팁  

HAProxy에서 문제가 발생하면 Log를 살펴보자. 우분투에서 기본으로 사용하는 rsyslog를 통해서 확인 가능하다. 

    # HAProxy log configuration file
    $ vi /etc/rsyslog.d/haproxy.conf
    
    # Files where you can find the HAProxy logs
    $ tail -f /var/lib/haproxy/dev/log
    $ tail -f /var/log/haproxy.log


----------


너무 내용이 길어서 1/2편으로 나누어서 작성한다.  
  
to be continued....








