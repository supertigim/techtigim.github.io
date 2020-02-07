--- 
layout: post 
title: C++로 웹서버 만들기 
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: Boost 공부하면서, 웹서버 하나 만들어볼까하고 정리해 보았다.
---  

Boost 공부하면서, 웹서버 하나 만들어볼까하고 정리해 보았다.   
  
## Web Framework in C++   

없겠지 했으나 너무 많아 어떤 것을 골라야 할지 감이 안온다.  

### cppcms  

개발자 커뮤니티에서도 자주 거론이 되는 것 보면 사용층이 많을 것으로 추정된다. -_-;; 몰라, C++로 웹서버 맹글면 변태 취급 할테니까... ㅋ

사이트에도 이런 말이 엄연히(?) 존재한다. 

> If you create small web applications that do not require high loads and require very short time-to-market periods -- probably tools like Django or RoR would be more appropriate for such tasks.

 - 공식 사이트 : http://cppcms.com/   
 - 공식 튜토리얼 : http://cppcms.com/wikipp/en/page/cppcms_1x_tut_hello  
 - 예제 : http://kukuruku.co/hub/cpp/building-a-website-with-c-cppcms-part-1   
  
### wt  
  
작명 하는 것 보면 오덕의 냄새가 너무 많이 난다. 노느나 파이손 웹 프레임웍들은 보면 그럴싸 하게 만드는데 말이지~  기능은 여러가지 지원된다. 특히, 로그인 모듈과 ORM이 있다는 것이 괜찮아 보인다.  

 - 공식 사이트 : http://www.webtoolkit.eu/wt  
 - GitHub : https://github.com/kdeforche/wt  
  
### proxygen  
  
facebook이 공개한 c++ http library다. ㅎㅎ 다른 놈들 다 뒤진거지 머 ㅋㅋㅋ 근데, 이거 웃긴게 SPDY라는 새로운 프로토콜을 지원하는거를 엄청 강조했다. 알겠지만, 구글이 만들고 얼마 전에 버린 그거... ㅎㅎ 어째든 소스 코드는 살펴 볼 필요가 있을 것 같다.  

 - GitHub : https://github.com/facebook/proxygen  
 - Google Group : https://groups.google.com/forum/#!forum/facebook-proxygen  
  
### crow  

오~! 이름이 덕후가 아니다. 알고 보니, 우리나라 개발자 만든거다. 것도 천재 C++ 개발자란다. 근데, 영어도 잘하시는지 다 영어로 되어 있어 몰랐다. ㅋ 개발자 본인도 취미 생활로 하는거라고 곧 쓰레기통으로 갈거라고 했다. ㅋㅋ 어째든 코드 공부하는데 한번 살펴볼 생각이다.  

 - GitHub : https://github.com/ipkn/crow  
 - shareslide에 설명  : http://www.slideshare.net/JaeseungHa/ndc2015-c11-crow  


----------

## boost.asio  

공부로 하는거라서 결국, asio로 돌아왔다. 관련 라이브러리 조각 모아서 mean stack 같은거 만들어야지.  

### libnghttp2_asio  

어떤 용감 무식한 분이 asio르 가지고 http2라이브러리를 만들고 계신다. 예제를 보면, 거의 node/express랑 사용하는 방법이 비슷할라고 한다. ㅋㅋㅋ 젠장 다 만들어져 있어~!!!

    using namespace nghttp2::asio_http2;
    using namespace nghttp2::asio_http2::server;
    
    int main(int argc, char *argv[]) {
    
      boost::system::error_code ec;
      http2 server;
      
      server.handle("/", [](const request &req, const response &res) {
        res.write_head(200);
        res.end("hello, world\n");
      });
      
      if (server.listen_and_serve(ec, "localhost", "3000")) {
        std::cerr << "error: " << ec.message() << std::endl;
      }
    }

 - 공식 사이트 : https://nghttp2.org
 - GitHub : https://github.com/tatsuhiro-t/nghttp2  
  
### boost.org  

boost.asio에도 간단히 http 서버 만드는 예제가 나와 있다. c++11과 c++03으로 나누어져 있고, 사용하는 데 맞추어 보면 되겠다.  

 - c++11 examples : http://www.boost.org/doc/libs/1_55_0/doc/html/boost_asio/examples/cpp11_examples.html  
 - c++03 examples : http://www.boost.org/doc/libs/1_55_0/doc/html/boost_asio/examples/cpp03_examples.html  

----------

## 웹 개발  

난 당연히 C++로 개발한 것이 어떤 다른 툴보다 빠를 것이라 믿었지만, 실제 벤치마크 한 결과로 보면, 어떤게 구현 했는지가 더 중요하는 것을 보여준다. 물론, 동일한 수준의 코드이면 무조건 c++가 제일 빠르다 ㅎㅎ 

https://www.techempower.com/benchmarks/ 에 보면 wt 프레임워크와 php-raw(php + nginx)의 성능이 거의 비슷하다. 허허~~! 결국, 이 짓이 나중에 내가 사업을 하던 취업을 하던 어떤 도움이 될지 공허감만 느끼는 블로깅이 되어 버렸다. ㅋ  

