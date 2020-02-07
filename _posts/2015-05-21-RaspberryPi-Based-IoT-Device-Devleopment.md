--- 
layout: post 
title: Research for RaspberryPi based IoT Device Development 
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---

내가 하는 일의 정체성(?)은 무얼까? ㅋㅋㅋ 폭넓은 행보가 아닐 수가 없다. 일단, 재미있어 보여서 해주기로 함.  

### Product Concept  

사내 기획이라 공개는 어렵지만, 작은 무엇인가를 만든다고 보면 된다. 별건 아니고... Portable이면서 Connectivity가 자유로워야 하는데, 쉽게 말해 센서링해서 데이터를 서버로 보내주면 되는 것이다. (요즘 IoT  다 이 컨셉 아님? ㅋㅋ)  

###Desirable components  

    Raspberry Pi2 + Shaomi Battery + Sensors
  
이것은 별로 중요치 않다. 시스템이야 언제든 바뀔 수 있으니까...  

### Alternative OSHW

아두이노는 종류가 굉장히 많은데, 그 중 크기가 작은 [Nano](http://www.arduino.cc/en/Main/ArduinoBoardNano), [미니](http://www.arduino.cc/en/Main/ArduinoBoardMini) 등의 제품이 있다. 성능은 유사하지만, 무엇인가 연결을 하려면 납땜질이 필요하다. 아주우~! 귀찮다.  
![enter image description here](http://arduino.cc/en/uploads/Main/ArduinoNanoFront_3_sm.jpg)

![enter image description here](http://arduino.cc/en/uploads/Main/Mini05_front_450px.jpg)

[TinyCircuit](https://tiny-circuits.com/)이라고 아두이노를 표방한 작은 사이즈의 제품이다. 하지만, 상용화를 한다면 칩 업체를 통해서 하드웨어 작업을 해야하는데 시제품에서 널리 알려지지 않은 이런 제품을 쓰는 것은 약간 수고스러운 것 같다.  
![enter image description here](https://www.openhomeautomation.net/wp-content/uploads/2013/11/boards_assembled.jpg)  


퀵스타터에 나온 [C.H.I.P](https://www.kickstarter.com/projects/1598272670/chip-the-worlds-first-9-computer)이라는 제품도 있다. 크기도 작으면서 가격도 무려 $9이다. GPIO도 달려 있는 것으로 봐서 여러가지 작업도 가능하리라 생각한다. 
![enter image description here](https://ksr-ugc.imgix.net/assets/003/746/703/5a41c8ab3a4063bb1a31d8c5f2340b84_original.jpg?v=1430978433&w=700&h=&fit=max&auto=format&q=92&s=3a25c851c72029e1fd5eca7d053fa51c)  

### Sensors
  

 - [Adafruit Ultimate GPS Breakout](https://www.adafruit.com/products/746)  : $39.95  
 - [Raspberry Pi Camera Board](http://www.adafruit.com/products/1367)  : $29.95  
 - [4GB Memory Card](http://www.adafruit.com/products/102) : $7.95  

### Time Table  

 - 서비스 로직 정립(재료 주문 / 개발 환경 세팅 포함) : 2주
 - 하드웨어 특성 확인(기본적인 드라이버 설계 작업 포함) : 2주
 - Software 상세 설계 및 관련 Open Source 조사 : 2주 
 - 서버 설치 및 연동 테스트 : 1주
 - 코딩 : 3주
 - 테스트 : 2주   

욕이 절로 입밖으로 튀어나오지만, 개발 한번 안해본 XX들은 "오픈소스 기반으로 작업하면 이미 다 되어 있는 거 갔다 붙이는 거 아니야?"란 어쭙잖은 소리를 지 멋대로 내 뱉는다. 네가 한번 해봐라, XXX들아... 그런 논리라면 반제품으로 부품 공급 받는 자동차는 조립하자마자 팔아야 해~~~!!!

### Reference 

 - [라즈베리파이를 활용한 전축 시스템 구축](http://cafe.naver.com/audiostudy/20557)   
 - [라즈베리파이로 RC카 개발](http://lsj30224.blog.me/220348241671) 