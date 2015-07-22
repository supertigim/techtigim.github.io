--- 
layout: post 
title: WebRTC 개발(을 한다면...) 
tags: webrtc  cordova phonertc coturn kurento appear.in opentok playrtc
---  
실제로 내가 개발할 일이 있을까 싶지만... ㅋㅋㅋ 일단 개발용으로 정리가 필요하다고 생각한다. 

## 1. 오픈소스  

너무 많아서 뭐가 좋은지 솔직히 모르겠다. 일단, 몇 시간의 웹서핑의 결과 이정도면 충분하고 생각하는 것으로 정리 해봄. 

> 빠른 개발을 원하면 매달 돈 내고 사용하는 CPaaS를 권장한다. Twilio, [Tokbox](https://tokbox.com/)(가격만 빼면 제일 괜찮은 듯), 국내에는 SKT가 하는 [PlayRTC](https://www.playrtc.com/) 정도가 있다.   

### [Kurento](http://www.kurento.org/)  

멀티 미디어 서버. PlayRTC 컨퍼런스에 참석한 적이 있는데, 미디어 스트리밍과 코덱이 가장 큰 이슈가 될 것 같으므로, 상용 프로젝트에서는 이부분에 대해 신경을 많이 써야 할 것으로 보였다. 특징은 일대다통신 지원, 여러 코덱의 지원이고, 인상적인 것은 텔코 후원이라 향후(지금도 될지 모름 -_-;;) PSTN(IMS)과 연동 기능도 제공 할 것으로 보인다. 유럽 정부에서도 펀딩을 받기 때문에 앞으로도 계속 업데이트 될 것 같다. 

### [coturn](https://github.com/coturn/coturn) 서버    

RFC5766 문서를 기반으로 개발한 표준 샘플 TURN/STUN 서버이다. 왠만한 기능은 다 되는 것 같고, 특히 Kurento를 쓰려면 TURN서버가 필요하기 때문에 같이 쓰면 될 것이다.

### [PhoneRTC](http://phonertc.io/)   

클라인트용으로 현재는 Beta 2.0이기는 하나 구글링 해보니 많이들 참고하는 것같다. 강점은 [Cordova](https://cordova.apache.org/#top)(예전 PhoneGap)기반으로 되어 있어, iOS나 Android 모두 지원되고 개발시 원소스 멀티유즈가 가능해서 좋아 보인다. 

### [appear.in](https://appear.in/)  

정말 아무것도 하지 않고, 내 웹페이지에서 화상 통신을 하고 싶다면, 강추!! 안드로이드/iOS 앱도 있어서 모바일에서도 사용 가능 하다. 가장 큰 특징은 iframe형태로 카피해서 사용도 가능하다. (물론, appear.in이라는 로고는 박혀있다) 하지만, 돈 안들이고 노력도 안하고 이만한 기능을 제공하는 데는 보지 못했다. ㅎㅎㅎㅎ  


----------


## 2. 기타  

손 안대고 코풀기 위해 이것 저것 많이 찾아봤다.   

### 초간단 STUN 서버 설치  

시그널링 서버는 간단히 Node.js 로 가능한데, TURN서버의 경우는 조금 어려운 것 같다. 물론 추측이다. 어째든, 구글링 통해서 안건데 TURN서버도 간단히 설치 가능하다.  

    sudo apt-get install rfc5766-turn-server  
 
그냥 당황 스러운 뿐 ㅋㅋㅋ 관련해서 셋팅하는 것은 [PhoneRTC사이트](https://github.com/alongubkin/phonertc/wiki/Installation)에 곱게 설명되어 있다.  

### [Chrome Embedded Framework](https://bitbucket.org/chromiumembedded/cef) (CEF)  

WebRTC의 현실적인 문제는 구동되는 브라우저가 제한적이라는 것이다. 해결 방법은 사용자에게 크롬이나 파이어폭스 설치하고 사용하라고 권하는 것인데, UX측면에서 봤을때 그리 좋은 행태(?)는 아닌것 같다. 그래서 고민해 볼 만한 것이다. Windows용 프로그램이다. 이게 잘 동작하는지는 확인 안해봐서 잘 모르겠지만, 실제 개발을 한다면 이 툴을 검토해 보는 것도 좋은 방법일듯 하다.   

