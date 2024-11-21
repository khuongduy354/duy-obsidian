### real flow 
- user disconnecting -> delete from room, after disconnect -> delete from user list6


### flow  
- player move (if his turn, validate server-side) -> update in database (Redis or mongo) (wait) -> fanout websocket   
- place a msg queue above

### Reconnect  
- lose connection: easy, socket.io has heartbeat mech, just attemp syncing on reconnect 
**in case still connect, but lost message**
- acknowledgement: player moved on client, wait for ACK from server, other users in room get a broadcasted message, if sender not acknowledge, attemp sync to previous state in all sender, and  alert("Operation not working, please try again")  
- and when all reciepients received that broadcasted message, ack back to server (with some kind of timebased id) to know its last synced, and attempt to sync if needed  
-> in case more sure: recipient has total lost from server (but sender is still connected), which is a bit weird, but just assume. We can do polling (time based id ofc)  
**above is a bit strict assumption**
-> very rarely happens: if sender connect but recipient lost, usually after a few turn, recipient will receive back and know it need to sync   
-> if lost too many, it's a lost connection, not a connected but lost message case

### message queue 
[[message queue]]
- it will not stuck the server while processing a room database update  

# Learnt
Chessclub   
- consistent  
websocket since there're others as well 
cache room, based on websocket events:  write through, since consistent  
MongoDB noSQL, try sharding 
Message queue for sure  
try scale with 2 websockets server, load balancer 


Pub Sub doesnt ensure order, pub sub need polling on client, pub sub have high latency 

move validation server side 
deploy an A.I




# realtime chess club
--- 
# Refererences 




2024 02 07 22:04 
#literature  [[backend]] [[computer science]]  [[websocket]] [[system design]]