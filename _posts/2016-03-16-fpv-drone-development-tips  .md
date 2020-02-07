---  
layout: post 
title: FPV 드론 조립 팁     
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: 드론 조립은 절대 어렵다. 팁 줄께~ ㅋㅋ 
---  

부품 사놓고 날씨 좋아져서 조립을 하는데, 정말 해결해야 할 문제가 한 두가지가 아니다. 몇가지 정리해본다.  

---
## 바나나 컨넥터로 모터-ESC 연결  
  
택배 상자를 열어 보고 가장 당황한 것이 ESC와 모터에 주렁주렁 달려있는 선들이다. 어떤 표시도 없고 색 구분도 없다. 마냥 황당할 따름이다. 구글링에 그림보고 따라서  그냥 납을 고정 시키는 실수를 절대로 하지 말자. ESC와 모터는 고장도 잘나거니와 구글링 그림들이 정확하지가 않다. openpilot에서 모터 calibration을 할 때, 방향 확인이 가능하니 **try & error**로 맞는 연결을 찾는다.  
  
----
## ESC 칼리브레이션  
  
처녀 비행 성공의 기쁨도 잠시... 뭔가가 이상하다. 스로틀을 아무리 올려도 땅에서 30cm이상 떠오르지 않는다. ㅠ.ㅠ 배터리가 거의 방전인 것도 이유가 되지 않을까 해서 충전을 하면서 찾아보니, ESC Calibration을 해주어야 했다.  

[http://dronefactory.co.kr/220599685522](http://dronefactory.co.kr/220599685522) 블로그에서 동영상을 보면서 작업하면 된다. OpenPilot은 이미 사용해봤다는 전제하에서 시작!  

---
## 프로펠러 연결  
  
![오픈파일럿의 모터 캘리브레이션](http://www.phasercomputers.com.au/wp-content/uploads/2016/02/54da3c8a6ab4457e8f1b5df20ab563714_rotor_quad.jpg)  
위 그림처럼 OpenPilot에서 모터의 방향 설정을 맞추었는데도 불구하고, 드론이 뜨지 않아 이상하게 생각했더니만, 프로펠러를 잘 못 설치한 것이다. 날개에 방향 표시가 되어 있지만, 뜨지 않는다면 바람 방향을 확인해보자.  
  
---
## CC3D와 fs-ia6 리시버 연결  
  
![CC3D 연결선](http://www.dronetrest.com/uploads/db5290/854/b6029537dd35d092.png)
시간이 제일 오래 걸렸다. 처음 보면 나와 있는 선의 정체와 어디에 연결해야 하는지 알 수가 없다.  
![리시버 채널 셋팅](http://g02.a.alicdn.com/kf/HTB15WdzIpXXXXXRXVXXq6xXFXXXn/Flysky-FS-iA6-6-Channel-2-4G-font-b-Remote-b-font-font-b-Control-b.jpg)
채널의 정체가 궁금하지만, 일단 위 그림의 배선의번호를 채널로 맞춰서 끼우면 된다. (예, 1.elev - CH1) 한가닥 짜리는 선이 꽂혀 있는 쪽을 리시버의 안쪽으로 해서 연결하자. 사실 채널은 리시버-컨트롤러의 연결 방식으로 사용 용도가 달라지는데 보통은 Default로 PWM 방식을 이용한다. 
![오픈파일럿PWM으로설정화면](http://1.bp.blogspot.com/-w1uAQWHNC30/Vg1HZCq_EUI/AAAAAAAAAsY/5RJsHssmwiE/s1600/signal%2Bconfiguration.PNG)  
  
## openpilot transmitter 설정시  주의사항   

- 설정에서 하라는데로 따라하다 보면, 조정기 방향과 반대로 움직이는 것이 있다. 꼭 reverse에 체크를 해야 정상으로 동작한다. 아래 그림에서는 우측에 Rev라는데 체크 된 부분. 설정은 "Start Configuration Wizard로 했다.  
![오픈파일럿조정기설정](http://www.fpvmanuals.com/wp-content/uploads/2013/07/Screen-Shot-2013-07-05-at-1.23.55-PM.png) 
- 설정에서 조정기를 맥시멈으로 움직여야 나중에 정상으로 동작하니 크게 크게 돌리자.  
- 아밍(Arming)이라고 시동 거는 거는 잊지말고 하자. 처음하는 사람들은 이거 세팅 못찾아서 3박4일헤맨다. 안정상 보통 Yaw-Right로 셋팅한다. (왼쪽 조정기를 오른쪽으로 밀면 조정 가능 상태로 변경)
![오픈파일럿에서아밍방법](http://photo.5imxbbs.com/forum/201408/26/094459yfwpscfxwlkl1ezu.jpg)
- 제일 중요한 것은 셋팅 후에 꼭 **Save**를 하자.  

> 이런거 쓰지말고, 이런 블로그 좀 더 찾아볼껄... ㅋㅋㅋ
> [http://blog.naver.com/kda10046/220612379916](http://blog.naver.com/kda10046/220612379916)