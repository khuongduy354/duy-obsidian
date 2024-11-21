# Syncing techniques  
**notes** mutex basic blocking related to pessimistic block [[multithreading]]
1. Lock: turn based user -> not practical, 1000 users for example  ([[pessimistic lock]])
2. Sync all: 
-> per character typed -> emit event ->  sync all contents
3. Diff syncing: (see resources)
-> like git, compare and sync   
4. [[Operational Transformation|OT]] syncing  ([[optimistic lock]]) (see resources)
-> key lies in syncing each operations  
for each clients, received a transformed operation  
such that all of them result in the same state 
-> apply in both client and server 
![[Pasted image 20231228161354.png]]
- s is actually received from another client
```python
ET(a,b): exclude effects of op b from a 
IT(a,b): include effects of op b in a 
Tii(remote, local)->remote'  
# ii = insert insert, whichs sync 2 inserts  
# for 2 ops: insert delete, there 4 trans functions  

 ``` 
```python  
clarify et it   
impacts mean in term of syncing not position 
insert insert: do not conflict but may skip so IT 
insert delete: conflict might negate each other so ET
**PHRASE THE WORD**: NEGATIIOOOOONNNNN!  
actually it doesnt matter, transform is transform

``` 

### algo control 
1.  server, client keeps track of synced and pending status, and send ops strategically, ot client side 
2. in fe, remote has a higher priority > local when sync, since it has been sent to other clients, also by doing this, it dont need a new sync request
```c++
client A -> send server, server update server docs  
-> broadcast to all clients 

for clients receive broadcast: update sync id (incremental or able to keep track of old-new) and apply OT: 
1. pending changes against broadcast (for sending to server)
2. local against broadcast (for applying to current editor), use revision id to know how far to apply for 
-> new pending change and a new local

for server receive request: check revision id, and apply OT based on that. broadcast  

// 1. emitting a new state after sync necess?   -> it's a side effect, dont worry
//2. how to know how far to apply to client 
// -> revision id 
//3. multiple locals: 
// -> may transform all locals on remote (starting from a pos)
// or remote on all locals   

// since client is all about document, dont need to get every ops correct as long as the one send to server is correct   
// let priorize remote first, since it synced from another system
// if we go for local first, another different sync is needed 
// after the transform! 



``` 
3. etherpad 
#### Batched sync   
1. pendingChanges that keep adding ops to it, only set if acknowledge is true, after send, empty pendingChanges  
2. 2 things: should pendingChanges and buffer be separated? 
-> if pendingChanges blocked, buffer stills change, or i write change directly to pendingChanges 
#### server OT apply
- when receive op with X revision id, compare that, and apply that against any ops it doesnt know!
- for e.g: receive id = 2, but current id = 7. need to apply for 2,3,4,5,6,7, then that op is saved with id = 8.
-> Why? revision id = 2, means it's accounted for id = 1 (see client apply pendingChanges above), but it doesn't know the rest, hence apply.   
```python   
# receive c_op (client operation) from client 
# receive client 
if c_op.id > curr_max_id       
	save(op)
	broadcast(op)  
else 
	for op in server_ops where op.id >= c_op.id:   
		c_op = OT(c_op,op)
	c_op.id = curr_max_id + 1
	save(op)
	broadcast(op)
```
#### client OT apply
- prioritise remote over local, cuz remote is sync (on server) and broadcasted for other devices already  
```python   
# receive s_op (server operation) from server    

pendingChanges.map(op => OT(op,s_op))     

same apply to (unsynced) local ops,   

# how to keep track unsynced local ops?    
# opsList.length - pendingChanges.length, get the first idx of unsynced op 

```    

```python  
# server 1 op change  
for op in server.ops where op.id >= c_op.id: 
	applyOT(c_op,op)
```

# Decision  
- FE: ops based rendering or docs based? only pendingChanges is enough, or even hybrid: editor = text + pendingChanges? 
-> tough to undo


# MVP  
user open a file, with an id  
share that id to the other user 
2 edit at same type 

a naive approach to syncing: for each event (typed into text area) -> take content of that client and give to server -> broadcast back to both client.    

enhance: sync based on line: emit line change, copy content of that line, and server broadcast change to that line  

# Frontend 
https://github.com/deeproop/google-doc-clone 

????
enhance:   
server keep track of a previous sync, for every changes on client, apply on the SERVER sync, not client sync.
 
prev = abc  
abcd: add d -> broadcast, client verified same sync -> update prev
aef: remove bc, add ef   from b 
-> aefb


















# google docs clone
--- 
# Refererences 

https://www.youtube.com/watch?v=2auwirNBvGg
https://hackernoon.com/operational-transformation-the-real-time-collaborative-editing-algorithm-bf8756683f66   




2023 12 24 20:18
#literature [[backend]] [[system design]] 