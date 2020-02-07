---  
layout: post 
title: ASIO 기반 C++ 프로그래밍    
tags: technology  
class: post-template
subclass: 'post tag-technology'  
author: tigim
excerpt: c++11/14 개발은 원시 시대 개발자의 무덤 
---  

변하지 않을 C++도 세월의 흐름에 맞추는 것인지 몰라도 업데이트가 화려하다. 몇가지 개발하면서 삽질한 부분에 대해 정리   
  
---
## enable_shared_from_this  
  
이넘때문에 아까운 시간을 허비했네... 결론적으로 사용하는 클래스가 enable_shared_from_this를 상속했다면, 
  
	//this <- 이걸로 함수 bind하면 새로 인스턴스가 생긴다. 
	this->enable_shared_from_this(); // 이게 정석   
  	
boost관련 문서를 많이 살펴보는 것이 필요하다.  
  
## lockless queue 만들기  
  
멀티 쓰레딩은 해야겠고, mutex는 쓰기 싫고 해서 고민하고 있었더니 이미 boost예제까지 존재~~ :) 반나절 돌려 보는데 괜찮음. 

	#include <boost/atomic.hpp>
	
	template<typename T, size_t Size>
	class ringbuffer {
	public:
	ringbuffer() : head_(0), tail_(0) {}
	
	bool push(const T & value)
	{
		size_t head = head_.load(boost::memory_order_relaxed);
		size_t next_head = next(head);
		if (next_head == tail_.load(boost::memory_order_acquire))
			return false;
		ring_[head] = value;
		head_.store(next_head, boost::memory_order_release);
		return true;
	}
	
	bool pop(T & value)
	{
		size_t tail = tail_.load(boost::memory_order_relaxed);
		if (tail == head_.load(boost::memory_order_acquire))
			return false;
		value = ring_[tail];
		tail_.store(next(tail), boost::memory_order_release);
		return true;
	}
	private:
	size_t next(size_t current)
	{
		return (current + 1) % Size;
	}
	T ring_[Size];
	boost::atomic<size_t> head_, tail_;
	};

[부스트예제사이트](http://www.boost.org/doc/libs/1_60_0/doc/html/atomic/usage_examples.html)로 확인하고, [응용 방법](https://nativecoding.wordpress.com/2015/06/17/multithreading-lockless-thread-safe-spsc-ring-buffer-queue/)도 참고해서 자기만의 것으로 만들면 되겠다.  
  
  
  
  