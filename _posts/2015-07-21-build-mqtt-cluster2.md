--- 
layout: post 
title: (번역) IoT용 MQTT 클러스터 구축 -2/2편- 
tags: iot mqtt node.js redis haproxy nscale mosca 
---

##SSL로 MQTT 보안 설정  


지금까지, Proxy서버를 통해서 2대의 서버로 분배하는 MQTT 인프라를 구축했다. 다음으로 SSL을 사용하여 통신 보안을 설정해 보자.   

> HAProxy 1.5.x부터는 Native SSL을 지원한다.  

### SSL이란?   

Secure Sockets Layer으로 서버-클라이언트 통신 상의 모든 데이터 암호화를 위한 기본 기술로 사용된다.   

### PEM Certificate/Key 파일 만들기  

먼저 SSL Certificate이 필요하다. ([OpenSSL로 만드는 방법](http://wiki.nginx.org/HttpSslModule))  만들어진 Certificate과 Key로 PEM파일을 생성해야 한다. 인증서 이름을 lelylan.com.crt로 가정하면 다음과 같이 PEM파일을 생성할 수 있다.  

    cat lelylan.com.crt lelylan.com.key > lelylan.com.pem  

> 당연한 말이지만 Private Key 간수 잘하도록!!! ㅎㅎ  

### PEM파일을 Docker Volumes에 올리기  

주의할 것은 PEM을 Public Repo에 올리지 않도록 한다. :) 보안!!! 다음으로 해야할 일은 HAProxy 서버에 파일을 올리는 것이다. 여기서 접근성을 위해 Data Volumes에 저장하자.   

**Docker Data Volumes란?**  

컨테이너간 데이터 공유를 위해 특별히 만들어진 Directory이다. -v 옵션을 통해서 파일 또는 폴더를 컨테이너와 공유할 수 있다.  

**Data Volumes으로 PEM파일 공유하기 **  

위에서 만든 SSL인증서를 /certs에 저장한다. (물론 HAProxy server에서) 아래의 명령어를 토해서 접근 가능하게 만든다.  

    $ docker run -d -p 80:80 -p 443:443 -p 1883:1883 -p 8883:8883 \
    -v /certs:/certs
    -v /root/haproxy-override:/haproxy-override  

> 8883 포트 여는 것을 잊지말자  

**인증서 로딩**  

Data Volumes를 통해서 인증서를 공유했다면, HAProxy Configuration 파일을 통해서 접근 가능하다. 한 줄만 추가하자!  8883 포트를 인증서로 맵핑하면 된다.  

    # Listen to all MQTT requests
    listen mqtt
    
    # MQTT binding to port 1883
    bind *:1883
    
    # MQTT binding to port 8883
    bind *:8883 ssl crt /certs/lelylan.pem
    ...

** 끝~ **

이제 다 되었고, Deploy이 하는일만 남았다.   

----------
## 자동 deployment를 위한 nscale 설정   

nscale로 서로 연결된 컨테이너 셋을 build & deploy해보자.  

> 여기서는 몇가지 필요한 명령어만 사용할 예정이므로, nscal의 자세한 내용은 [여기](https://github.com/nearform/nscale-workshop)를 참고하자.   

### 어디에 설치할 꺼임??  

여기서는 [Digital Ocean](https://www.digitalocean.com/)을 기준으로 할 것이다.  사용할 모든 droplet(하나의 서버라고 생각하면 된다) 우분투 기반으로 이미 Docker가 설치 된 것으로 한다. 

![enter image description here](https://d262ilb51hltx0.cloudfront.net/max/2000/1*vmtoLnk83QKvbuej0K9JMQ.png)  
  
여기서 할 것은 모두 5개의 Droplet을 생성하는 것이다.  하나는 전체 셋을 관장하는 매니지먼트 서버, 2대는 모스카 MQTT서버, HAProxy와 Redis서버를 각 한대씩 생성한다. 

![enter image description here](https://d262ilb51hltx0.cloudfront.net/max/2000/1*C_NhlKIiOWKxh8zGzpH9vg.png)  
  
여기서, nscale은 매니지먼트 서버에 설치 할 것이다.  관리 서버는 따로 두지 않아도 되겠지만, 변경에 대한 팀전체 경우를 위해서 하나를 따로 생성하였다.   

**설치**  

node.js를 설치하고 npm(node package manager)를 통해서 nscale을 설치한다.  

    # install needed dependencies
    apt-get update
    apt-get install build-essential
    
    # install node and npm
    nvm install v0.10.33
    nvm alias default v0.10.33
    npm install npm@latest -g --unsafe-perm
    
    # install nscale
    npm install nscale -g --unsafe-perm  

**Github user configuration**  

사용하려면 Github 유저 설정이 필요하다. (왜? -_-;;)  

    git config --global user.name "<YOUR_NAME>"
    git config --global user.email "<YOUR_EMAIL>"  

**nscale 프로젝트 생성**  

위 설정이 끝나다면, nscale로 로그인 하자. (잉??)  

    $ nsd login  

처음 하는 것이라면 아래와 같이 질문이 나온다. 네임이나 네임스페이스나 동일하게 해도 된다.  
  
    $ nsd sys create 
    1. Set a name for your project: <NAME>
    2. Set a namespace for your project: <NAMESPACE>  

이렇게 하고 나면 자동으로 프로젝트 폴더와 파일들이 생성되는데, 다른것은 신경 쓰지말고, definition/폴더의 services.js 와 system.js 보면 된다.  

    |— definitions
    | |— machines.js
    | `— services.js *
    |— deployed.json
    |— map.js
    |— npm-debug.log|— README.md
    |— sudc-key
    |— sudc-key.pub
    |— system.js *
    |— timeline.json
    `— workspace
    ...  

정상적으로 생성 되었다면, 아래 명령어로 프로젝트 이름과 ID를 볼 수 있다.  

    $ nsd sys list
    Name         Id 
    lelylan-mqtt 6b4b4e3f-f22e-4516-bffb-e1a8daafb3ea  

**Secure Access (nscale에서 다른 서버로)**  

접속을 위해서는 새로운 SSH키가 필요한데, passphrase가 없는 것으로 생성한다.  

    ssh-keygen -t rsa  

"no passphrase"라고 치고, 프로젝트 이름으로 저장한다. 여기서는 lelyan-key라고 한다. (생성된 키는 nscale 프로젝트 root에 있어야 한다) 일단, 키를 만들었으면 모든 서버(haproxy, mosca 1, mosca 2, redis)에 공개키를 셋팅하자.  이것은 Digital Ocean의 대시보드에서 하거나, 아래의 명령어로 가능하다.  

    cat lelylan-key.pub | \
      ssh <USER>@<IP-SERVER> "cat ~/.ssh/authorized_keys"  

**SSH Agent Forwarding**  

매니지먼트 서버에서 하나를 더 해야하는데, 바로 SSH Agent Forwarding이다. (뭔지 모르겠음 -_-;;)  

    # ~/.ssh/config
    Host *
      ForwardAgent yes  

> 이것을 하는 이유가 nscale에 [문제](https://github.com/nearform/nscale/issues/47)가 있어서인데, Fix가 되었다면 안해도 될 것이다. (이미 closed가 되었다)   

###nscale 설정  

이제 설정을 해보자. 먼저 nscale analyzer부터 하는데, 타겟 서버 접근 에 대한 내용이다. ~/.nscale/config/config.json의 파일을 다음과 같이 편집하자.   

from:  

    {
     ...
       "modules": {
           ...
           "analysis": {
              "require": "nscale-local analyzer",
              "specific": {
              }
           }
           ...
    }  

to:  

    {
     ...
       "modules": {
           ...
           "analysis": {
              "require": "nscale-local analyzer",
              "specific": {
	              "user": "root",
	              "identityFile": "/root/lelylan/lelylan-key"
              }
           }
           ...
    } 
 
한번만 설정하면 다음부터는 하지 않아도 된다. (당연하잖아!! -_-;;)

###Process 정의  

여기서 말하는 Process는 도커 컨테이너로 Github repo에 있는 이름으로 정의되어 있다. 그리고 도커 실행시 사용하는 옵션도 정의한다.  

    exports.mqtt = {
	    type: 'process',
		    specific: {
		      repositoryUrl: 'git@github.com:lelylan/mqtt.git',
		      execute: {
			      args: '-e "DEBUG=lelylan" -e "REDIS_HOST=178.62.31.9" -p 1883:1883 -d'
			  }
			}
	};
	
	exports.haproxy = {
	  type: 'process',
		  specific: {
			  repositoryUrl: 'git@github.com:lelylan/haproxy-mqtt.git',
			  execute: {
				  args: '-p 80:80 -p 1883:1883 -p 8883:8883 -v /certs:/certs/ -d'
		      }
		  }
	};
	
	exports.redis = {
		type: 'process',
			specific: {
				name: 'redis',
				execute: {
					args: '-d -p 6379:6379'
				}
			}
	};

> github에 없으면, docker hub에서 가져온다. Redis의 경우이다.  

### System Definition  

이제는 운영하사는 서버에 대해서 정의할 것이다. 모든 서버는 Digital Ocean에 있고, system.js의 정의 통해서 사용한다.  

    exports.name = 'lelylan_mqtt';
    exports.namespace = 'lelylan_mqtt';
    exports.id = '6b4b4e3f-f22e-4516-bffb-e1a8daafb3ea';
    
    exports.topology = {
	    local: {
	    },
	    
	    direct: {
		    machine$1: {
			    contains: ['redis'],
			    specific: {
				    user: 'root',
				    identityFile: 'lelylan',
				    ipAddress: '178.62.31.9'
				}
			},
			
			machine$2: {
				contains: ['haproxy'],
				specific: {
					user: 'root',
					identityFile: 'lelylan',
					ipAddress: '178.62.108.47'
				}
			},
			
			machine$3: {
				contains: ['mqtt'],
				specific: {
					user: 'root',
					identityFile: 'lelylan',
					ipAddress: '178.62.122.204'
				}
			},
			
			machine$4: {
				contains: ['mqtt'],
				specific: {
					user: 'root',
					identityFile: 'lelylan',
					ipAddress: '178.62.104.172'
				}
			},
		}
	};  

위에서 보듯이 system.js에 모든 서버 셋업을 정의한다. IP, 로그인 유저, ssh 키 이름이 정의되어 있다.  

**MQTT 서버를 더 추가하고 싶다면?**  

nscale의 system.js 와 HAProxy 설정에 추가해 주자.  

**인프라 구축 (서버 배치)**  

아래의 명령어를 실행으로 간단히 MQTT 인프라가 구축된다.  

    # Compile the nscale project
    nsd sys comp direct
    
    # Build all containers
    # (grab a cup of coffee, while nscale build everything)
    nsd cont buildall
    
    # Deploy the latest revision on Digital Ocean
    nsd rev dep head

**대용량 MQTT 클러스터 완성**  

셋업이 완성되면, 위의 3 명령어 인프라 구축이 완료된다. 추가하는 것도 몇분의 노력으로 가능하다.  

----------
## 마무리  

추가해서 살펴볼만한 자료

 - [Lelylan MQTT HAProxy](https://github.com/lelylan/haproxy-mqtt) (TCP load balancer)
 - [Lelylan MQTT Server](https://github.com/lelylan/mqtt) (Mosca implementation)
 - http://www.nearform.com/nodecrunch/internet-of-things-how-to-build-it-faster/
 - http://www.hivemq.com/building-a-high-availability-mqtt-cluster/
 - https://www.digitalocean.com/community/tutorials/an-introduction-to-haproxy-and-load-balancing-concepts
 - https://github.com/nearform/nscale-docs
 - http://mmtn.borioli.net/?p=1342  
 - http://www.philchen.com/2015/05/04/how-to-setup-a-mosca-node-js-mqtt-broker-service-on-ubuntu-14-04  

