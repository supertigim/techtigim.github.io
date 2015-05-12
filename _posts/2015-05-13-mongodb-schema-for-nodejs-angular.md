---
layout: post
title: MongoDB Schema for Angular fullstack
tags: mongodb schema angular fullstack node.js
---

![enter image description here](https://lh3.googleusercontent.com/-iQlk17YWTvM/VVIRTj0f2FI/AAAAAAAAqWA/b9Bh4ssmZIQ/s0/fullslider1.jpg "fullslider1.jpg")
[이건 그냥 넣어 본거임 ㅎㅎㅎ]

몽고DB 처음, AngularJS 처음, node.js 초보 ㅋㅋㅋㅋ
개삽질 중이구나~~ ㅎㅎ
Modeling이 쉽지 않다는게 느껴짐 ㅎㅎㅎ

```
var CalendarSchema = new Schema({
  type: String, /*offtime, class*/
  teacher: String,
  student: String,
  status: String, /*booked, registered, studying, done*/
  start: String,
  dow: [ { type: Number } ],
  events: [ {
    start: String,
    end: String,
    dow: [ { type: Number } ], /*only for offtime*/
    rendering: String
  } ]
});
```


