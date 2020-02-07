--- 
layout: post  
title: 분산 메시지 브로커 Kafka    
tags: kafka docker python c++       
excerpt: 카프카 설치 및 간단한 예제            
---  

카프카를 알게 된지 수년이 지났는데, 드디어 써보게 되었다. 물류 시스템에 들어가는 멀티 로봇 제어 시스템에서 쓸 예정이다. 알다시피 2-3 대의 소수 로봇 제어에서는 이런 독립 메시징 시스템 없이도 간단히 가능하다. 그런데, 원대한 시스템을 꿈꾸는 이가 있어서 구색을 맞춰주기 위해 한번 검토 해보았다.  
  
여기의 모든 예제와 설명은 우분투 16.04 기준으로 작성되었다.  

Docker  
=======  

요즘 왠만한 툴/시스템은 도커 이미지도 제공한다. OS에 맞춰 설치 하지 않아도 되는건 크나큰 장점이 아닐수 없다. 물론 도커는 설치해야 하지만... ㅎㅎㅎ
  
```    
    $ sudo apt-get remove docker docker-engine docker.io  
    $ sudo apt-get update && sudo apt-get install apt-transport-https ca-certificates curl software-properties-common  
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    $ sudo apt-get update && sudo apt-cache search docker-ce

    docker-ce - Docker: the open-source application container engine

    $ sudo apt-get update && sudo apt-get install docker-ce docker-compose
    $ sudo usermod -aG docker $USER
```  

카프카 실행  
========  

깃허브에서 오픈소스 가져오는거 비슷하게 카프카 도커 이미지를 가져온다. 이건 도커 시스템 어딘가에 저장되기 때문에 어디서 실행하든 상관없다.    

```
    $ docker pull wurstmeister/kafka
```

다음으로 간단히 실행할 수 있게 정리해 둔 소스코드(설정/스크립트)를 가져온다.    

```
    $ git clone https://github.com/wurstmeister/kafka-docker.git
    $ cd kafka-docker 
``` 

연습으로 로컬에서 구동하는 것이기에 docker-dompose-single-broker.yml에서 KAFKA_ADVERTISED_HOST_NAME를 **127.0.0.1**롤 변경 해준다.  

```  
    # 실행 
    $ docker-compose -f docker-compose-single-broker.yml up

    # 정지 (다른창에서)
    $ docker-compose stop   
```  

이걸로 구동완료!! 로그를 보고 있자면, 뭔지는 모르겠으나 주키퍼와 카프카가 사이좋게 메시지 뽑아내면서 잘 동작한다고 나온다.  

테스트   
=====  

개발 언어로 연습해 보기 전에 카프카 바이너리 릴리즈에 있는 스크립트로 동작을 확인 할 수 있다.  

먼전 지금 구동 중인 카프카/스칼라 버전을 확인한다. Dockerfile 파일을 열어보면 다음과 같이 나오는데, 

    ARG kafka_version=2.2.0  
    ARG scala_version=2.12  

이 경우, **kafka_2.12-2.2.0.tgz**를 파일을 [http://archive.apache.org/dist/kafka/](http://archive.apache.org/dist/kafka/)에 가서 받으면된다.  

압축을 해제하고, 

```  
    $ tar xzvf kafka_2.12-0.10.2.0.tgz  
    $ cd kafka_2.12-0.10.2.0  
```

토픽을 만든다. 토픽은 Producer와 Consumer가 메시지를 주고 받을수 있게하는 채널/식별자 같은 것이다. 레플리케이션이나 파티션 등은 여기서 다루지 않겠다. 연습용이니까~ ㅎㅎ    

```  
    $ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test_topic  
```  

다음, 새로운 터미널을 열고 컨슈머를 실행한다.  

```  
    $ cd kafka_2.12-0.10.2.0
    $ ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test_topic --from-beginning
```  

새로운 터미널을 열고 프로듀서를 실행한다. 

```  
    $ cd kafka_2.12-0.10.2.0  
    $ ./bin/kafka-console-producer.sh --broker-list localhost:9092 --topicc test_topic  

    # 터미널에 글자를 입력하고 엔터를 치면, 컨슈머 터미널에서 전송된걸 확인할 수 있다.  
    # 근데, 1초 정도 늦게 반응한다. (왜?)  
```  


파이썬 프로듀서 예제     
================  

파이썬으로 카프카 클라이언트 개발하기 위한 모듈은 몇가지 옵션이 있으나, 조사 결과 confluent-kafka가 제일 속도가 빠르다고 한다. 

![](http://www.popit.kr/wp-content/uploads/2016/07/kafka_python-600x346.png)  


래빗MQ 대신 카프카 쓰는 이유가 속도인데 당연히 빠른 놈으로 고고~  

```
    # Python 3.7에서만 테스트 해봄  
    $ pip install confluent-kafka   
```

테스트를 위해 [producer.py](https://github.com/confluentinc/confluent-kafka-python/blob/master/examples/producer.py) 가져온다.  

```  
    $ python producer.py localhost:9092 test_topic  
    # 터미널 글자를 입력하고 엔터를 누르면 된다.  
    # 실시간으로 전송되는 것을 컨슈머 터미널에서 확인 할 수 있다.   
```  

C++ 프로듀서 예제  
===============  

언제나 처럼 씨뿔뿔은 복잡하고 으렵다 ㅎㅎㅎ  

먼저, 카프카 라이브러리를 설치해야 한다. sudo apt-get를 이용해서는 하지말고 소스로 설치해야 이후 C++래퍼 라이브러리가 정상으로 동작한다.  

```
    $ git clone https://github.com/edenhill/librdkafka.git

    $ cd librdkafka   
    $ ./configure  
    $ make  
    $ sudo make install  
``` 

다음으로 C++래퍼 라이브러리(cppkafka) 설치하자. boost와 cmake가 이미 설치되어 있어야 한다. 

```  
    $ git clone https://github.com/mfontanini/cppkafka.git
    
    $ cd cppkafka  
    $ mkdir build  
    $ cd build  
    $ cmake ..  
    $ make   
    $ sudo make install  
```
C++ 래퍼 라이브러리 소스코드에 examples 폴더가 있는데 프로듀서 예제가 있다. 근데, 사용전에 CMakeLists.txt를 좀 수정해줘야 한다.  

    cmake_minimum_required(VERSION 3.9)
    FIND_PACKAGE(Boost COMPONENTS program_options REQUIRED)
    INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS})

    set(CMAKE_CXX_FLAGS "-std=c++11 -Wall")
    SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath -Wl,/usr/local/lib")

위 내용을 맨 위에 넣어 주고 다음 실행하자. 

```  
    $ cd cppkafka/examples/
    $ mkdir build
    $ cd build
    $ cmake ..
    $ make producer 

    $ ./producer -b localhost:9092 -t test_topic
    # 터미널 글자를 입력하고 엔터를 누르면 된다.  
    # 실시간으로 전송되는 것을 컨슈머 터미널에서 확인 할 수 있다. 

```  

자바스크립트 프로듀서 예제
===================== 

To-be Updated 

참고  
====  

- [파이썬 카프카 클라이언트 비교](https://www.popit.kr/kafka-python-client-%EC%84%B1%EB%8A%A5-%ED%85%8C%EC%8A%A4%ED%8A%B8/)  
- [로컬에서 kafka 실행하기](https://medium.com/@dokkl2323/kafka-47c7b785c65f)
- [카카오 Kafka운영 담당자의 글 모음](https://www.popit.kr/author/peter5236)