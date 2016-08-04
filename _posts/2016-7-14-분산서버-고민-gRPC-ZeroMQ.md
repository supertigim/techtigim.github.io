--- 
layout: post  
title: 분산 시스템 고민  
tags: gRPC UDP ZeroMQ NetMQ pub/sub  
excerpt: 시스템, 모듈(드론)간 통신을 위해 생각한 내용 정리       
---  

드론 시스템 구성 때문에 여러가지 방식을 고민 해보았다. gRPC를 써보려고 했으나, 일단 무겁고, 임베디드에서 해보려니 약간 망설여졌다. 결국, 실시간 확보를 위해서는 udp외 대안이 없다고 생각하였고, pub/sub로 구성할 예정이다. 

gRPC설명한 한글 블로그  
[http://bcho.tistory.com/1013](http://bcho.tistory.com/1013)  

gRpc로 개발한 게임 프레임워크  
[https://github.com/gonet2/game](https://github.com/gonet2/game)  
    
고로 작성한 간단한 UDP 서버/클라이언트   
[https://github.com/jackyyf/isreversi](https://github.com/jackyyf/isreversi)  

ZeroMQ 기초 소개하는 공식 한글 홈페이지   
[http://kr.zeromq.org/intro:read-the-manual](http://kr.zeromq.org/intro:read-the-manual)    

ZeroMQ 설명 위키   
[http://www.potatogim.net/wiki/ZeroMQ](http://www.potatogim.net/wiki/ZeroMQ)  

고버전 ZeroMQ  
[https://github.com/pebbe/zmq4](https://github.com/pebbe/zmq4)  
  
C#에서 사용하는 라이브러리, NetMQ - XPub/XSub 설명
[http://netmq.readthedocs.io/en/latest/xpub-xsub/](http://netmq.readthedocs.io/en/latest/xpub-xsub/)