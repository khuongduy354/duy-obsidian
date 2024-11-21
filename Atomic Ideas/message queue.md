
# Kafka or RabbitMQ  
### Kafka  
- real-time data (streaming)  ( massive data )
- designed for horizontal scales 
-  not removed when consume (a log)  
- poll based

### RabbitMQ 
- traditional 
- long running tasks 
- have a GUI builtin for check messages 
- removed when consume (a queue)
- push based 

# Kafka 
> very technical https://kafka.apache.org/documentation/#design
- Bunch of distributed servers   
- fucking high throughput for massive data
- Pub/sub  or store msg  (durable) 
- events === a message (has key, value, timestamp)   
- events organized in topics ( like files in folders ) 
- 1 topics has many partitions (events same key stored in same partition) 
![[Pasted image 20240307224857.png]]
-> each topic can be replicated with setting (factor 3 = each topics has 3 copies ), across the Earth. 
-  multi consumers multi producer, 
- not deleted after consumed, but time-based deleted 
### connect adapter 
- has a group id which control consumer, if a group id is subscribed and polled all messages, relaunch it will not poll these old message again 





# message queue
--- 
# Refererences 




2024 03 06 21:54
#literature [[system design]] [[backend]] [[computer science]] [[low level]] [[devops]] 