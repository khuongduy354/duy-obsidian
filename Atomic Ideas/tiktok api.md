

# Endpoints

add authorization header to all *

  

POST /user/signup

POST /user/signin

GET /users/:email (just basic data, so no need to auth)

POST /users/:email/follow *

PUT /users/:email *

  

POST /video *

POST /videos/:id/like *

POST /videos/:id/comment *

GET /videos/:id (no private public yet)

DELETE /videos/:id *

GET /videos/feed * (has options)

  

# Features

- self implement Auth password based

- create videos

- like, comment videos

- follow users

- update users profile

- Feed generation (followed or all)



# Insights    
- Feed generation: for you and all  
- Pagination for infinite scroll  
- System design:... 
### notification  
- fanout  
### recommendation  
- based on likes ,comments  

### feed generation  + caching 
mode -> rank -> pagination 
- 2 modes:  all, followed     
- pagination: pass a random seed, has a limit and skip  option 
cache?  
- cache next feeds, when requested  (read based fanout, aka pull)
- cache hit = get that, request newer feed   batch, delete old ones.     
- cache miss = server generate feed response and cache NEXT batch (no need to wait) 
- who to cache? LRU   
- strat?  read through ?    

how about write-based feed gen?  (this is good for read-intensive though)
- we can update feed in a database  whenever a person post    
- based on LRU cache, we can cache these only 
- random? we can store feed in database in time order, but when fetch, we apply a random()  and track seed as above

pagination cache?   
- user scroll to end = get next batch of that  seed from cache 
- user force refresh = request a new random seed  

addressed problems 
- force request make previous cache go wasted -> acceptable, use low amount of caches  items
- next batch from users, how do you know if it runs out    
limit 20, skip 40  30  -> COUNT smaller === END  
-> user fetch next batch, if cache empty and END, do thing, 
if cache empty and still fetchable, it's a cache miss
- why not write based?
-> need to allocated feed storage for  nonactive users  

# Cache strategies 
tiktok   

read-heavy     
read through cant control users or hot data (dont use)
cache-aside -> use this   

write-based fanout -> user post -> write to feed of each followers -> cache only users in LRU queue 
write through  
write back -> use this since this maybe a write heavy task ( write to both feed and cache )
write around 
cache-aside -> stated above

read-based fanout -> user fetch -> cache followees 
cache aside for sure






# Learnt  
- feed generation, caching and pagination problem
- reset sql auto increment  how?  
- atomic sync between Video Table and data object in Cloudinary 
- asymmetric pair of unique (1,2) and (2,1) are diff, but cant have exactly 2 (1,2)  in same table  : for following rels
- symmetry: (1,2) (2,1) same, for friend rels 












# tiktok api
--- 
# Refererences 
https://www.geeksforgeeks.org/designing-tiktok-system-design/#5-lowlevel-designlld-for-tiktok-system-design



2024 01 29 11:42 
#literature [[backend]] [[system design]] [[Cheatsheet/Nodejs|Nodejs]] [[javascript]] 