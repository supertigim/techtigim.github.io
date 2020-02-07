--- 
layout: post  
title: Swarm Pathfinding 알고리즘 정리 
tags: ai drone game pathfinding collision avoidance swarm gcs    
excerpt: 멀티 드론 관제하려고 AI 논문까지 찾아볼 줄이야...   
---  

드론 간 충돌 방지를 위한 AI(그냥 길찾기?) 관련 자료 정리   
  

## 사이트 정리         

Pathfinding 정의   
[https://en.wikipedia.org/wiki/Pathfinding](https://en.wikipedia.org/wiki/Pathfinding)  
  
Steering Behaviors For Autonomous Characters  
[http://www.red3d.com/cwr/papers/1999/gdc99steer.html](http://www.red3d.com/cwr/papers/1999/gdc99steer.html)
[http://opensteer.sourceforge.net/](http://opensteer.sourceforge.net/)  

Using reinforcement learning in Python to teach a virtual car to avoid obstacles  
[https://medium.com/@harvitronix/using-reinforcement-learning-in-python-to-teach-a-virtual-car-to-avoid-obstacles-6e782cc7d4c6#.aes0ycrmx](https://medium.com/@harvitronix/using-reinforcement-learning-in-python-to-teach-a-virtual-car-to-avoid-obstacles-6e782cc7d4c6#.aes0ycrmx)


auto-mission-prepare-for-mission-planner  
[https://github.com/unnamed-idea/auto-mission-prepare-for-mission-planner](https://github.com/unnamed-idea/auto-mission-prepare-for-mission-planner)  

AutoRCCar - 학습으로 길찾기  
[https://github.com/hamuchiwa/AutoRCCar](https://github.com/hamuchiwa/AutoRCCar)  

Python Library for google earth   
[https://www.youtube.com/watch?v=kWncnBBxoJ4](https://www.youtube.com/watch?v=kWncnBBxoJ4)
[https://github.com/simondlevy/GooMPy](https://github.com/simondlevy/GooMPy)  

Swarm Like collision avoidance, path finding and smooth following  
[http://roy-t.nl/2009/12/29/swarm-like-collision-avoidance-path-finding-and-smooth-following.html](http://roy-t.nl/2009/12/29/swarm-like-collision-avoidance-path-finding-and-smooth-following.html)  

Path Finding and Collision Avoidance in Crowd Simulation  
[https://dl.dropboxusercontent.com/u/18180490/OJS_file.pdf](https://dl.dropboxusercontent.com/u/18180490/OJS_file.pdf)  

Collision Avoidance and Swarm Robotic Group Formation 
[https://dl.dropboxusercontent.com/u/18180490/598-2413-1-PB.pdf](https://dl.dropboxusercontent.com/u/18180490/598-2413-1-PB.pdf)  
  
Collision Avoidance of Multiple UAS Using a
Collision Cone-Based Cost Function  
[http://www.eng.auburn.edu/files/acad_depts/csse/csse_technical_reports/csse12-07.pdf](http://www.eng.auburn.edu/files/acad_depts/csse/csse_technical_reports/csse12-07.pdf)  

유동적인 군집대형을 기반으로 하는 군집로봇의 경로 계획  
[http://ocean.kisti.re.kr/downfile/volume/kspe/JMGHBV/2012/v29n12/JMGHBV_2012_v29n12_1321.pdf](http://ocean.kisti.re.kr/downfile/volume/kspe/JMGHBV/2012/v29n12/JMGHBV_2012_v29n12_1321.pdf)  

Behavior-based Multi-Robot Collision Avoidance  
[https://www.semanticscholar.org/paper/Behavior-based-multi-robot-collision-avoidance-Sun-Kleiner/49e61705f282f48e1424c1611a153496327aa020/pdf](https://www.semanticscholar.org/paper/Behavior-based-multi-robot-collision-avoidance-Sun-Kleiner/49e61705f282f48e1424c1611a153496327aa020/pdf)  
  
Intelligent Moving of Groups in Real-Time Strategy Games  
[http://www.csse.uwa.edu.au/cig08/Proceedings/papers/8075.pdf](http://www.csse.uwa.edu.au/cig08/Proceedings/papers/8075.pdf)  
  
Agent Based Simulations, Game Technology  
[http://www.transportationtechnologyventures.com/simwiki/index.php?title=Agent_Based_Simulations,_Game_Technology](http://www.transportationtechnologyventures.com/simwiki/index.php?title=Agent_Based_Simulations,_Game_Technology)  

Parsec Patrol Diaries: How To Avoid Smashing Into Things  
[http://blog.lmorchard.com/2014/01/18/ppd-avoidance/](http://blog.lmorchard.com/2014/01/18/ppd-avoidance/)
[https://github.com/lmorchard/parsec-patrol/blob/2e3f9afa2404fee54b09152e3d5746c4c4a2b4ca/app/scripts/systems.coffee#L932](https://github.com/lmorchard/parsec-patrol/blob/2e3f9afa2404fee54b09152e3d5746c4c4a2b4ca/app/scripts/systems.coffee#L932)  
  
Robot Path Planning using An Ant Colony Optimization Approach: A Survey  
[http://thesai.org/Downloads/IJARAI/Volume2No3/Paper_10-Robot_Path_Planning_using_An_Ant_Colony_Optimization_Approach_A_Survey.pdf](http://thesai.org/Downloads/IJARAI/Volume2No3/Paper_10-Robot_Path_Planning_using_An_Ant_Colony_Optimization_Approach_A_Survey.pdf)  
  
SWARM - Lua Proramming  
[https://github.com/ninnghazad/SWARM](https://github.com/ninnghazad/SWARM)  
