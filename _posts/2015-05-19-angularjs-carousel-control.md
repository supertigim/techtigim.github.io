---
layout: post
title: AngualrJ UI Bootstrap의 Carousel 사용시 문제 해결
tags: angular bootstrap carousel animate collision 
---

<br />
최근의 웹 개발에서 bootstrap을 안쓰는 것이 더 어려울 만큼 인기가 높다. 나 같이 웹의 지식이 전무한 상태에서 시작했고, 아는 웹디자이너가 없을 때는 이런 framework은 더욱 절실하다.

하지만, 깔끔히 이용하기에는 무리가 있음을 기억하자.

어제는 슬라이드 기능 구현을 위해 [carousel](http://angular-ui.github.io/bootstrap/#/carousel)를 이용하려고 했는데, 공식 페이지에 나와 있는 가이드 라인 준수했음에도 불구하고 동작을 하지 않는 것이다. 결국 몇 시간에 구글링을 통해서 workaround를 찾아냈다. 바로, ngAnimate와 충돌로 문제가 발생 한 것이다.

먼저, 기본 설정이다.

```
angular.module('myModule', ['ui.bootstrap','ngAnimate']);
```
css 파일에 다음을 포함. (안 넣어도 동작하나 기본 문서에서 넣자고 하니.. ㅎㅎ)  

```
.nav, .pagination, .carousel, .panel-title a {
    cursor: pointer;
}
```
이렇게 한 후 샘플 코드를 넣으면... 짜잔!!! 하고 안된다!!! ㅋㅋㅋ
두번째 그림에 더이상 슬라이드가 안되는 것이다. 웃기는 짬뽕이 아닐 수 없다.

구글링을 해보니 나같은 이가 한 둘이 아니다. [공식 깃허브](https://github.com/angular-ui/bootstrap)에도 [이슈](https://github.com/angular-ui/bootstrap/issues/1350)로 등록 되어 있다.

뭐 어렵지는 않다.
controller.js에 파일에 다음과 같이 directive를 정의 하자.

```
angular.module('demoApp')
  .controller('MainCtrl', function ($scope, $interval) {
	....
  })
  /* to fix collision between bootstrap-carousel and ngAnimate */
  .directive('disableNgAnimate', ['$animate', function($animate) {
  return {
    restrict: 'A',
    link: function(scope, element) {
      $animate.enabled(false, element);
    }
  };
```

html파일에   "disable-ng-animate" 추가!!

```
<carousel interval="myInterval" disable-ng-animate>
            <slide ng-repeat="slide in slides" active="slide.active">
              <img ng-src="{{slide.image}}" style="height: 305px;">
              <div class="carousel-caption">
                <p>[{{slide.title}}]</p>
                <h2>{{slide.text}}</h2>
                <p>{{slide.details}}</p>
              </div>
            </slide>
          </carousel>
```

###참고
carousel의 캡션 특성을 바꾸려면, css에서 다음과 같이 수정해보자.

```
.carousel-caption {
  padding-top: 0px;
  background: rgba(0, 0, 0, 0.4);
  }
```
