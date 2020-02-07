--- 
layout: post 
title: Angular fullstack에서 contact message 기능 구현
tags: technology  
class: post-template
subclass: 'post tag-technology' 
author: tigim 
---

### Contact Message란?

웹사이트 돌아다니다 보면, Q&A나 질문하는 곳이 있다. 이런 곳에 메시지를 남겨서 주인장에게 알리는 기능을 말한다. 처음에는 카톡API를 이용하여 해보려고 했으나, 안된다는 것을 깨닫고 그냥 이메일을 매개체로 하기로 했다. 내가 카톡 사장이라면, Yellow ID에는 최소한 구현해 보겠다.  

### 개발 환경 

현재 개발 환경은 angular fullstack이다. 고로, angular와 노드 기반으로 작성되었으며, folk는 [여기서](https://github.com/DaftMonk/generator-angular-fullstack) 해왔다. 

### nodemailer 설치

내가 node를 쓰는 유일한 이유로 생각하는 대부분의 기능이 NPM으로 제공된다. package.json에 다음을 추가하자. 

```
"nodemailer": "^1.3.4",
```

그리고 terminal에서   

```
 npm install
```
하면 된다. 

### api 구현

로그인/보안 관련은 제외다. 아시다시피 fullstack angular는 api 모듈 생성도 scaffold로 제공된다.  

``` 
 yo angular-fullstack:endpoint message
```

이렇게 하면, server/api/ 밑에 message라는 폴더와 관련 파일들이 이쁘게 생성된다. 물론, 알아서 셋팅도 다되기 때문에 손댈것이 별로 없다. 다만, 더미코드가 많이 생겨서 좀 지워야한다. *.model.js 와와 *.socket.js 파일은 필요없으니 삭제~! 

index.js 파일에도 내용 많지만 다 지우고 아래처럼 만들자.  

```
'use strict';

var express = require('express');
var controller = require('./message.controller');

var router = express.Router();

//router.get('/', controller.index);
//router.get('/:id', controller.show);
//router.post('/', controller.create);
//router.put('/:id', controller.update);
//router.patch('/:id', controller.update);
//router.delete('/:id', controller.destroy);

/* routes.js에 설정된 /api/message/ api로 post 기능 */
router.post('/', controller.sendmessage);

module.exports = router;
``` 

server/api/message/message.controller.js에도 많이 지우자. 별로 필요없고 아래 처럼 만들자. 

```
'use strict';

var nodemailer = require('nodemailer');
// create reusable transporter object using SMTP transport
var transporter = nodemailer.createTransport({
    service: 'Gmail',
    auth: {
        user: 'google ID',
        pass: 'userpass'
    },
    debug: true
});

transporter.on('log', console.log);

exports.sendmessage = function(req, res) {

    var data = req.body;

    transporter.sendMail({
        from: 'google ID', /* transporter setting과 동일.. 틀려도 동일하게 날라감*/
        to: 'destination email', /* 이곳으로 메일 전송 */
        subject: '[Q&A] Guest : ' + data.contactName,
        html: '<p> email: ' + data.contactEmail + '</p><p> phone: ' + data.contactPhone + '</p><br>' +data.contactMsg
    });

    res.json(data);
};

function handleError(res, err) {
  return res.send(500, err);
}

```

서버 쪽 셋팅은 이것으로 끝!

### client 구현

일단 컨트롤러에 $http를 추가한다.  

```
 .controller('MainCtrl', function ($scope, $interval, $http) {
```

다음으로 컨트롤러에 다음 함수를 추가  

```
    $scope.message = {};
    $scope.sendMail = function () {

            $http.post('/api/messages', $scope.message).
                success(function(data, status, headers, config) {

                  alert("질문 주셔서 감사합니다");
                  $scope.message = {};
```

마지막으로 html 은 다음과 같다.  

```
<form id="contact" class="contact-form" ng-submit="sendMail()" novalidate>
    <div class="message"></div>
     <div class="col-md-5 col-sm-5 col-xs-12 animated hiding" data-animation="slideInLeft">
      <div class="form-group">
        <input type="text" name="name" class="nameform form-control input-lg" placeholder="이름" ng-model="message.contactName" required>
      </div>
      <div class="form-group">
        <input type="email" name="email" class="emailform form-control input-lg" placeholder="이메일" ng-model="message.contactEmail" required>
      </div>
      <div class="form-group">
        <input type="text" name="phone" class="phoneform form-control input-lg" placeholder="전화번호" ng-model="message.contactPhone">
      </div>
    </div>
    <div class="col-md-7 col-sm-7 col-xs-12">
      <div class="form-group">
        <textarea name="message" class="messageform form-control input-lg" placeholder="무엇이든 물어보세요" ng-model="message.contactMsg" required></textarea>
      </div>
    </div>

    <div class="col-md-7 col-sm-7 col-xs-12">
        <input type="submit" class="btn btn-custom up form-button" value="Send Message">
</form>
```

### 마무리 

다른 이메일로 하느니 그냥 google ID로 하는 것이 편하다. 셋팅 해줄게 없거든~ 그리고 잊지 말아야 하는 것이 **https://accounts.google.com/displayunlockcaptcha**에 가서 unlock 해야 한다. 

good day~~! :) 