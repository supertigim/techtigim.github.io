--- 
layout: post 
title: PlayRTC 세미나 후기 
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---

skt에서 개최한 PlayRTC 세미나  

### WebRTC란? 

이런거 찾아볼 사람이면 당연히 아는거지만, 나눠준 브로셔에 쓰인 내용을 옮겨본다. (문외한이 듣기에 적절한 표현)

**오직 웹 브라우저만으로 인터랙티브하게 영상이나 음성 통화, 파일 공유를 실시간으로 커뮤니케이션 할 수 있는 신기술 표준**  

구라가 아닌듯 구라인건 아시죠? 그렇게 좋으면 뉴스 소개도 없이 이렇게 숨어서 개최부터 할리가 없잖아~~ :) 홍보에 열라 바쁘신 텔코께서 이러면 너무 궁색해요~

### PlayRTC란?

괜히 남기면 자동 홍보하는 것 같은데, 그러거나 말거나 우리 임원들은 할 생각이 없거든... 윗사람 잘 만나는 것도 복이다. 여기와서 이런거 하면 좋겠구만~ 각설하고,

PlayRTC는 html5의 WebRTC 기반 기술로 만든 플랫폼? 개발 도구?  

**비전문가도 적은 비용으로 쉽게 빠르게 개발할 수 있는 WebRTC의 가치**  

ㅋㅋ 텔코답다. 절대 공짜는 없어요~~ 베타 끝나면 바로 유로 들어갑니다? 

**고객 경험 증대 및 차별화, 비용절감, 고객 만족도 향상, 확장성, 미래표준, 새로운 Biz 기회, 비용절감**  

어디서 많이 듣던 말 아니니? 결론 고로 다 해주어라~ :)

그래도 여기가 제일 낫다는 게 내 결론~ ㅠ.ㅠ 에잉씨! 

-------

### 사전 설명

-> 웹RTC분과에서 행사
-> 웹보안 / 웹접근성 

홈페이지에서 가입 
포럼 설명 : 네이버 중심으로 통신3사가 진행  

### 프로그램 간략 소개
 
- 초기이슈: codec 전쟁이었으나, H.264지원으로 일단락  

- 상용 서비스 현황  

|||||
  ------- | ------------ | ------- | ----
|   weemo  | appear.in    | vLine       |easyRTC     |
| PeerJS | switch.co | Norming |Performing |  
|   talko  | veckon    | temasys       |tokbox     | 
| plivo | skyway | hookflash |cafeX |  
| twilio | azar | ||  
 (*) azar 서비스는 이미 천만 사용자   

 - 유명업체 동향  
MS는 ORTC 개념을 정리하는 자체 브라우저에 적용 준비   
시스코는 TROPO 인수  
힙챗 / 슬랩 - 스크린 쉐어링을 위해 WebRTC 사용   

 - WebRTC의 비지니스 특징: 소기업 - 중기업 - 대기업 모두가 다 할 영역이 있다. 아래 4가지 PaaS로 구분  

|   텔코중심  | 전통 comm. API    | 순수 WebRTC PaaS |영상 솔루션 기반     |
  ------- | ------------ | ------- | ----
| tokbox, AT&T, PlayRTC | Twilio, Plivo | Temasys, TokBox, EasyRTC, PlayRTC |bistri, appear.in, CafeX, viblast |  
<br />  

 - 킬러서비스 : azar 정도가 후보 

 - IoT와 결합 가능성 : 낮다고 고려  
스크린이 있는 장치
웨어러블 캠   

 - Hot Trend  
 2015년 화두 실 시간 스트리밍 서비스 -> 개인 방송 서비스  
tinder 가 대세  

 - CDN과 WebRTC의 관계    
PeerCDN이라는 회사를 야후 최근 인수 --> 개념: 피어가 개인 CDN이 있다. 

### 기술소개 세션 

P2P라서 디스커버리(서버를 통해서)  커뮤니케이션 과정이 있음 

시그널링 방법 ? ICE Framework가 제일 많이 한다 (UDP로)

NAT(Network Address Translation)  - 일종의 라우터, 사설 IP 망이라고 보면 됨 

STUN(Session Traversal Utilities for NAT) - 공인 IP 확인을 위한 외부 서버

TURN(Traversal Using Relays around NAT) - 미디어 스트림을 전달하는 경우 

DTLS (Datagram Transport Layer Security) - UDP 상에서 보안용

SCTP (Stream Control Transmission Protocol) - UDP + TCP 복합적인 프로토콜 - Multi Homing / Multi Streaming 때문에 좋다? 

RTP (Real-Time Transport Protocol)

ORTC - 데이터 좀 더 집중할 수 있게 

시그널링 서버를 어떻게 만드나? 
채널API / Socket.IO / Websocket으로 사용 

Candidate - 미디어를 수신할 수 있는 가능성을 가진 네트워크 경로 후보군 

signaling - peer 식별을 위한 / 핵심은 sdp와 candidate 교환

![enter image description here](https://lh3.googleusercontent.com/-ajy1sjW-3Nk/VWa5Lt_jG-I/AAAAAAAA7MQ/xVaWu4KFISI/s0/IMG_0692.JPG "Flow Chart at PlayRTC 세미나")

![enter image description here](https://lh3.googleusercontent.com/-NR_EfCypCHs/VWa4cUNFQoI/AAAAAAAA7Lg/wtsZCYCjfi4/s0/IMG_0693.JPG "DataChannel at PlayRTC 세미나")

![enter image description here](https://lh3.googleusercontent.com/-YJJya_ejDLY/VWa5A790IaI/AAAAAAAA7ME/uJNAAfilZWY/s0/IMG_0695.JPG "대용량 파일 보내기 at PlayRTC 세미나")





