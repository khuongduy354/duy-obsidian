# full  
- cmake in build folder instead
![[Pasted image 20230224215140.png]]
```cmake 
cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

project(Tutorial VERSION 3.0)
add_executable(Tutorial runsth.cpp)

add_library(sayhello source1.cpp source2.cpp) #add a lib called sayhello 

target_link_libraries(Tutorial PRIVATE say-hello) #link exec to lib

```

# example 
![[Pasted image 20230224220121.png]]
```cmake  
# target =lib or exec 


TOP LEVEL CMAKE
cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

project(Tutorial VERSION 3.0)

add_subdirectory(say-hello)  #dir 
add_subdirectory(hello-exe) 

say-hello CMAKE   
add_library(say-hello src/hello.cpp src/hello.hpp) #target 
target_include_directories(say-hello PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src") #target 

hello-exe CMAKE 
add_executable(Tutorial main.cpp) 
target_link_libraries(Tutorial PRIVATE say-hello)   

#usage 
#exec: 
#include <say-hello/say-hello.cpp>  //dir/file



```

# cmake
--- 
# Refererences 

[Site Unreachable](https://codeiter.com/en/posts/subdirectories-spliting-code-in-cmake)



2023 02 23 21:55
#cheatsheet   [[c++]]  [[C++ Essence]] 