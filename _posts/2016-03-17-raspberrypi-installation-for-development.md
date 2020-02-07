---  
layout: post 
title: 라즈베리파이에 개발 환경 설치  
tags: rpi    
excerpt: 라즈베리파이를 소형 컴퓨터로 사용해 보자.     
---  

## 라즈비안 설치  
  
드론에 붙이는 용도여서 lite버전을 설치하였으나, GUI의 불편함을 이기지 못하고, 결국 재설치 했다. 한김에 차근 차근 정리 해보자. 

### NOOBS Jessie 설치  
  
초보자용이라고 당당히 나와 있다. [공식 홈페이지](https://www.raspberrypi.org/help/noobs-setup/)에서 동영상까지 만들어 잘 설명했다. 따라하면 된다.  
  
간단히 말하면,  
  
1. [NOOBS 파일](https://www.raspberrypi.org/downloads/noobs/) 다운로드 (Lite버전은 랜설이 있을 경우에) 
2. [프로그램](https://www.sdcard.org/downloads/formatter_4/)으로 SD Card 포맷
3. SD Card에 받은 파일 이동 :  압축 풀고 폴더 내 모든 것
4. 라즈베리파이에 카드 삽입 후 부팅  

### lite 버전 설치 방법
  
 나중에 다시 설치할때 할란다. 귀찮음... -_-;;


## 최소 필수 설치 프로그램   

### 시스템 업데이트 

	sudo apt-get update & upgrade 
	sudo reboot  

### 한글 폰트  

	sudo apt-get install ibus ibus-hangul ttf-unfonts-core

### git  

있는 것 같으나... 

	$ sudo apt-get install git-core
	
	$ git config --global user.name "your name"
	$ git config --global user.email "your email"   

이후 ssh로 소스코드 control을 위해서는 암호키 생성과 등록이 필요하다. 내용이 길어서 [블로그 링크](http://uiandwe.tistory.com/992)로 대신하다.  
  
github에서 소스코드 클론은 다음과 같다.  

	$ sudo git clone ssh://"your github project".git  


> 후에 변경은 다음과 같이 한다.  
> $   vi ~/.gitconfig

### chrome

설치된 사파리가 매우 후짐. 복잡하니 [참고사이트](http://conoroneill.net/running-the-latest-chromium-45-on-debian-jessie-on-your-raspberry-pi-2/) 잘 따라하기  
   
--- 
## 빌드 환경 셋팅  
  
### gcc-5.x 버전 설치 (이 버전 쓰지 않음!!) 

gcc.5.0부터 성능 개선이 가능하다고 한다. 아래 사이트에서 먼저 다운로드를 한다. 
  
	curl -o http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/  
	tar xvf gcc-5.xxxx.tar.bz2 

압축 푸는데 약 10분 정도 걸린듯 하다. 빌드를 위해서 swap 사이즈를 키워야 하므로, 아래 처럼  해두자. 

	sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
	sudo chmod 600 /swapfile
	sudo mkswap /swapfile
	sudo swapon /swapfile

빌드에 필요한 패키지 설치  

	sudo apt-get install libgmp-dev libmpfr-dev libmpc-dev  
	
  
마지막 한마디 때문에... 절망... 빌드하는데 **30시간**... ㅠㅠ 안해!
  
### boost library 설치  

	sudo apt-get update
	sudo apt-get install libboost-all-dev
	
---
## 참고 사이트  
  
- [라즈베리파이로 카메라 스트리밍](https://jeanleflambeur.wordpress.com/2014/06/07/compiling-boost-1-55-with-c11-support-on-the-raspberry-pi/) : 환경 설치 부터 관련 정보 풍부 
