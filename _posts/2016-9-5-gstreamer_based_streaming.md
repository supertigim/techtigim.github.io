--- 
layout: post  
title: gstreamer를 활용한 video streaming  
tags: gstreamer webrtc raspberry      
excerpt: gstreamer를 라즈베리파이에서 해보자         
---  

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/db/Gstreamer-logo.svg/250px-Gstreamer-logo.svg.png)  

## 사용하는 환경  

드론 프로젝트에서 사용할 것이라 LTE와 WiBro의 무선 환경이다. 단방향으로 latency가 약 60ms정도 발생한다고 보면 된다. (LTE:20ms + WiBro:40ms) LTE 라우터를 곧 구입할 예정인데, 성능은 별 기대 안한다. 왜냐 안되니까... ㅠ.ㅠ    

![](http://mex-us.co.kr/data/file/sub02_1/2105793697_PXbcmHRB_77745.jpg)
  
## UDP로 통신  

 UDP가 TCP보다 빠르기에 이것으로 한것인데, 결과적으로 이동통신 무선으로는 패킷 드랍이 너무 심한지 거의 동작 하지 않는다. 절망이다. WiFi에서 테스트 해보면 거의 실시간으로 보이는데 말이다. [updated] 잘 되는 방법을 찾았다. ㅎㅎㅎ  
 
**[맥 PC]**  
고정 IP는 LTE라우터에서 제공하기 때문에 포트만 4653 열어두고 대기 한다.  

	gst-launch-1.0 -e -v udpsrc port=4653 ! gdpdepay ! rtph264depay ! avdec_h264 ! videoconvert ! autovideosink sync=false  

아래꺼는 잘 됨... 딜레이 약 2초 

	gst-launch-1.0 -e -v udpsrc port=4653 ! queue2 max-size-buffers=1 ! decodebin ! autovideosink sync=false	
**[라즈베리파이]**  

	raspivid -t 0 -h 400 -w 600 -fps 15 -hf -vf -b 2000000 -o -| gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! gdppay ! udpsink host=your MAC IP port=4653

위에 되는 거랑 pair로 하면 된다.. framerate을 떨어뜨리니 속도까지 빨라진다.   

	raspivid -t 0 -hf -vf -n -h 300 -w 400 -fps 5 -o - |gst-launch-1.0 fdsrc ! udpsink host=your MAC IP port=4653 



## WebRTC Gateway 이용  

이거 발견하고 얼마나 기뻤었던지 ㅎㅎㅎ 집에서 WiFi로 하는데 참 잘되었다. 허나, 무선망에서는 역시 뺑글뺑글 시계만 돌아가고 전혀 되지를 않는다. ㅠ.ㅠ 아~ 짱나~!  

### [Janus](https://github.com/meetecho/janus-gateway) 오픈소스  

요거를 서버에 설치하고 중계 역할을 맡기면 된다. 설치는 아래와 같다.  

	$ sudo install libmicrohttpd-dev libjansson-dev libnice-dev libssl-dev libsrtp-dev libsofia-sip-ua-dev libglib2.0-dev libopus-dev libogg-dev libini-config-dev libcollection-dev pkg-config gengetopt libtool automake dh-autoreconf

옵션으로 몇 가지 설치(Data Channels, WebSockets, RabbitMQ)할 수 있는데, 안해도 상관 없다. 

	$ git clone https://github.com/meetecho/janus-gateway.git
	$ cd janus-gateway
	$ sh autogen.sh
	$ ./configure --disable-websockets --disable-data-channels --disable-rabbitmq --disable-docs --prefix=/opt/janus
	$ make
	$ sudo make install  

default configuration 파일을 만들어 주자.  

	$ sudo make configs

다음으로  /opt/janus/etc/janus/janus.plugin.streaming.cfg를 다음과 같이 수정한다. 다른 내용은 주석 처리하면  된다. 기능이 많은데 안쓰는 거라서...   

	$ sudo vim janus.plugin.streaming.cfg

	[gst-rpwc]
	
	type = rtp
	id = 1
	description = RPWC H264 test streaming
	audio = no
	video = yes
	videoport = 8004
	videopt = 96
	videortpmap = H264/90000
	videofmtp = profile-level-id=42e028\;packetization-mode=1  
	
firewall을 사용하면 포트 열어주는 것 잊지 말자.   

	ufw allow 8004  

**[서버]** 

서버 ip는 server_ip로 하고 포트는 8004번으로 한다. 일단 터미널 창을 2개 실행한다. 하나는 janus 데몬을 실행하고 다른 하나는 간단히 웹서버를 돌린다. 

	## 1번 터미널 
	cd /opt/janus/bin/
	./janus -F /opt/janus/etc/janus/  

아래의 웹서는 8000 포트로 열었다. 방화벽이 설치했으면 포트 오픈 잊지 말자. 

	## 2번 터미널  
	cd /opt/janus/share/janus/demos
	python -m SimpleHTTPServer 8000  
  
브라우저에서 http://server_ip:8000으로 접속해서 아래와 비스무리한 화면이 나오면 성공이다.  

![](https://pbs.twimg.com/media/B8czy6RCAAAEozH.png)


**[라즈베리파이]**  

일단, 테스트로 동영상을 만들어서 서버에 보내 보자. test.mp4로 만들고 qtdemux를 붙여주면 잘 된다.  

	gst-launch-1.0 -v filesrc location=./test.mp4 ! qtdemux ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink host=server_ip port=8004  
	
카메라가 제대로 설치 되었다고 가정하면, 다음과 같이 보낼 수 있다. 

	raspivid --verbose --nopreview -hf -vf --width 640 --height 480 --framerate 15 --bitrate 1000000 --profile baseline --timeout 0 -o - | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink host=server_ip port=8004


## 결론  

LTE로는 잘 안된다!!! 안돼~~~!!! 그리고 UDP에 비해 TCP가 latency가 심해도 동작으로 한다. UDP는 WiFi 환경에서만 되는걸로~ ㅠ.ㅠ  


## 참고    
**[맥북에서 카메라 동작 확인하는 명령]**

	gst-launch-1.0 autovideosrc ! osxvideosink  

**[사이트]**  
- [Building a Raspberry Pi 2 WebRTC camera](http://www.rs-online.com/designspark/electronics/eng/blog/building-a-raspberry-pi-2-webrtc-camera)  
- [How to deploy Janus](https://janus.conf.meetecho.com/docs/deploy.html)   
- [gstreamer latency handling](http://zoolu.co.kr/blogs/2015/11/09/gstreamer%EB%A1%9C-rtp-%EC%8A%A4%ED%8A%B8%EB%A6%AC%EB%B0%8D-%ED%95%98%EB%8A%94-%EB%B2%95)



