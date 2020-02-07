---
layout: post
title: jekyll-now기반 블로그에서 테이블 넣기
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
---

markdown 언어가 그렇게 친숙하지 않기에 만나 문제 중의 하나

##**문제 이유**
github + jekyll 기반으로 블로깅을 고려하면, 제일 쉽게 하는 방법이 **[jekyll-now](https://github.com/barryclark/jekyll-now)**를 사용하는 것이다. ([참고사이트](http://ilmol.com/2015/01/Jekyll,Git%20%EC%9D%84%20%EB%AA%B0%EB%9D%BC%EB%8F%84%20%EB%AC%B4%EB%A3%8C%20Github%20Pages%20%EC%A6%90%EA%B8%B0%EA%B8%B0.html))

물론, 블로깅 경험이 많지 않으면 글처럼 쉽지 않다는게 큰! 함정.

어째든 나는 이 방법을 통해서 현재 블로그를 개설했고 만족하면서 쓰고 있었는데, 어제 하나 문제점을 발견했다.

바로 **markdown 문법으로 테이블이 만들어지지 않는 것이다 ㅠ.ㅠ**.  

마크다운이 간단해서 시작한건데 안되면 어떻하라고~

<br />
##**해결 방안**

찾은 방법은 이것 저것 수정하면서 알게 된거라, 관련이 없는 것일 수 도 있지만, 일단 적어둔다.

###1. _config.yml 수정
```
markdown: redcarpet
redcarpet:
 extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
```
아는 사람 알겠지만, 여기 스페이스 하나라도 잘못 넣으면 오류 나오니 주의!

###2. style.scss에 table 관련 css 추가

```
/*********************/
/* TABLES */
/*********************/
table {
  //margin-left: 20px;
  padding: 0; }
  table tr {
    border-top: 1px solid #cccccc;
    background-color: white;
    margin: 0;
    padding: 0; }
    table tr:nth-child(2n) {
      background-color: #f8f8f8; }
    table tr th {
      font-weight: bold;
      border: 1px solid #cccccc;
      text-align: left;
      margin: 0;
      padding: 6px 13px; }
    table tr td {
      border: 1px solid #cccccc;
      text-align: left;
      margin: 0;
      padding: 6px 13px; }
    table tr th :first-child, table tr td :first-child {
      margin-top: 0; }
    table tr th :last-child, table tr td :last-child {
      margin-bottom: 0; }

```
(*) [발췌 사이트](https://github.com/sindresorhus/github-markdown-css/blob/gh-pages/github-markdown.css)

<br />
##**그 외 기타**

마크다운이 간단하기는 하지만 여간 까다로운게 아니다. 스페이스 하나에 동작 안되고 난리다.

### 테이블 위치 조정
테이블이 생뚱 맞은 위치에 있는경우가 있어 옮겨보려고 온갖 짓을 다해봤지만 실패~~~ -_-; 결국 날 HTML 코들 넣는걸로 해결

```
<style>
    table {
      margin-left: 30px;
    }
</style>
```

### 테이블 다음에 line feed (return)

스페이스 두번 넣어두면 다음에 줄 하나 들어가는 것은 아실테지만, 테이블 다음에는 아무리 넣어도 동작하지 않는다. 결국 이것도 날 코딩~ -_-;;

```
<br />
```
