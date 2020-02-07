---  
layout: post 
title: FPV Drone 개발 준비   
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: 나도 드론 레이싱을 해보자 
---  

[드론 레이싱 동영상](https://www.youtube.com/watch?v=uhw2KpkGj9E)을 우연히 보고, 이건 하지 않을 수 없다는 생각을 했다.  
  
---   
## 시작하기  
 

드론 개발을 이전에 해보지 않아 시작이 제일 어려웠다. 제일 간단히 시작할 수 있는 방법은 완제품을 구입하는 것이나, 가격적인 압박과 함께 엔지니어 마인드 발동으로 직접 개발 하기로 결정했다. 
  
최종 목표는 FPV를 하는 것이기 때문에 관련 기종으로 조사를 하였고, QAV250가 여러가지 시도해보기 좋다고 하여 관련 부품으로 구입을 했다.  
  
---   
## 구매 목록  
 
  
어디를 찾아봐도 국내 사이트에서 구매는 없었다. 시간을 걸리지만, 나도 대부분의 부품은 alliexpress를 통해서 샀다. 

### 필수 사항  
- [바디세트](http://www.aliexpress.com/item/Mini-250-Quadcopter-2204-2300kv-Motor-12A-Esc-CC3D-Flight-Control-5030-Propeller-CC3D-Distribution-LED/32297309822.html) : 250 Mini Quadcopter + CC3D EVO Flight Controller + 2204 2300KV Motor+ ESC+5030 propeller 구성으로 가격은 $74.99  
- [추가날개](http://www.aliexpress.com/item/16pcs-8-Pairs-5x3-5030-Nylon-Prop-Propeller-CW-CCW-for-250-mini-Quadcopter-QAV250-BLACK/32350958395.html) : 처녀 비행부터 작살이 난다하여 넉넉하게 준비. $6.54  
- [조정기+수신기](http://www.aliexpress.com/item/Newest-Flysky-FS-i6-FS-I6-2-4G-6ch-RC-Transmitter-Controller-w-FS-iA6-Receiver/32238547697.html) : Newest Flysky FS-i6 Controller + FS-iA6 Receiver. 좋은 것도 많으나 이정도면 충분하다는 여러 글을 보고, 그리고 어설프게 좋은거 사느니 나중에 제일 좋은거 사려고 결정. $49.80  
- [프로펠러가드](http://www.aliexpress.com/item/4pcs-Diatone-Glass-Fiber-250-Quadcopter-For-5030-Propeller-Protective-Guard/32319811541.html) : 드론이 생각외로 위험한 물건이라는 것은 조금만 조사해 보면 안다. 우리집에 애기도 있고하여 구입. $4.68  
- [배터리스트립](http://www.aliexpress.com/item/10-Pcs-Set-100-New-Strong-RC-Battery-Antiskid-Cable-Tie-Down-Straps-26-2cm-5/32281308568.html) : 배터리던가, 여러가지 추가 장치들 고정에 필요. $2.11  
- [배터리충전기](http://www.aliexpress.com/item/IMAX-B6-Combo-with-12V-5A-AC-Adaptor-2S-6S-7-4v-22-2V-AC-DC/480064617.html) : 뭔지 모르고 인터넷으로 찾다가 발견. 일단 다 싼걸로 구입. $24.17  
- [배터리](http://www.aliexpress.com/item/TURNIGY-Lithium-Polymer-LiPo-Li-Polymer-Battery-Li-Po-2S-7-4v-2200mah-30C-with-XT60/32479243597.html) : 종류가 너무 많고, alliexpress에서는 비추한다는 글이 있었으나, 난 한국 남자. 한곳만 판다. ㅋㅋ 그런데, 사고나서 후회했다. **배터리 연결이 본체 파워 연결 장치와 틀려서 당황**, 결국 나중에 연결 부분 재 구입 -_-;; $14.99  
  
### 추가 사항  
  
- [바나나콘넥터+수축튜브](http://www.aliexpress.com/item/20pairs-lot-2-0mm-2mm-Banana-Gold-Bullet-Connector-DIY-RC-Battery-ESC-Motor-Plug-with/32315618822.html) : 모양이 바나나 같이 생겼다. 주 용도는 모터와 ESC연결인데, 아무말 말고 **꼭 사자!!!!!** 모터와 ESC에 삐져나온 전선 들에는 용도 표시가 없기 때문에 몇 번씩 바꿔서 연결해 봐야한다. 이것이 없으면 재 납땜질을 하면서 감미로운 납 향기를 계속 맡아야 한다. **꼭 구입하자**. $4.69  
- [배터리연결콘넥터](http://www.aliexpress.com/snapshot/7369989034.html?orderId=73168939971378) : 삽질의 결과로 구입이나, 있으면 좋을듯 하다. 다른 RC에서도 쓸 수 있고 용도는 풍부. $3.27  
- [조립용오실로스코프](http://www.aliexpress.com/item/DSO138-2-4-TFT-Digital-Oscilloscope-DIY-Kit-DIY-Parts-for-Oscilloscope-Making-Pocket-size-Handheld/32507320092.html) : 순전히 엔지니어 마인드에 구입한 제품. 드론만 날릴꺼면 필요 없다. $16.90  
- [GPS모듈](http://www.aliexpress.com/item/GPS-Receiver-U-blox-NEO-6M-Module-with-Ceramic-Antenna-TTL-Interface-for-raspberry-pi-2/32373966272.html) : 세트에 있는 CC3D에는 GPS가 없다. 그리고 나는 다른 용도로 확인해 볼 것이 있어 구입. $15.75  
- [선정리용타이](http://www.11st.co.kr/product/SellerProductDetail.tmall?method=getSellerProductDetail&prdNo=351130995&xfrom=&xzone=) : 제품 가격보다 배송비가 비싸다. 가능하면, 연계해서 구입 가능하여 구입하는게 좋다. 990원  
- [인두기세트](http://www.11st.co.kr/product/SellerProductDetail.tmall?method=getSellerProductDetail&prdNo=178763063&xfrom=&xzone=) : 세라믹 인두기 3종 세트로 구입했는데 만족, 페이스트 한통도 같이 샀다. 12,650원   
  
### 그외  

- 육각렌치 세트 : 없으면 조립 못하기 때문에 꼭 따로 구입해야 한다. 사이즈 2개인데, 크기 설명을 못하겠다. 대충 보기에 이쑤시개 크기하나, 이쑤시개2개 크기 하나가 필요하다. ^^;;;  
- 검정 테이프 : 정리를 위해서 필요  
- 공구 상자에 있는 여러 도구?  
- AA건전지 4개 : 조정기에 필요   
  
---    
## 정리  


키덜트 취미생활을 돈이 많이 든다. 같이 라즈베리파이/아두이노까지 구입하니, 30만원 가까이 들었다. 그래도 완제품에 이정도 성능을 맞추려면 50-60만원을 써야하니 거기에 만족한다.  
  
모든 조립이 끝나고, 조정에도 익숙해질 무렵 고글과 카메라도 구입할 예정이다. 우훗!!!    
  
마지막으로, alliexpress로 주문하면 짧아도 3주가 걸리기 때문에 마음에 여유를 가지자. 잊을만 하면 한개 두개씩 날라오는 재미가 있다... -_-;; (재미없다)  
  
---    
## 참고사이트  


[http://zoone77.blog.me/220434087214](http://zoone77.blog.me/220434087214)  

