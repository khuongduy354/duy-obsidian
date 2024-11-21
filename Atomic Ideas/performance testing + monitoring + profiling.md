# Terms  
- Performance testing includes: load testing, stress testing, endurance testing,...   Usually show results in term of throughput, response time  
- Profiling show the cpu, memory usage of a program  

-> Differences: performance testing may be fine (useable), even if we profile and see there's plenty of room for optimzation  
-> Vice versa, even if we optimize the heck out of it through profiling, it may not fulfill performance testing. 
-> Performance testing has a problem? -> Do profiling and fix the problem
# Tools 
oha.rs: load testing cli, very basic 
k6: load testing, fully configuraeble with .js 
https://youtu.be/ghuo8m7AXEM?si=aChho5cFHELA2pR7
# Types of testing 
https://www.blazemeter.com/blog/stress-testing-vs-soak-testing-vs-spike-testing#what-is-soak-testing-vs-stress-testing

# Prometheus monitoring
https://youtu.be/STVMGrYIlfg?si=gTUXYbSGOkOKqslF

# Profling 
### nodejs profiling
```js
//terminal 1
node --prof index.js

//terminal 2
oha http://localhost:8000 // run load testing  

// terminal 1  
should create a .log 
node --prof-process name.log > profile.txt 
// read profile.txt for profiling  


// use chrome  
node --inspect index.js 

// open chrome  
chrome://inspect // in url 
select index.js 

```
 





# performance testing
--- 
# Refererences 




2024 01 14 14:13
#literature [[testing]] [[devops]] [[monitoring]] [[optimization]]