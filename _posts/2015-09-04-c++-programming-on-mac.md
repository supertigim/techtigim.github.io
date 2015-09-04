--- 
layout: post 
title: C++ 프로그래밍 리턴즈  
tags: c++ c++11 boost mac makefile  
excerpt: 맥에서 C++프로그래밍 환경 셋팅을 해보자.  
---  

맥에서 C++프로그래밍 환경 셋팅을 해보자.  
  
인생이 돌고 도는 것처럼 프로그래밍 관심도 돌고 돌아 결국 다시 C++로 오게 되었다.  
  
----------
  
## IDE 선택  
  
몇 가지 후보가 있고, 현재 설치된 것도 있느데 아직 결정은 못했다. 후보는 Xcode, Visual Studio Code, Codelite정도이다. 어짜피 빌드는 커맨드 창에서 할거라서 가벼운게 좋긴한데, debugging을 제대로 하려면 무거워도 Xcode를 써야 하지 않을까 생각한다.  
  


----------
## Boost 라이브러리 설치  
  
c++11 or later와 이 라이브러리를 좀 살펴보고 싶어서 다시 C++프로그래밍 주섬 주섬 시작하게 된거다. 설치 방법은 간단하다.  
  

    brew install boost  
  
설치는 /usr/local/Cellar/boost 폴더에 있다. (버전 확인 가능)  
  
----------
## 테스트  

아래 두 파일 생성하고 make실행하면 된다. 

###main.cpp  

    #include <iostream>
    #include <boost/lambda/lambda.hpp>
    #include <boost/lambda/if.hpp>
    
    using namespace std;
    
    int main(int argc, const char * argv[]) {
    
    vector<int> v;
    
    v.push_back(1);
    v.push_back(3);
    v.push_back(2);
    
    //1보다 큰경우만 출력
    std::for_each(v.begin(), v.end(),
	    boost::lambda::if_then(boost::lambda::_1 > 1,
	    cout << boost::lambda::_1 << "\n"));
	return 0;
	}  

###makefile  
  

    EXEC = main
    SOURCES = $(wildcard *.cpp)
    HEADERS = $(wildcard *.h*)
    OBJECTS = $(SOURCES:.cpp=.o)
    
    all: $(EXEC)
    
    main: $(OBJECTS)
	    g++ -L/usr/local/Cellar/boost/1.57.0/lib -lboost_regex-mt -lboost_filesystem-mt -lboost_thread-mt $(OBJECTS) -o $(EXEC)
	    
	    %.o: %.cpp $(HEADERS)
	g++ -I/usr/local/Cellar/boost/1.57.0/include -c $< -o $@
	
	clean:
		rm -f $(EXEC) $(OBJECTS)  
  


----------
## 참고  

 - C++11/14 내용 요약: http://www.slideshare.net/jacking/modern-c-cpp11-14
 - C++ 웹서버 : https://github.com/eidheim/Simple-Web-Server
 - 알고리즘 공부? : http://sunnykwak.tistory.com/86
 - 디자인 패턴 : http://hongjinhyeon.tistory.com/24
 - Boost C++ 책 : http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788960776678
 - C++11 STL 책 : http://postgame.tistory.com/561

  

