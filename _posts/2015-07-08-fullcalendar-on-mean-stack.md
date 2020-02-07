--- 
layout: post 
title: Add FullCalendar feature on angular fullstack 
tags: fullcalendar mean angular fullstack node.js 
---  

카렌다 모듈 중에 제일 쓸만한 [FullCalendar](http://angular-ui.github.io/ui-calendar/)을 [angular fullstack](http://fullstack-demo.herokuapp.com/)에 추가하기  


### 모듈 추가  

bower.json 에 다음 추가 

    "angular-ui-calendar": ">=1.0.*",
    "fullcalendar": ">=2.3.1"
    
터미널에서 update 한다.

    bower update  

모듈 간 conflict가 나면, 아래 처럼 선택~

    3) angular#>=1.2.* which resolved to 1.4.2 and is required by your app name
    
    Prefix the choice with ! to persist it to bower.json
    
    ? Answer: 3
    
    Unable to find a suitable version for fullcalendar, please choose one:
    
    2) fullcalendar#>=2.3.1 which resolved to 2.3.2 and is required by your app name
    
    Prefix the choice with ! to persist it to bower.json
    
    ? Answer: 2  
  
### Basic settings  

다른 javascript는 자동으로 포함이 되지만, gcal.js에는 수동으로 index.html에 넣어주어야 한다. 이 파일은 google calendar 연동에 필요하다.  

    <script src="bower_components/fullcalendar/dist/gcal.js"></script>
  
client/app/app.js 파일에 다음 추가  

    'ui.bootstrap',
    'ui.router',
    'ngAnimate',
    'ui.calendar' /* added */  
        
### 구현    
  html 파일에 다음 추가   

    <div ui-calendar="uiConfig.calendar" class="calendar" ng-model="eventSources" calendar="myCalendar"></div>

  contrller.js에는 http://angular-ui.github.io/ui-calendar/ 내용을 추가하면 된다. 