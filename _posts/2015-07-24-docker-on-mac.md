--- 
layout: post 
title: mac에서 docker 설치  
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---  

블로그 따라 하다가 안돼서 나름 정리. 

### 1. Mac용 Docker 설치 파일  

공식 사이트에 가서 [Download](https://github.com/boot2docker/osx-installer/releases/tag/v1.7.1)  

### 2. 설치 후 Upgrade  

Launchpad에 생긴 boot2docker 클릭하면, 알아서 ssl용 키 설정등을 하면서 설치를 마무리한다. 정상적으로 끝나면, bash command line이 보인다.    

이유는 잘 모르겠지만, 여기서 정상 동작하지 않아 업그레이드도 해줬다.   


    $ boot2docker stop 
    $ boot2docker upgrade
    $ boot2docker start

### 3. nginx container 실행 해보기  

https://docs.docker.com/installation/mac/ 페이지의 내용을 따라하면 되는데, 일단은 다음의 명령으로 boot2docker가 정상 설치 되었는지 확인한다.  

    $ boot2docker status
    $ docker version  

그리고 nginx container를 실행해 보자.  

    $ docker run -d -P --name web nginx  

web이라는 이름으로 nginx docker image를 실행한다는 의미. local에 없기 때문에 자동으로 registry.docker.com으로 접속하여, nginx image를 다운로드 한다. 파일이 커서 시간 좀 걸린다. 다운로드가 완료 되면,  실행까지 자동으로 되는데 아래 커맨드로 

    docker ps -a

local에 container가 생성된 것을 확인 할 수 있다.  

    CONTAINER ID    IMAGE           COMMAND                          
    5fb65ff765e9    nginx:latest    "nginx -g"  

이제 동작을 확인해야 하는데, boot2docker는 포트 확인 까다롭다. 일단 기본적인  포트 확인 방법은 다음과 같다.  

    $ docker port web
    443/tcp -> 0.0.0.0:49156
    80/tcp -> 0.0.0.0:49157

run 할때, container 이름을 web으로 해주었기 때문에 이렇게 한 것이다.  

> Docker가 처음이라면 image와 container가 혼동 될 것이다. 쉽게 설명하면, image는 실행파일이고 container는 데몬 또는 프로세스라고 생각하면 된다. 더 궁금하면 구글링 추천~!!! :)  

하지만, 브라우저에서 http://localhost:49157 (80포트 번호) 입력을 해도 안나온다. ㅋㅋㅋ  

    $ boot2docker ip
    192.168.59.103

위의 명령어를 치면, ip가 주소가 반환이 되고 이 주소를 입력해야 화면이 보인다. **192.168.59.103:49157**   

### 4. 기타 명령어  

/Users 디렉토리 공유가 가능하므로 /site에 index.html을 만들어서 시작 페이지를 변경할 수도 있다. 

    docker run -d -P -v $HOME/site:/usr/share/nginx/html --name mysite nginx`

컨테이너 정지  

    docker stop mysite  

컨테이너 삭제  

    docker rm mysite  

이미지 삭제  

    docker rmi nginx:latest  



