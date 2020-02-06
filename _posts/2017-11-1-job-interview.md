--- 
layout: post  
title: 캐나다 IT 잡 인터뷰 관련    
tags: canada it job interview      
excerpt: 3개월 정도 캐나다에서 구직 활동하며 쌓은 경험 정리~            
---  

## 시작    

개발자 면접이라 다 덮어놓고 코딩 인터뷰만 해주면 고마울(?)것 같으나 막상 실전에 가보면 코딩 인터뷰없이 경력 사항이나 behavior question을 하는 곳도 굉장히 많음. 영어 네이티브가 아니기 때문에 확실한 준비가 필요! 솔직히 코딩 인터뷰는 string을 역순으로 정리 하는데 추가 메모리 사용 없이 해보라고 한게 다였음~ ㅋㅋㅋ  
  
[자주 묻는 Behavior Questions](https://devskiller.com/45-behavioral-questions-to-use-during-non-technical-interview-with-developers/) : 실제 여기 있는 거 몇개 질문 받아 봄. 되게 상세히 파면서 물어보기 때문에 예상 되는 추가 질문에 대한 답변도 고민해두는게 좋음. 생각한게 막상 온사이트에서 입으로 잘 나오지 않기 때문에 **소리내서 예상 답변 준비**하는 것이 키 뽀인트!!   

[엠베디드 시스템 및 C관련 인터뷰 질문](https://rmbconsulting.us/publications/a-c-test-the-0x10-best-questions-for-would-be-embedded-programmers/) : 코딩 인터뷰 외에 기본적인 지식을 묻는 질문도 함. 나의 경우에는 폰스크닝관 온사이트에서 모두 질문 받아봄.      
  
[c++ 질문들](http://www.geeksforgeeks.org/c-plus-plus/) : geeksforgeeks의 모든 내용을 안다면 FANG 할아버지가 와도 다 합격 가능할 만큼 중요하고 좋은 내용 많음. c++외에 다른 언어 관련해서도 찾을 수 있음. db도 있고... 내가 보기엔 책으로 굳이 안봐도 된다면 cracking the code보다 이 사이트를 딥러닝(!) 하는게 더 도움 될 듯~ :)     
  
## 코딩 인터뷰 문제 풀이 사이트 

[leetcode](https://leetcode.com/): FANG규모의 회사 지원할게 아니라면 easy 문제 위주로 풀면됨. 지금까지 인터뷰 해본 결과 medium이상 수준의 문제는 묻지 않음.   
[interviewbit](https://www.interviewbit.com/courses/programming/) : 롤플레잉 게임하는 것과 유사한 형태로 문제를 풀 수 있음. FANG지원 전에 풀면 좋을듯  
[hackerrank 알고리즘](https://www.hackerrank.com/domains/algorithms/warmup) : 요즘 주로 애용하는 사이트. c++, python을 둘다 풀어보는데 기본적으로 사용하는 자료 구조나 내장 함수가 저절로 외워짐    


## 코딩 인터뷰시 영어로 설명할 때 참고  

[cracking the coding interview in hackerrank](https://www.hackerrank.com/domains/tutorials/cracking-the-coding-interview) : 쉬운 문제이지만 기본적으로 알고 있어야 하는 내용을 저자가 설명해줌. 동영상 강의는 꼭 보고 입으로 따라 해보기. 자바 코드로 설명하는 것이 나에게는 별로~ ㅎㅎ  
  
[C++, Python, 자료구조 등을 영어로 설명해주는 비디오](https://www.youtube.com/channel/UCQoN6H05vCGN7EGkarCBHYA)  
  
## 기타 참고 사이트    

[미국에서 개발자로 일하는 허민석님 깃허브](https://github.com/minsuk-heo/problemsolving) : Python 코드로 간단히 자료 구조 살펴 볼때 엄청 좋음. 인터뷰 전날 한번 훓어보고 가는 사이트 


## 실제로 받았던 질문들  

캐나다 지원시 받은 질문들이니 지역 고려 필요함. 미국은 대부분 코딩인터뷰가 제일 중요하다고 하나 실제 캐나다에서는 코딩 인터뷰 없는 경우도 꽤 많았음  

- 32bit 시스템에서 메모리 할당은 어떻게 하나?  
  
- 1C가 10진수로 뭐냐?  
  
- 폰북을 만드는데 어떤 데이터 구조 쓸거냐?  
  
- 바이너리 서치 트리의 검색의 시간 복잡도는?  
  
- Virtual Function이란?  
	
	class A {  
		public:  
		void foo() {printf (”A”);}  
	};  
	class B : public A{  
		public:   
		virtual void foo() {printf (”B”);}  
	};   
	class C : public B {   
		public:   
		void foo() {printf (”C”);}   
	};   

- 위의 클래스를 보고 다음에 프린트 되는 값을 적으시오. <추가질문> 그 이유는?  
	A *a = new B; a->foo();  
	A *b = new C; b->foo();  
	B *c = new C; c->foo();  
	
- 특정 element를 자주 사용 또는 삭제 할 때 사용하면 좋은 것은? <추가질문> 그다음으로 쓸 수 있는 데이터 구조와 그렇게 생각하는 이유는?  
	가) linked list 나) stack 다) heap 라) queue 마) balanced binary tree  
  	
- 아래의 Singleton 클래스를 멀티스레딩 환경에서 사용할때 발생할 수 있는 문제? 그리고 막는 방법은? <추가 질문> 효율적!!으로 바꾸는 방법은?  

	class Singleton {   
		static Singleton* Instance(){    
			static *instance = null;  
			if(instance) {  
				instance = new Singleton;  
			}  
			return instance;  
		}  
	};  
	
- class 구현시 open api로 사용을 고려 할때 const 최적 활용 방법 
<추가질문> bool operator == (const A& a) const; 가 올바른 표현이다. 그렇다면 가장 뒤의 const가 붙은 이유는?  
- void reverse(char* str) 함수를 추가 메모리 사용 없이 구현 (화이트 보드에...)  
- hash table의 속도는? <추가 질문> 내부적으로 어떻게 동작 하는지 수도 코드로 구현  
- very_heavy_data class는 내부 데이터가 많아 constructor 동작이 굉장히 무겁다. 그렇다면 very_heavy_data를 멤버 변수로 갖는 아래 X class의 생성자를 효율적으로 하는 방법은?  

	class X {  
		very_heavy_data data_;  
		public:  
		X(very_heavy_data& d){  
			// 먼가를 하시오~!  
		}  
	};  
	
- 아래 두 클래스의 hierarchy 를 보고 무엇이 잘못되었는지 설명하고 가능하면 수정해라.

	class intArray {  
		public:  
		int get(int index);  
		void set(int index, int val);  
	};  
	
	class intStack: public intArray{  
		public:  
		void push(int v);  
		int pop();  
	};  
   
## 캐나다 연봉 정보    
 
주 별 소프트웨어 개발자 연봉 정보가 틀리므로 [이 사이트](https://www.jobbank.gc.ca/report-eng.do?area=9219&lang=eng&noc=2173&ln=n&s=1#report_tabs_container2)에서 
확인이 필요함. 협상을 이걸 기준으로 하면 될 것이나 영어 핸디캡 때문인지 기대 이하의 금액으로... ㅎㅎㅎㅎ ㅠ,.ㅠ  
  
  **온타리오 신입($50,000), 평균 ($89,000), 상급($130,000)**  
  
  

 