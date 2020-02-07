--- 
layout: post  
title: owncloud DB/File 정보 업데이트 방법  
tags: technology  
class: post-template
subclass: 'post tag-technology'  
excerpt: 버전이 올라가면서 임의 추가한 파일이 보이지 않아 해결 방법 찾아보았다.     
author: tigim   
---  
  
버전이 올라가면서 임의 추가한 파일이 보이지 않아 해결 방법 찾아보았다. DB바꾸고, F5누르고 별 짓을 다해도 안되었는데, 공식 홈페이지에 방법이 있었다.  
  
[owncloud CLI](https://doc.owncloud.org/server/9.0/admin_manual/configuration_server/occ_command.html#file-operations)를 사용하면, Manual로 업데이트한 파일 동기화를 할 수 있다. 우분투에 설치했다면, "/var/www/owncloud"에가 가서 **occ**를 실행하면 된다.  
  
옵션은,  

	files
	files:cleanup              cleanup filecache
	files:scan                 rescan filesystem
	files:transfer-ownership   All files and folders are moved to another
								user - shares are moved as well. (Added in 9.0)
  
이고, 사용 방법은 아래와 같다.  

	sudo -u www-data php occ files:scan --help
	Usage:
	  files:scan [-p|--path="..."] [-q|--quiet] [-v|vv|vvv --verbose] [--all]
	  [user_id1] ... [user_idN]
	
	Arguments:
		user_id               will rescan all files of the given user(s)
	Options:
		--path                limit rescan to the user/path given
		--all                 will rescan all files of all known users
		--quiet               suppress any output
		--verbose             files and directories being processed are shown
                        additionally during scanning  
                        
500G정도 되는 데이터를 rescan하는데, 거의 3시간 걸린다. 마음의 준비를 하고 시작하길~ Cheers!!! :)  

