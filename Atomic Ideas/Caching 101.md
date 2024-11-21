# Overview 
- cache has a TTL: time to live, keeps data fresh and not stale
- usually stored key value or close db: fast to retrive (if not, why not use primary database)
- where to cache: used frequent and cost resources.
# Places cache
client cache (partial control: local storage active cache, http headers) 
-> proxy cache (cdn for e.g)
-> server cache (total control) 
-> database cache (dependent on database provider) 
-> os cache (dont touch this)   
![[Pasted image 20240114143730.png]]
### http header can cache  
- if response has these header, browser understand to put that data in cache
- store in a browser region called cache 
- when access, browser can behave differently  
	- either make no request and use total cache in browser
	- make a request to check if data new/old, if no need to refresh, no response to receive   
	- or request and refresh 
https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
# Cases  
- Any below can implement cache database hot index  
- nginx is better for caching prebuilt assets, origin server, usually put cdn in front
- redis for more arbitrary dynamic data 
### Server Side rendering with 1 server  
- Static engine express   
- Next 
- Word press, Joomla 
#### Bottleneck 
- db queries before insert into html template -> db query cache
- frequently used templates -> use lib to cache template (only template)
- Media (video image) -> use CDN   
-> database has option to cache, just turn on 
-> template html cache with framework option 
-> some html cache with file cache
### Server side render multiple server  
- offload query result to Redis (3rd storage), cuz SQL caching may have locking (bad performance)
- also remove file cache (due to multiple server)  switch to Redis
- has a proxy (load balancer) -> cache full page here


### Client side SPA
- client server cache more, since need more request to server
-> cache header:  
	- client will not make request but use old response insteaad 
	- some sort of expiry mechanics 


# Cache strats  
- main diff of cache aside and read through
-> read through ENSURE SUPER CONSISTENCY
but cache death = no data lol, and maybe lots of unused data  
-> cache aside can control what to cache (LRU for example)   
- write through: write to cache, database immediately  
-> ENSURE CONSISTENCY, (but double write loads at same time)  
- write back: write cache -> write db
-> SAC consistency for performance, cost (reduce write load)
- write around: no update cache when write. (depend on read strat to update)
![[Pasted image 20240303162843.png]]



# Caching 101
--- 
# Refererences 
https://viblo.asia/p/caching-dai-phap-1-nac-thang-len-level-cua-developer-V3m5WdO8KO7#_level-1-monolith-webpage-with-single-server-3



2024 01 14 14:31
#literature [[system design]] [[backend]] 