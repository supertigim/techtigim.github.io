--- 
layout: post  
title: (번역) docker로 openvpn 설치 하기  
tags: openvpn docker  
excerpt: source build의 생지옥을 벗어나 도커로 5분만에 openvpn 설치 완료  
---  

엔터프라이즈 버전이 있어서인지 native 설치 지원(?)에 매우 인색하다. iptable을 건드리고, 어쩌구 하다보니 서버가 맛이 가서 결국 roll-back을 하고 도커로 깔끔하게 마무리 했다. 도커 만세!!! :)  
  
## 방화벽에서 port 오픈    

방화벽을 enable 했으면, openvpn이 사용하는 포트를 열어두자.  
  
	sudo ufw allow 1194  
	
## docker & docker compose	 

도커는 몇 번 사용해서 있었는데 docker compose는 뭔지... 일단 설치!  

	sudo apt-get install docker # docker 없으면!!!  
	sudo apt-get install docker-compose  
	
## compose 파일 생성  

compose를 이용하여 만들려면 .yml로 되어있는 템플릿 파일이 필요하다. 중요한 것은 **이후 docker 관련 명령 및 실행은 이 파일이 있는 디렉토리**에서 해야한다.   

	touch docker-compose.yml  
	vi	docker-compose.yml
	
아래 내용을 파일에 복사한다. udp 대신 tcp를 사용하고 싶으면 변경하면 되지만, 대부분 udp 쓴다. 속도도 조금더 빠르고...    

	version: '2'  
	services:  
  		openvpn:  
    		cap_add:  
     		- NET_ADMIN  
    		image: kylemanna/openvpn  
    		container_name: openvpn  
    		ports:  
     		- "1194:1194/udp"  
    		restart: always  
    		volumes:  
     		- {path_to_save_openvpn_config}:/etc/openvpn  

{path_to_save_openvpn_config}는 본인 환경에 맞게 하면 된다. 예를 들어,

	- /home/root/openvpn :/etc/openvpn   
  
## openvpn docker 생성 certificate 생성  
	
	docker-compose run --rm openvpn ovpn_genconfig -u udp://{vpn_server_address}  
    docker-compose run --rm openvpn ovpn_initpki  
    

{vpn_server_address}는 본인 서버의 ip address나 도메인 주소를 넣으면 된다.   

두번 째 라인에서 인증서를 생성할 때, 암호(원하는데로 넣으면됨)를 두번 넣고 원하는 서버 이름을 넣는다. 인증서 생성에는 시간이 좀 걸리니까 기다린다.    

![](https://blog.ambar.cloud/content/images/2017/04/generate_key.gif)  
  
## openvpn 시작  
  
	docker-compose up -d openvpn    
 
템플릿이 있는 폴더에서 실행해야 한다. 잊지말자~ ㅎㅎㅎ  


## 클라이언트에서 사용하는 인증서 생성  

openvpn은 인증서로 클라이언트에서 연결 할 수 있다. native로 설치할라 하면 이것도 개고생인데, 도커는 정말 사랑이다.. !!! -_-;  

	ocker-compose run --rm openvpn easyrsa build-client-full {client_name} nopass  
	
    
실행하면, 서버 인증서를 생성할 때 넣은 암호를 넣으면 된다. 꼭! Signature OK! 확인하자!!! {client_name}는 원하는대로 정한다. 나의 경우, 서버 이름과 동일하게 하니 안되서 다른 이름으로 클라이언트 인증서를 새로 생성했다. 다른 이름으로 여러개 만들어 동시에 연결할 수도 있다. 굳굳!!! :)    
  
마지막으로 .ovpn 파일이 있어야 설치가 된다. 이동은 이메일/ftp/dropbox등으로 하면된다.    

	docker-compose run --rm openvpn ovpn_getclient {client_name} > {client_name}.ovpn  
	
![](https://blog.ambar.cloud/content/images/2017/04/generate_client_key.gif)  

## iPhone에서 사용  

앱스토어에서 openvpn을 설치한다. 위에 생성한 인증서를 불러온다. 프로파일 설치 완료. 연결하기. 외국에서 접근이 되지 않았던 많은 컨텐츠를 신나게 감상(?)한다.  
  
네이티브로 설치한다고 하루를 날렸으나, 이걸로 여기까지 걸린 시간은 대략 5분이었다... ㅜ.ㅜ     
  
## Reference  

- [영문 원문](https://blog.ambar.cloud/tutorial-set-up-openvpn-with-docker-compose/)  
- [Github](https://github.com/kylemanna/docker-openvpn/blob/master/docs/docker-compose.md)  
- [Digital Ocean에서 제공하는 openvpn 설치가이드](https://www.digitalocean.com/community/tutorials/how-to-run-openvpn-in-a-docker-container-on-ubuntu-14-04)  
  

 