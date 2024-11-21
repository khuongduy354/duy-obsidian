# Overview
- With 1 WS server, that server takes in and broadcast, easily. 
- But with 10 WS server, A -> WS1, B -> WS2. A send to B. WS1 broadcast doesn't reach B -> Need sending from WS1 to WS2 
-> A middleware, which we use here is a Message Queue, or Pub/Sub layer 
https://medium.com/nerd-for-tech/scaling-websockets-horizontally-a-simple-guide-for-beginners-bf8b06c042f7
![[Pasted image 20240311155337.png]]
# Further problem: Publish all vs Routed
- Above diagram broadcast from Redis to **ALL** servers, which is fine. 
But when scale up, we need a node discovery, publish to all servers is costly
-> We need to route them selectively
https://journal.valeriansaliou.name/horizontal-scaling-of-socket-io-microservices-with-rabbitmq/
my idea: a key-value mapping service to track user id to websocket server
leverage of msg queue routing key

# Other benefits 
- Msg queue can be durable, and can retrieve lost msg  
- Async handling/offloading: for e.g WS doesn't have to wait for db store.  







# scaling websockets with message queue
--- 
# Refererences 




2024 03 11 15:41
#literature  [[message queue]] [[websocket]] [[backend]] [[system design]] 