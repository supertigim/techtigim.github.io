---
layout: post
title: Jekyll을 mac에 설치
tags: technology  
class: post-template
subclass: 'post tag-technology' 
author: tigim 
---

github에 직접 수정하는 미친 짓을 반나절 하다가, local에서 뭔가를 설치하지 않고서는 너무 불편해서 할 수 없다는 것을 깨닫고 jekyll 설치하기로 결정. 터미널 창에서 아래 프로그램을 차례로 설치.

    xcode-select --install
    \curl -sSL https://get.rvm.io | bash -s stable
    source ~/.rvm/scripts/rvm
    rvm install 2.2.0
    gem install jekyll

    gem install bundler
    gem install github-pages

이게 이렇게 써놓으니 쉬워 보이지 한참 걸렸음~ -_-;;
어째튼, 해당 폴더로 이동해서,

    jekyll serve

입력하면, 서버 실행...

Server address: http://127.0.0.1:4000/ 로 접근 가능하다. 혹시나/설마/아니겠지만 모를까바~ ㅋㅋㅋ
