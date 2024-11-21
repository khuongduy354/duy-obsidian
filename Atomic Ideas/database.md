# sql and nosql
# ACID
> Back then 1 server, this rule works fine, but with large distributed system, these things need relax for sake of performance and scale.
- Atomic: All or Nothing 
- Consistency:  almost everything is eventual consistency 
UPDATE 1 database, have to wait for other replicas to change -> data in different regions have different values
-  Isolation: a transaction read another transaction while updating  
Problems: 
	- Dirty read: read when the other not commited update
	- Non-Repeatable: read after commited update
	- Phantom Read:  same above, but insert a new row instead of updating
Solution: setting levels, performance decrease
![[Pasted image 20240226160418.png]]
### row level security 
Only allow authenticated user to modify that row 
- for e.g, if logged in user_id is in author_id col of that table, allow
### Exclusive lock and shared lock  
https://www.linkedin.com/advice/3/what-some-common-locking-scenarios-patterns-different 
Exclusive: only 1 allow read write at a time  
Shared: All allow read, no write

# Indexing and Trees 
[[Database indexing & B-tree]]

## What  data structure to store index?
### binary search tree 
- not balanced 
- BST has less comparisions but read more levels as it decesding the tree 
-> Ineffective for file read, cuz it read in batch (SIMD) 
### b-tree comees to rescue 
- more keys per level -> more effective for batch read
- balanced 
## How to store index?  
> Arbitrarily (with no data structure) -> fucking long queries. 
### clustered index/ primary key 
- Trees: root + mid nodes store primary key (a.k.a cluster-index) only, leaf (bottom) nodes store full value (keys + other fields) 
- When query: traverse through nodes (primary keys) to get to the data at bottom 
##### notes 
- this concept is called CLUSTERED. CLUSTER-INDEX == PRIMARY KEY (1 per table unique not null)
- it's reflects physical memory, which means create a cluster == sort original disk data -> changed
- faster than non-cluster, because non-cluster is based off this. 

#### non-clustered index / non-primary key/ normal index
> when u have a primary key already, but need to add more key
- Non-primary key index: have a separate virtual B-Tree, root + mid store non-primary keys, leaf store PRIMARY key. 
- When query: traverse through nodes (keys) to get to the primary key at bottom. then traverse the primary keys tree once more to arrive at real data
##### notes 
- it's a copy of disk data then sort on copy, so original disk data is unchanged
- entirely based off clustered index above.
-> can have many, but increase write time (insertion need update indexes)
-> slower than clustered because speed = non-cluster lookup + cluster lookup

## What format to used as index ?
- UUID hurt performance, still widely used
-> https://www.youtube.com/watch?v=NI9wYuVIYcA 
- auto increment is fastest, simplest but cant work in distributed, 
- Snowflake is better uuid in terms of performance, but has high collision rate compared to uuid/uild
- UILD has time component so in order and better index/performance, lower collision than snowflake. But overall more complex that uuid




# Replicas and Sharding 
### replication 
master-slave: master read write, slave  read only  
-> need logics to promote slave 
master-master 
-> write inconsistency problems 
###### overall disadvantages
cut off master before replicated data -> inconsistent data of nodes, load when replicating (lag) 
### federation 
split based on functions: post database, user database,...
-> less load when read/write because unaffected databases are not touched 
-> more logics on features 
### sharding 
split subset of data 
-> less load, more cache hit
-> hard to join, uneven distributed when most users access same shard

### denormalization 
normalization: keep data repeat as little as possible. For e.g User and Address in 2 tables, this allow reuse address, but 
**denormalization**: keep data repeat, to reduce reads (joins). For e.g address embedded in users. 
-> more write load, less read load 
-> mutable copies need to be in sync

### optimization / tuning 
https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sql-tuning


# NoSQL 
> key- value, documents, wide column 
> Lacks true ACID, eventual consistent 

# GraphQL 

# [[Caching 101]]




# database
--- 
# Refererences 




2024 02 26 16:02
#literature [[computer science]] [[sql]] [[nosql]] [[low level]] 