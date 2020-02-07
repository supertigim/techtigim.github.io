--- 
layout: post 
title: Instant Messenger & MQTT 
tags: technology  
class: post-template
subclass: 'post tag-technology' 
author: tigim 
---  

화려한 Unified Communication 관련 기술이 없나 알아보다가 결국은 기본으로 돌아왔다. 경량 메시징 프로토콜을 기반으로 해야 다음(?)으로 넘어갈 수 있는 확장성이 보장된다고 할까~  

### MQTT란?
설명은 대략 [Joinc.co.kr](http://www.joinc.co.kr/modules/moniwiki/wiki.php/man/12/MQTT/Tutorial)에 잘 되어 있다.  ([일본어 사이트](http://tdoc.info/blog/2014/01/27/mqtt.html)인데 변역해서 보면 더 잘되어있음)
![enter image description here](https://docs.google.com/drawings/d/17jmOyeJk_7FI7A8cyd37Dfx0i9mik0mP2dUxwrXDeU4/pub?w=863&h=406)
대략 가장 큰 장점은 프로토콜 자체가 매우 가벼워서IoT에서 빛을 볼 가능성이 높다는 것이다. 거기에 Facebook이 메신저 앱에서 사용했다고 해서 더 유명해졌다. 예전에는 XMPP라고 하는 XML기반의 메시징 프로토콜이 있었는데, 기반 기술의 무거움 때문에 덩달아 매우 무겁다. 하지만, 여전히 많이 쓰고 있어서 별 생각이 없다면 [OpenFire/Spark](http://www.igniterealtime.org/) 조합 통해서 사용하면 될 것 같다.  
![enter image description here](http://www.igniterealtime.org/fans/logo-spark.png)

### MQTT Broker(서버)
위에서 말했다 시피 MQTT는프로토콜이기 때문에 관련 시스템(서버)를 구축해야 한다. [node.js](https://github.com/mqttjs/MQTT.js/wiki/client)로 간단히 만들어도 되지만, 이미 괜찮은 [오픈소스 브로커 서버](https://github.com/mqtt/mqtt.github.io/wiki/server-support)가 존재하기에 가져다 쓰면 되겠다. 페이스북은 mosquitto놈을 썼고, 나는 rabbitMQ라는 놈에 관심이 많다. "모기"가 더 가볍게 사용 가능하고, [어떤 글](http://www.slideshare.net/ultrasonic/android-push-server-mqtt)(정확히는 SlideShare인데 꼭 보길!!)을 보니 성능도 우수하여 서버 한대로 20,000 접속까지 동작한다고 되어있는데, 무겁기는 하지만 확장하기 편한 후자를 선택하는 합리적인 판단으로 보인다. 그리고, [Presence 기능을 지원](http://www.rabbitmq.com/blog/2010/10/19/exchange-to-exchange-bindings/)한다는 것이 가장 큰 이유이다.   

### RabbitMQ  
![enter image description here](http://blog.jonharrington.org/content/images/2015/03/2013-11-02-comms_overview.png)  
며칠 검색 결과 최종 후보로 선택이 되었다. 소개나 자세한 글은 [홈페이지](https://www.rabbitmq.com/)를 통해서 보면 될 것이다. 영문이라서 불편하면, [여기서](http://airlee00.blogspot.kr/2014/01/1-jms-rabbitmq.html) 확인해도 좋다. node.js로 개발한 간단한 [채팅 프로그램(with node.js)](http://rocksea.tistory.com/284)을 따라 만들면 좀 더 감이 올 것이다.   

그런데 한가지 찜찜한 부분이 있다. 구글링하다가 [발견](http://stackoverflow.com/questions/23158842/using-rabbitmq-in-android-for-chat)했는데, Android에서 클라이언트 배터리가 광탈을 한다는 것이다. -_-;;  현재로는 뭐 딱히 대안도 없고, 그런 경험의 글도 하나 밖에 없어서 무시하는 중이다. ㅎㅎ

### Paho  
![enter image description here](http://www.eclipse.org/paho/images/paho_logo_400.png)  
MQTT 프로토콜을 지원 클라이언트 오픈소스다. 내용은 [홈페이지](http://www.eclipse.org/paho/#getting-started)를 참고하자. 다른 것은 모르겠고, 지원하는 언어가 많아서 좋다. 모바일도 지원이 된다. (iOS는 없지만, 이미 [여기](https://github.com/jmesnil/MQTTExample) [저기](https://github.com/mobile-web-messaging/MQTTKit)서 작업을 해두었다)  

### 참고  

mosquitto auth 추가 방법 : http://my-classes.com/2015/02/05/acl-mosquitto-mqtt-broker-auth-plugin/   

mosquitto + python : http://blog.jonharrington.org/displaying-mqtt-messages-in-a-browser-in-real-time/  
mosquitto + paho 조합 : http://blog.naver.com/bestdriver94/220174180761   

rabbitMQ + paho + Go 로 tutorial : http://blog.wnotes.net/blog/article/rabbitmq-paho-mqtt-beginng  

또 다른 rabbitMQ + node.js 예제 : http://codezip.tistory.com/633  

Micro-Service Fun – Node.js + Messaging + Clustering Combo :
http://dejanglozic.com/2014/04/21/micro-service-fun-node-js-messaging-clustering-combo/  