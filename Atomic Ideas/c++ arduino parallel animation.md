
### parallel animation in Arduino   
```C++
animation1 = new Animation();
animation2 = new Animation();  

if((now - last_time )> interval){
animation1.update();
animation2.update();
} 
//allow running 2 animation without blocking.
```
-> create a class 
```c++
class Flasher{
lastMillis 
currentMillis 
interval 
n
}
```










# c++ arduino parallel animation
--- 
# Refererences 




2022 11 29 22:34
#ideas    [[Atomic Ideas/C++]] [[C++ Essence]] [[arduino]] 