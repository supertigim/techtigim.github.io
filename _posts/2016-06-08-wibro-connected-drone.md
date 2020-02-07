--- 
layout: post  
title: WiBro Connected Drone  
tags: wibro drone gcs streaming pixhawk  
excerpt: Wibro 에그를 활용하여 드론 자율주행/스트리밍 구현   
---  

이것 한다고 했다고 했을때의 모두의 반응은 왜 LTE안쓰고 Wibro로 했냐는 것이다. 다른 이유는 없다. LTE모듈을 내 돈 주고 사기가 너무 싫었고, 집에서 고히 잘 놀고 있는 에그를 활용해서 만들어 보고 싶었다.     

## 부품 소개     
  
FPV 레이싱 전용의 250 사이즈의 작은 기체를 구입 하였으나, 몇 번 날려보고 느낀 것은 그외에 어떤 활용도 불가능하다는 것을 깨달았다. 결국, 고민 끝에  새로 기체 제작을 하기로 결정했다.  

- 프레임: DJI F450 짭으로 [알리](http://www.aliexpress.com/item/F450-Quadcopter-MultiCopter-Frame-kit-W-Black-Tall-Landing-Gear-Skid-for-DJI-F450-F550-SK480/32339098360.html)에서 구입 
- Flight Controller: DJI vs. 3DR제품 중에 고민을 하다가 [Pixhawk](http://www.aliexpress.com/item/Pixpilot-v2-4-6-Pixhawk-PX4-open-hardware-Autopilot-Flight-Controller-I2C-RGB-PPM-Neo-M8N/32316396160.html)으로 구입  
- 모터 & ESC: F450에 가장 잘 어울리는 2212 920KV 모터와 30A짜리 SimonK ESC로 [알리](http://www.aliexpress.com/item/6x-2212-920KV-Brushless-Motor-6x-30A-SimonK-ESC-Quad-Multirotor-X525-F450/32427073173.html)에서 구입  
- 프로펠러: 회전하면서 자동으로 locking이 되는 [제품](http://www.aliexpress.com/item/Drone-Replacement-Spare-Parts-4-Pairs-9450-9-4-inch-Self-locking-Enhanced-Propeller-Prop-for/32545653089.html)으로 구입. 정말 마음에 든다. 
- 배터리: 산 것중에 제일 실패한 것. 제작 전에 레이싱용으로 주문한것이라 어쩔수 없었다. 용량이 너무 작아서 5분 남짓 비행 가능하다. [1500ma/11.1v/40c](http://www.aliexpress.com/snapshot/7592029447.html?orderId=74719694811378) 다시 구매 한다면, 알리말고 국내나 하비킹에서 구입하고 5200ma로 장만 할 것이다. 여담으로 5개 도착하였는데, 이미 한개가 불량이었고, 한번 날리자 마자 다시 1개 불량. 개발한다고 라즈베리파이에 연결해 두고 커피 마시고 왔더니, 방전되어 2개 사망. 결국 현재는 한개만 남았다. ㅠ.ㅠ    
- 바나나 컨넥터: 이건 지난 블로그에서도 강조한 것인데 잊지 말고 넉넉하게 구입해야한다.  
- 기타: 양면 테이프, 찍직이 테이프, [밴드](http://www.aliexpress.com/item/10-Pcs-Set-100-New-Strong-RC-Battery-Antiskid-Cable-Tie-Down-Straps-26-2cm-5/32281308568.html), [충전기](http://www.aliexpress.com/item/IMAX-B6-Combo-with-12V-5A-AC-Adaptor-2S-6S-7-4v-22-2V-AC-DC/480064617.html), [수축튜브](http://www.aliexpress.com/item/New-Arrvial-70-pcs-Flame-Retardant-Durable-7-Color-Assorted-Colors-Ratio-2-1-Polyolefin-Heat/32371460795.html), 육각 드라이버, 인두, 납  

**추가 부품**  
드론만 날릴 생각이라면 위에 것만 준비하면 된다. 하지만, 난 와이브로 미션 수행과 스트리밍을 할 것이므로 아래 부속품을 더 준비했다. 

- Wibro 동글: 정말 구려서 이루 말할 수 없이 후지 지만, 다른 것 사기 싫어서 그냥 사용하는 제품  ![](http://mblogthumb3.phinf.naver.net/20130726_218/likesmirage_137481563691143irw_JPEG/BDBA1.jpg?type=w2)
- 배터리: 라즈베리파이3을 위한 배터리로 Pixhawk에서 전원을 공급 할 수 있으나, 동작 안정성과 로그 추출, 무게 중심 등의 이율로 유니클로에 받은 휴대용 배터리 사용 ![](http://nimage.newsway.kr/photo/2015/11/15/2015111515250353476_20151115152553_1.jpg)  
- 라즈베리파이3, 카메라모듈, 케이스: 국내 모 판매점이 있어서 쉽게 구입 가능  
 
## 드론 조립  

인터넷에 정리 잘된 사이트가 있어 해당 내용을 보고 참고 하면 될 듯하다. 

- [F450+NAZA/국내사이트](http://blog.naver.com/benzydad/220381394276)  
- [F450+Pixhawk/국내사이트](http://www.internetmap.kr/entry/Frame-Assembly-for-For-My-2nd-Quadcopter-using-Pixhawk)로 정리가 깔끔하게 잘 되어 있음  
- [P4X 공식 홈페이지](https://pixhawk.org/platforms/multicopters/dji_flamewheel_450)에서 제공하는 DJI F450 + Pixhawk 조립 방법  

**[추가 부품 붙이기]**

1. 라즈베리파이3+카메라+케이스와 배터리, 와이브로 동글을 무게 중심이 잡히도록 찍찍이 테이프와 밴드를 활용하여 고정한다. ![](https://dl.dropboxusercontent.com/u/18180490/IMG_1233.JPG) 
2. 라즈베리파이와 Pixhawk연결이 필요하다. 그림은 이전 버전의 라즈베리파이인데, GPIO핀 배열이 동일하여 동작하는데 상관은 없다. **한 가지 주의할 점**은 따로 배터리를 연결하므로 Power는 연결하지 않는다. ![](http://ardupilot.org/dev/_images/RaspberryPi_Pixhawk_wiring1.jpg)  
  
## Pixhawk Firmware 설치  

나는 직접 소스를 빌드해서 업로드 했으나, 보통은 Mission Planner나 APM Planner2를 사용하여 설치하면 된다. **Qground Control**도 사용 가능한데, 설치하는 Firmware 동작(P4X전용 Firmware로 올림)이 살짝 틀려서 처음에는 이용하지 않고, 이후 **센서 튜닝할때 이용**하면 좋을 것이다.   

### Custom Build  

일부러 소스 빌드를 할 분은 많지 않을 것 같지만, 정리해보면 [깃허브](https://github.com/ArduPilot/ardupilot)에서 소스코드를 받아서 [데브 사이트](http://ardupilot.org/dev/index.html)의 내용을 따라 빌드하면 된다. 

### GCS(Ground Control Station) 이용  

보통 [동영상](https://www.youtube.com/watch?v=ukfBFxXBF1w)을 보고 따라한다. 한글로 된 [사이트](http://www.internetmap.kr/entry/Loading-Firmware-Onto-Pixhawk)도 많으니 참고하면 된다. 

### Calibration 

Firmware 업로드 후, Sensor, Flight Mode등 기본적인 Calibration이 끝나면 FC 위에 보이는 LED가 파란색(GPS Loking이면, 녹색)으로 되어 있을 것이다. 하지만, 비행 전에 꼭 하나 더 해야 하는 것이 있는데 바로 **"ESC Calibration"**이다. 안하고 처녀 비행을 시도하면, 모터 중 하나가 제대로 돌지 않아 이륙 하자마자 뒤집어지는 경우도 발생한다. [동영상](https://www.youtube.com/watch?v=Hl-Q7RPOn18)을 보고 순서대로 따라하거나 공홈에서 설명한 내용대로 하면 된다.  
  
마지막으로 [Auto-trim](http://www.internetmap.kr/entry/Basic-Tuning-For-My-2nd-Quadcopter-using-Pixhawk)과 auto-tune을 진행하면 되는데, Pixhawk 기본 셋팅이 워낙 좋아서 하지 않아도 비행에는  무리가 없다.  

## Raspberry Pi3 Setting  
  
### Raspbian 설치  

**Download :** [official site](https://www.raspberrypi.org/downloads/raspbian/)에서 받아서 unzip 

**SD card format :** SD Format 프로그램으로 포맷을 한다.    

**Flashing image :** 아래 방법으로 Flashing   

	diskutil list  <= disk name of your sd card formatted  
	diskutil unmountDisk /dev/disk2 <= unmount
	sudo dd bs=1m if=2016-05-10-raspbian-jessie.img of=/dev/rdisk2 <= flashing the image downloaded  

  
**WiFi 셋팅 :** GUI에서 오른쪽 윗 부분의 WiFi Icon을 클릭하여 하는 방법도 있고, 아래와 같이 명령어로도 가능하다. 마지막에  **"}"** 빼먹지 말자. 30분 삽질한 아련한 추억이... ㅠ.ㅠ 

	sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
	
	# 파일에 아래 내용 추가   
	network = {
		ssid="Your SSID"
		psk="Your Password"
	}


**한국어 지원 :** [사이트](http://blog.naver.com/6533574/220709666224) 참고해서 적용  


### Pi Camera 

**구입하기 :** [$21.24](http://www.aliexpress.com/item/New-Camera-Module-Board-5MP-160-Degree-Wide-Angle-Fish-Eye-Lenses-For-Raspberry-Pi/32375046422.html?spm=2114.30010308.3.174.Bd88UV&ws_ab_test=searchweb201556_0,searchweb201602_3_10017_10021_507_10022_10020_10009_10008_10018_10019_101,searchweb201603_2&btsid=0cd8b287-433e-42f9-bbef-bce917d18a6c)로 알리에서 구입 가능하다. 2주만 참을 수 있다면... ㅎㅎ  

**연결 및 셋팅 :** 이 글 보는 분이라면 케이블 연결은 무리 없이 할 수 있다고 가정하고 아래 내용으로 세팅하면 된다.   

	sudo raspi-config  
	-> Choose "6 Enable Camear"  
	-> Select "Enable"
	-> Then, reboot  
  
**테스트 :** 라즈비안에서 제공하는 명령어로 확인 가능 

	raspistill -o filename.jpg
	raspistill -vf -hf -o filename.jpg -> when your cam is placed upside down
	raspivid -o video.h264
  
### GStreamer  

Gstreamer를 이용해서 스트리밍을 할 예정이므로 설치가 필요하다. 아래 명령어로 안되면 [다음 사이트](http://blog.gbaman.info/?p=150)나 구글링해보자. ㅎㅎ   

	sudo apt-get install gstreamer1.0  

**OSX :** 랩탑에도 필요하기 때문에 설치하도록 하자.    

	brew install gstreamer gst-libav gst-plugins-ugly gst-plugins-base gst-plugins-bad gst-plugins-good  

### 라즈베리파이3와 Pixhawk연동    

[ardupilot site](http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html) 에 설명이 잘되어 있다. 하지만, 이전 버전의 RPI로 썼기 때문에 라즈베리파이3와 연결하기 위해 몇 가지 변경사항이 있다. 먼저 공식 사이트 부터 살펴보고, 틀린 부분만 적용하면 될 것이다.   

**패키지 설치 :** Mavproxy.py를 주로 사용할 것이므로 관련 Python 라이브러리 설치가 필요하다.  
  
	sudo apt-get update
	sudo apt-get install python-matplotlib python-serial python-wxgtk2.8 python-lxml
	sudo apt-get install python-scipy python-opencv ccache gawk git python-pip python-pexpect  
	sudo apt-get install screen python-dev libxml2-dev libxslt-dev
	
	sudo pip install pymavlink
	sudo pip install mavproxy  
  
**시리얼 포트 enable :** 공식 사이트에는 Disable하라고 되어 있는 RPI3에서는 Enable해야 동작한다. "raspi-config"의 advanced 항목에서 하면 된다. 
  
**/dev/ttyS0**이 시리얼 포트 이름이다. 하지만, mavproxy.py가 자동적으로 찾아주기 때문에 따로 하지 입력하지 않아도 된다.  

**연결 확인 :** 모든 것이 제대로 되었다면 간단히 아래 명령으로 픽스호크와 통신 상태를 확인 할 수 있다.  

	sudo mavproxy.py
	# 정상적이라면 여러 데이터들이 출력 되며, 모드 변경이 가능하다. 
	mode poshold <= poshold로 flight mode 변경 

모든 준비는 끝났고, 간단한 파이손 프로그래밍으로 Wibro 드론을 날릴 수 있다. :) 

## 동작 다이어그램  

전체 동작은 아래 다이어그램과 같이 된다. 그림에는 설명을 빠트렸는데, 비행체(드론)에 있는 Pixhawk와 RPI3은 **시리얼 포트**로, RPI3와 와이브로 동글은 **WiFi** 연결되어 있다.  
  
![](https://dl.dropboxusercontent.com/u/18180490/wibro-connected-drone.png)  
  
얼핏 보면 왜 이렇게 복잡하게 하였을까 생각이 든다. 나도 이 별 것 아닌 것을 위해 이렇게 복잡하게 하기는 싫었다. 하지만, RPI3와 와이브로 동글이 WiFi로 연결되면서 NAT에 문제 해결 방법이 마땅치 않은 것이다. 거기에 자주 바뀌는 IP를 처리하기 위해 부득히 하게 Relay 서버를 두었다.  

### Hole Punching  

UDP통신에서 NAT 문제 해결을 위해 사용할 수 있는 방법이다. [간단한 파이선 소스코드](https://github.com/supertigim/hole_punch)를 찾아서 테스트 해보니, 재미있게도 와이브로 동글에서는 동작이 되었으나, 아이폰으로 한 LTE연결은 동작이 되지 않는 것이다. 결국 포기~ ㅎㅎ  

### Relay Server 

UDP-UDP, TCP-TCP간의 Relay는 간단하고 구현한 소스 코드가 있어 적용하기 쉬우리라 생각했는데, 불행하게도 GCS와 Relay 서버 간의 UDP통신이 안되는 문제가 발생했다. GCS는 한번이라도 데이터를 받은 후에야 회신을 하고, Relay서버는 한번이라도 데이터가 들어와야 Client를 인식하게 되어 있으니 서로 눈 뜬 장님 처럼 가만히 있는다. 결국, Relay 서버와 GCS 사이는 TCP로 구현을 했다. [소스코드](https://github.com/supertigim/hole_punch/blob/master/relayd-tcp-udp.py)는 다음과 같다.  

	from socket import *
	from select import select
	
	DEFAULT_PORT    = 4653
	MAVLINK_LENGTH  = 263

	def run():
   		host = ''
   		backlog = 5
    	sock_GCS = None
    	addr_udp = None

    	# create tcp socket
    	tcp = socket(AF_INET, SOCK_STREAM)
    	tcp.bind(('',DEFAULT_PORT))
    	tcp.listen(backlog)

    	# create udp socket
    	udp = socket(AF_INET, SOCK_DGRAM)
    	udp.bind(('',DEFAULT_PORT))

    	input = [tcp,udp]

    	while True:
			inputready,outputready,exceptready = select(input,[],[])
			
		for s in inputready:
			if s == tcp:
				sock_GCS, addr = s.accept()
				input.append(sock_GCS)
			elif s == udp:
				data, addr_udp = s.recvfrom(MAVLINK_LENGTH)
                if sock_GCS is not None and data is not None:
                    sock_GCS.send(data)
				else:
                data = sock_GCS.recv(MAVLINK_LENGTH)
                if addr_udp is not None and data is not None:
                	udp.sendto( data, addr_udp )
                
	if __name__ == '__main__':
		run()  
  
코드는 개판이다. Python은 며칠 본거라 에러 처리라고는 눈꼽만치도 없다. 동작만 확인하면 될 것이다.  

### Streaming  

흐흐... 하하...  
Streaming은 Relay서버를 거치면서 패킷이 새는지 반나절의 시도에서 실패하고 결국 RPI3와 MAC을 직접 연결 시켰다. 불행 중 다행으로 Wibro에 DMZ기능이 있어서 포트 포워딩이 되더라... 릴레이 서버 왜 만들었누.... ㅠ.ㅠ  

### 동작 시나리오  

총 5개의 터미널 열어서 한다. 릴레이 서버 없으면 한 개 줄고... -_-;; 

우선, 맥에서 터미널 실행, ssh로 릴레이 서버 구동한다. 
  
	python relayd-tcp-upd.py
	
터미널 두개를 더 실행하여 RPI3에 접속한 후,  

	# 1번째 터미널에서 
	sudo mavproxy.py --out relay_server_ip:4653
	
	# 2번째 터미널에서  
	sudo aspivid -t 0 -h 300 -w 400 -fps 15 -hf -vf -b 2000000 -o -| gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! gdppay ! tcpserversink host=RPI3_IP port=4653

4번째 터미널은 스트리밍을 받기 위해,  
  
	gst-launch-1.0 -v tcpclientsrc host=125.152.240.156 port=4653 ! gdpdepay ! rtph264depay ! avdec_h264 ! videoconvert ! osxvideosink sync=false

마지막으로 APM Planner2를 실행한 후, communication에서 tcp로 relay서버를 연결하면, 모든 셋팅 클리어~~~ auto mission으로 실행 시켜보고, 날아가는 화면을 Wibro를 통해서 (정확히는 WiFi+Wibro+LTE를 통해) 스트리밍으로 감상할 수 있다.  

## 마무리  

삽질 삽질 이런 삽질이 없다 싶은데도 정리가 필요하여 작성했다. 필요한 작업은 python으로 된 코드를 c++로 변경하고, unity3d나 unreal로 간단한 gcs를 꾸며 보는 것이다. 그 이후는 멀티 드론 작업을 해볼 생각이다. 드론이 한 3대 정도 있었으면.... -_ㅠ....  
  
## 기타 팁  

- 라즈베리파이와 픽스호크를 연결하면, 초기에 Garbage값이 계속 올라온다. 짭퉁이라 생기는 것인지 원래 그런 것인지 모르겠는데, 1분 정도 두면 괜찮아진다. 이유는 아직까지 잘 모르겠다.   
 










  