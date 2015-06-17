---
layout: post
title: 리눅스 데몬 실행
tags: linux daemon nohup
---

리눅스 명령어는 잘 까먹기도 하지!  

데몬 만들고, 서비스 등록하지 않으면서 백그라운드 실행을 위해 필요한 명령어

    sudo nohup ./start.sh &


PID는 기억해 두고 있다가, 죽일 때 사용

    sudo kill -9 1234

동작 안되면, chmod / chown 등으로 알아서 파일특성 조절
