---
layout: post
title: MEAN Stack main page에 Slide 헤더 넣기
tags: technology  
class: post-template
subclass: 'post tag-technology' 
author: tigim 
---

MEAN 스택이라고 했지만, 사실 [angular fullstack](https://github.com/DaftMonk/generator-angular-fullstack)를 사용한다. 가벼울 뿐만 아니라 로그인 처리가 잘되어 있고 디렉토리 구성도 깔끔해서 다른 MEAN 연습 하다가 이리로 넘어왔다.

설치는 간단하다. 아래 방법 말고 위 사이트에서 받아도 되지만, 아래가 더 간편~~ :)
 1. [데모 버전](https://github.com/DaftMonk/fullstack-demo)을 Fork (안하고 바로 local에 복사해서 사용 가능)
 2. Local에 clone
 3. npm install & bower install
 4. grunt serve

그럼 동일 환경이라는 가정 하에서,  
<br/>

[1] bower.json에  

```
	...
    "angular-ui-router": "~0.2.10",
    "angular-animate": ">=1.3.15"
  },
```
추가 한 다음, bower install 을 한번더 하자.  
<br/>

[2] **/client/assets/images** 폴더에 원하는 슬라이드 그림(같은 크기가 좋다) 추가~  

<br/>

[3] **client/app/main/main.html** 에서  

```
<header class="main-header slides-with-slogan">
  <div class="slogan">
    <div class="col-md-8 col-sm-8 col-md-offset-2 col-sm-offset-2 text-center">
        <p class="h3">
            String 1</br>
        </p>
        <p class="h1 ">
            <strong>String 2</strong></br>
        </p>
    </div>
  </div>
  <div class="fullscreen-flexslider slider">
      <img ng-repeat="photo in photos" class="slide" ng-show="isActive($index)" ng-src="{{photo.src}} "/>
  </div>
</header>
```
를 추가한다. 물론, 이미 있는 header는 삭제~!  
<br/>

[4] 다음으로 **client/app/main/main.controller.js**에  

```
.controller('MainCtrl', function ($scope, $interval) {

    // Set of Photos
    $scope.photos = [
        {src: 'assets/images/fullslider1.jpg'},
        {src: 'assets/images/fullslider2.jpg'},
        {src: 'assets/images/fullslider3.jpg'},
        {src: 'assets/images/fullslider4.jpg'},
        {src: 'assets/images/fullslider5.jpg'},
        {src: 'assets/images/fullslider6.jpg'},
        {src: 'assets/images/fullslider7.jpg'},
        {src: 'assets/images/fullslider8.jpg'},
    ];

    // initial image index
    $scope._Index = 0;

    $interval(function() {

      $scope._Index++;
        if($scope._Index + 1 > $scope.photos.length){
          $scope._Index = 0;
        }
      }, 8000); /* 인터벌 조절 */

    // if a current image is the same as requested image
    $scope.isActive = function (index) {
        return $scope._Index === index;
    };
```
<br/>

[5] **client/app/main/main.scss**에서 style 설정  


```
//--- Header (Slides with slogan) ----//
.main-header {
  height: 100%;
}

.slides-with-slogan {
  height: auto;
}

header .slogan {
  position: absolute;
  width: 100%;
  height: 270px;
  top: 40%;

  z-index: 10;
  color: #ffffff;
}
header .slogan [class^="col-"] {
  padding-left: 0;
  padding-right: 0;
}
header .slogan .h1 {
  font-size: 65px;
}
header .slogan .h2 {
  font-size: 40px;
}
header .slogan .h3 {
  font-size: 35px;
}
header .slogan strong {
  font-weight: 900;
}
header .slogan .square {
  height: 190px;
  width: 100%;
  background-color: #ce0000;
}

/* flexslider */
.fullscreen-flexslider {
  overflow: hidden;
  top:-20px;
  padding: 0;
  border: 0px;
  border-radius: 0px;
  height: 654px;
  margin: 0;
  position: relative;
  width: auto;
  max-width: 100%;
  text-align: center;

  -webkit-perspective: 1000px;
  -moz-perspective: 1000px;
  -ms-perspective: 1000px;
  -o-perspective: 1000px;
  perspective: 1000px;

  -webkit-transform-style: preserve-3d;
  -moz-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  -o-transform-style: preserve-3d;
  transform-style: preserve-3d;

}

.fullscreen-flexslider .slide.ng-hide-add {
    opacity:1;

    -webkit-transition:1s linear all;
    -moz-transition:1s linear all;
    -o-transition:1s linear all;
    transition:1s linear all;

    -webkit-transform: rotateX(0deg) rotateY(0deg);
    -moz-transform: rotateX(0deg) rotateY(0deg);
    -ms-transform: rotateX(0deg) rotateY(0deg);
    -o-transform: rotateX(0deg) rotateY(0deg);
    transform: rotateX(0deg) rotateY(0deg);

    -webkit-transform-origin: right top 0;
    -moz-transform-origin: right top 0;
    -ms-transform-origin: right top 0;
    -o-transform-origin: right top 0;
    transform-origin: right top 0;
}

.fullscreen-flexslider .slide.ng-hide-add.ng-hide-add-active {
    opacity:0;
}

.fullscreen-flexslider .slide.ng-hide-remove {
    -webkit-transition:1s linear all;
    -moz-transition:1s linear all;
    -o-transition:1s linear all;
    transition:1s linear all;

    display:block!important;
    opacity:0;
}

.fullscreen-flexslider .slide,
.fullscreen-flexslider .slide.ng-hide-remove.ng-hide-remove-active {
    opacity:1;
}
//--- End of Header ----//

```  


효과를 다르게 주려면 내용 변경 하면되나, 내가 잘모른다 -_-;; 최소한만의 지식과 통밥으로 개발~!! ㅋㅋ  
<br/>
[6] 마지막으로 **client/app/app.js** 수정  

```
  ...
  'ui.bootstrap'
  'ngAnimate',
])
```  
<br/>
[참고] 사진은 Flickr에서 무료 라이센스로 찾아서 사용하면 된다 :)  

<br/>
####안되면 음... 스스로???? ㅋㅋㅋㅋ
