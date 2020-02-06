--- 
layout: post  
title: 파이썬 입문      
tags: python      
excerpt: 기존 프로그래밍 언어 적응자에겐 그저 신묘한 파이썬 팁!                     
---  

## Dictionary에서 * 사용  
  
c/c++ 개발자에겐 포인터로 익숙한 *을 이리 이용할 줄이야...   
  
	test_dic ={'param': 0.09}  
	
	class Test:  
	
	    def __init__(self, param=0.01):  
	        print param  
	        self.param = param  
		        
	test = Test(test_dic)  
	{'param': 0.09}
	
	test = Test(*test_dic)  
	param
	
	test = Test(**test_dic)  
	0.09  
  
## numpy.random.permutation    
   
	import numpy as np
	
	permutation = np.random.permutation(10)  
	
	print permutation  
	[4 3 7 2 5 9 6 1 0 8] // 0부터 10이전까지 숫자 shuffle   
	

## numpy.max에서 axis 인자    

	data = np.random.rand(2,3)  
	  
	print data  
	[[ 0.3076822   0.68571054  0.67192556]  
	 [ 0.57636354  0.07777546  0.8745576 ]]  
	 
	print np.max(data, axis=1)
	[ 0.68571054  0.8745576 ]  
	
	print np.max(data, axis=0)  
	[ 0.57636354  0.68571054  0.8745576 ]  
	
	print np.max(data)  
	0.874557598604  
	
  