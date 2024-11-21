![[Pasted image 20220731222518.png]]
setTimeout(time, doSthing)
- operation is wait for some time 
- handler is doSthing 

- **main stack** | **application** sends event to **Event Demultiplexer**
- Then place them in a **[[queue]]** (there're not 1 but may queues with different priority)
- Load event from queues to Event Loop 
- event loop let **OS** (libuv) do the **operation**, when done, **OS** return event back to event loop
- **[[Event Loop]]**  poll main stack, when free give **handler** to it and executes  , main stack can runs the mentioned handler and sends more events to E.D simultaneously 


- another intuition: event loop: a loops that check if an event has happened, and push it back to call stack (executor or mainstack ) whenever it's free
![[Pasted image 20230531203627.png]]






# Event Demultiplexer
--- 
# Refererences 
- [Fetching Title#siy1](https://stackoverflow.com/questions/56622353/how-does-reactor-pattern-work-in-node-js)
[Event Loop explained in 3 minutes - YouTube](https://www.youtube.com/watch?v=5YcMKYTZZvk)


2022 07 31 22:31
#idea [[Event Loop]] [[async]] [[computer science]] [[reactor pattern]] [[Atomic Ideas/Nodejs]] 