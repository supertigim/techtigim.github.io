--- 
layout: post  
title: Pixhawk based Drone Development  
tags: pixhawk drone mavproxy python pyqt pyopengl qt  
excerpt: pixhawk 기반의 드론 개발을 위한 준비    
---  

드론은 날려 보았으니 이것 저것 다른 것이 해보고 싶었다. 최종으로는 드론 군집 비행 같은 것을 좀 해보려고 하는데, 관련해서 스터디 한 내용을 정리해 보았다. 그런데, 군집 비행 하려면 몇 대가 필요할까... -_-;;;    

## python 설치 

가능하면, c/c++로 개발을 할건데, 관련 내용은 대부분 python으로 되어 있어 공부를 좀 해야했다. 스크립트 언어에 대해 그리 좋은 인상이 없는데, 프로토타입 개발이 빨라서 익혀두면 좋을 것 같다. (라고 긍정적으로 생각해보지만...)  

특히, python은 참 마음에 안드는게 버전 별로 호환이 안 되는 거다. 최신인 3.5버전이 있는데도 불구하고  대부분 사용하는 것은 2.7대 버전. 제대로 된 언어로 인정받고 싶다면 스스로 통일부터 했으면... 여기서는 일반적인 설치는 냅두고 에러 상황이나 특이한 내용에 대해 정리했다.  

### pip로 모듈 설치할 때 이상하면

모듈 설치가 안될 경우, 이렇게 하면 되지만 결국은 재설치했다. 속편하게... 
  
	pip install --ignore-installed [module]  
	easy_install [module]  

### python 실행이 이상할 때, 재설치

이리 설치되어 있는 python을 다 설치하는 방법. 물론 나는 맥이라 brew를 이용해서~ ㅎㅎㅎ 
  
	brew doctor   
	brew reinstall python
	brew install python
  
	brew linkapps python
	brew link --overwrite python
	hash -r python

### 주석 때문에 실행이 안될 때  

주석 시작하는 부분에 아래 내용을 넣어둔다. 
  
	# -*- coding: utf-8 -*-
	"""
	your comments
	bluh bluh~~~
	
	"""

## DRONEKIT  
![](https://cloud.githubusercontent.com/assets/5368500/10805537/90dd4b14-7e22-11e5-9592-5925348a7df9.png)  

3d Robotics에서 개발한 python tool이다. 간단히 여러가지 상황을 확인해 볼 수 있어서 좋다. 특히, "**Guided 모드**"를 활용한 Example은 많은 도움이 되었다.   


### dronekit 설치 

[공식 홈페이지](http://python.dronekit.io/guide/quick_start.html)의 Quick Start로 해서 안될 때가 있는데, 그냥 직접 github에서 받아 설치하면 된다. 
  
	sudo pip install git+https://github.com/dronekit/dronekit-sitl  
	sudo pip install git+https://github.com/dronekit/dronekit-python

### mavproxy 연동  

굳이 연동 할 필요 없지만, 프로그램과 함께 GCS를 통해 동작을 확인하기 좋다. 
우선, 드론 simulator만 실행을 시킨다. home position은 한국/서울로 잡고, copter 이미지를 이용한다. 

	dronekit-sitl --list
	dronekit-sitl copter-3.3 --home=34719086,127.0285034,584,353

mavproxy로 bridge역활을 하며, 출력을 두 개로 나눈다. 

	mavproxy.py --master tcp:127.0.0.1:5760  --out 127.0.0.1:14550 --out 127.0.0.1:14551

dronekit의 예저 실행  

	python guided_set_speed_yaw.py --connect 127.0.0.1:14551  

QGroundControl을 실행 시키면 자동으로 port 14550으로 접속

## GUI Programming - PyQT  

뭔가 테스트 해보려고 하니 CI는 느무 불편해서 아예 GUI로 구현 시도 중

왜 이걸 많이 안 쓰나 했더니 설치해야하는 진입 장벽이 존재함 -_-;; (윈도우는 제외) 매뉴얼도 좁쌀만한 크기의 말 때문에 읽기도 싫고 말이지...    

SIP을 먼저 [다운로드](https://riverbankcomputing.com/software/sip/download) 받아서 압축을 풀자. 

	python configure.py  
	make install   

PyQT도 [다운로드](https://www.riverbankcomputing.com/software/pyqt/download) 받고 압축을 풀자. 

	python configure-ng.py  
	make   <== 꼭 해야함 
	make install   
	
테스트 해보자고... 

	import sys
	from PyQt4.QtCore import *	
	from PyQt4.QtGui import *

	app = QApplication(sys.argv)
	label = QLabel("Hello PyQt")
	label.show()
	app.exec_()

코딱지만한 윈도우가 나오므로 눈크게 뜨고 찾아봐야함  

### QT Creator  

qt designer를 쓰라고 되어 있는데, qt creator로 통합이 되어있는듯. 설치 파일을 받아서 그중에 Tool만 설치하면 된다. 다른 것들은 c++ 라이브러리인데, 쓸일이 없다. 

설치된 QT Creator를 실행하고, 상단의 File->New File을 선택하면 아래의 화면이 나온다. 여기서 Desginer Form를 선택하고 파일 이름 넣으면 된다.![](https://camo.githubusercontent.com/0b8403f1947132294d3c440d5c29a3c42c383bdf/68747470733a2f2f646c2e64726f70626f7875736572636f6e74656e742e636f6d2f752f35303236323231392f496e74726f2d746f2d51742d55492d66696c65732f6265666f72652d73746570312d7573696e672d717463726561746f722e706e67)  
 
간단히, 위젯, 버튼 등을 넣고 저장한다. 여기서 파일 이름은 mainwindow.ui이다. 이파일을 python 파일로 변경한다.    
	
	# 정상적으로 qt가 빌드 되었다면, pyuic4가 설치 되어 있음
	pyuic4 -x mainwindow.ui -o mainwindow.py 
	 
	# 아무 메시지 없으면 변환 성공 
	python mainwindow.py
	

## 기타 도움 될 만한 것  

Use mavproxy to make the initial connection, and then fork it to Dronekit and Mission Planner.

	mavproxy.py --master=/dev/ttyUSB0 --out=127.0.0.1:14550 --out=127.0.0.1:14551

## Reference Sites

- [dronekit github](https://github.com/dronekit/dronekit-python)
- [QT Designer를 이용한 GUI 개발](http://scripting.tistory.com/790)
- [예제로 배우는 파이선 프로그래밍](http://pythonstudy.xyz/)
- [Qt Designer를 이용한 layout](https://wikidocs.net/2598)
