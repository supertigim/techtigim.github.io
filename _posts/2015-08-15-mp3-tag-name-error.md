--- 
layout: post 
title: 우분투에서 mp3 tag 한글 깨지는 현상 
tags: subsonic mp3 tag utf-8 
---  
리눅스에서 한글 깨지는 것 고치는 방법  


    sudo apt-get install python-mutagen
    
    mid3iconv -e utf-8 *.mp3  
  
잘 안되면,  
  
    mid3iconv -e cp949 *.mp3
    
    mid3iconv -e utf-8 *.mp3   
      
