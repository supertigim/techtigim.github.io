--- 
layout: post  
title: Hugo Static Web Engine  
tags: hugo golang web_engine bitbucket    
excerpt: Go언어로 개발된 정적 웹 엔진으로 무료 블로그 운영해보자       
---  

깃허브에 이미 Jekyll기반  Blog를 운영하고 있지만, 개인 생활에 관련 된 내용 정리가 필요할 것 같아 찾아보다 발견한 매우 유용한 정적 웹 엔진  

## [Hugo](https://gohugo.io/)란?  
![hugo logo](https://gohugo.io/img/hugo.png)  

  
markdown기반의 정적 웹엔진이다. 해외 블로그 엔진 순위에서도 요즘 1위일 만큼, 인기가 많은 것 같다. theme개발도 많이 되어 있어 편리하게 사용 가능하다.  

Jekyll과 같은 스크립트 타입의 언어가 아닌, Go언어로 작성되어 무척이나 가볍고 빠르면서도 설치, 운영 또한 매우 간단하다. 무엇보다도 Hugo와 함께 [Bitbucket](https://bitbucket.org/)+[aerobabittic](https://www.aerobatic.com/) 조합으로 무료 블로그 운영이 가능하다. 

### 설치 방법  

[소스코드](https://github.com/spf13/hugo)도 공개되어 있어 직접 Build하여 사용해도 되고, Brew(맥의 경우)로 인스톨하여 사용도 가능하다.  

	brew update
	brew install hugo  
  
나는 가능하면 최신 버전을 사용하고 싶어 Brew로 설치한 다음 직접 빌드한 바이너리로 변경했다. 

	cp ./hugo /usr/local/bin

### 사용방법   

Go로 빌드해보면 알지만, **hugo** 파일 하나다. 이 파일을 이용하여, 사이트 구축, 포스트 작성 등 대부분의 것을 다 할 수가 있다. 설치-사이트생성-포스트생성까지 2분도 채 걸리지 않는다. [퀵스타트](http://gohugo.io/overview/quickstart/) 페이지에 간단히 설명 되어 있는데, 간단히 정리해 본다.  

	hugo help  <== 사용 방법이 주르륵 나온다  
	hugo new site myblog <== myblog 사이트 생성
	hugo new post/helloworld.md <== /post/helloworld 포스트 생성 
	hugo server -w   		<== localhost:1313 접속하여 확인  

### theme 적용  

위방법으로 사이트를 생성했다면, 폴더 트리 중에 themes라고 생긴다. 여기에 사용하고 싶은 theme을 clone한 후 설정하여 사용하면 된다. git은 이미 설치 되어 있다고 가정하고, [Ghost](https://ghost.org/)의 default theme인 casper를 적용해 보자.      

	git clone https://github.com/vjeantet/hugo-theme-casper.git ./themes/casper   
	
설정파일 **config.toml**에 아래 내용 추가 한다음, 재실행 하면 끝~!

	theme = "casper"  

## 무료 블로그 운영  
  
무료 블로그를 할 수 있다는 것이 참 매력적이다. [hugo 튜토리얼](http://gohugo.io/tutorials/hosting-on-bitbucket/)에 잘 설명되어 있는데, 추가로 필효한 부분에 대해 정리해 본다.   

### 추가 파일  

없어도 되지만, 있으면 한결 편리하게 사용할 수 있다.  

1. .gitignore 
  
		backup
		public/*
		themes/casper/tmp
		.DS_Store

2. .gitmodules  

		[submodule "themes/casper"]
			path = themes/casper
			url = https://github.com/vjeantet/hugo-theme-casper.git  
			
3. README.md  

		my blog :)  
		
4. package.json  

		{
		  "_aerobatic": {
		    "build": {
		      "engine": "hugo",
		      "themeRepo": "https://github.com/vjeantet/hugo-theme-casper.git"
		    }
		  }
		}  

5. favicon.ico  

		/static/images/favicon.ico  
  

### SourceTree  

맥에 설치하는 프로그램인데, 나처럼 CLI에 능숙하지 않은 사용자라면, 사용하길 추천한다.  

### aerobatic add-on 설치  

메뉴 찾기가 어렵다. 셋팅의  **Find integrations**에서 찾아 설치하면 된다. 근데, 찾을 필요 없이 첫번째 것이다. :) Add후에 리파지토리에 가보면, 왼쪽 아래에 지구본 모양의 아이콘으로 aerobatic 실행하면 된다.  
  
aerobatic 메뉴에서 "myblog"로 웹호스팅 이름 시작하면, 빌드 후에 **production** 상태에서 아래 주소로 확인 가능하다. 
  
	myblog.aerobatic.io  
    
### 다른 도메인 연결 방법    
  
aerobatic 메뉴의 **Hosting settings**에서 설정 할 수 있다. 내용은 [Custom Domains and SSL](https://www.aerobatic.com/docs/custom-domains-ssl)에서 확인 가능하지만, 기존의 경험이 없다면 조금 복잡할 수 도 있다. 문제 없이 했는데도 안되면 [custom domain not working](https://aerobatic.atlassian.net/wiki/display/AKB/Custom+domain+not+working) 사이트로 해결 가능하다.  
