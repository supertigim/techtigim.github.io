--- 
layout: post  
title: 도커로 owncloud 설치하기    
tags: technology      
class: post-template
subclass: 'post tag-technology'
excerpt: 도커는 한 번 쓰면 빠져나오길 힘들만큼 편하다. owncloud도 예외 아님     
author: tigim            
---  

## owncloud with docker 

### 1. Install 

	sudu apt-get install docker.io

### 2. Run 
  
	sudo docker run -d -p 8080:80 -v /home/somewhere:/var/www/html/data --name="tigim_cloud" owncloud
  
-d : 백그라운드 실행. (Foreground옵션은 -i?)  

-p : Linux 8080포트 - 컨테이너 80번 포트를 연결  

-v : 도커 내부의 /var/www/html/data와 도커 외부의 /home/somewhere을 연결. /var/www/html/data는 owncloud에 업로드된 모든 물리적 데이터가 저장

-name : 컨테이너의 이름   

Nginx + HTTPS와 연동 할 예정이므로, 외부 port는 그냥 냅뒀다. 아니면, 8080포트 대신, 80포트를 이용해야 외부 접근이 용이.   
  
### 3. Auto-Run when booting  
  
I don't need this feature yet~ :)  

### 4. NginX + ufw 

	#!/bin/bash

	# change host name
	hostname $1
	echo $1 > /etc/hostname
	sed -i 's/debian/$1/g' /etc/hosts

	# install necessary packages
	apt-get install -y nginx ufw
	
	# configure ufw
	ufw disable
	ufw default deny incoming
	ufw default allow outgoing
	ufw allow 80  
	ufw allow 443  
	ufw allow 22  
	ufw enable  

### 5. NginX Setting

To solve the error when uploading bigger than 1M file, Add following line to http{..} block in nginx config:

	http {
		#...
        client_max_body_size 512m;
		#...
	}

### letsencrypt 

Make sure that 80 port has to be open.  
  
add a sub-domain (rm -rf ./* in /etc/letsencrypt/archive, /etc/letsencrypt/live, and /etc/letsencrypt/renewal first)   
  
	sudo letsencrypt certonly --expand -d tigiminsight.com -d cloud.tigiminsight.com -d music.tigiminsight.com -d torrent.tigiminsight.com 
  
### 3. ETC  

When you immigrate your data or files from other system, you need to update your database like below.  

	sudo docker exec -u www-data tigim_cloud php occ files:scan --all  
	
## Reference  
- [Docker로 사용하는 owncloud](https://blog.iwanhae.ga/owncloud_using_docker/)
- [Using OCC command](https://doc.owncloud.org/server/9.0/admin_manual/configuration_server/occ_command.html)  
- [Docker 사용해보기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter03)  
- [이미 만든 컨테이너의 포트 변경 방법 / 댓글확인](https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container)
- [starssl에서 인증서 받기](https://www.xetown.com/slope/135905)
- [키에서 PEM암호 삭제하는 방법](https://futurestud.io/tutorials/how-to-remove-pem-password-from-ssl-certificate)
- [startssl 설정하는 것인데, 인증회사 문제로 크롬에서 접근 불가 ㅎㅎㅎ](https://www.linode.com/docs/web-servers/nginx/install-nginx-and-a-startssl-certificate-on-debian-8-jessie)
- [Increase file upload size limit in PHP-Nginx](https://easyengine.io/tutorials/php/increase-file-upload-size-limit/)
- [crontab에 등록하여 자동으로 업뎃](https://extrememanual.net/10030)
- [nginx 리버스 프록시 https 셋팅](https://deviantengineer.com/2015/05/nginx-reverseproxy-centos7/)