- index to search data faster, like a dictionary has first letter as index.
- index is stored in a data structure, typically a tree 
- whole tree is stored in disk, when lookup, consume nodes into RAM:
	- when traverse to deeper level of tree, take the next node into RAM, to lookup 
	-> higher the tree, the more disk I/O -> slower speed 
	-> Need reducing tree height
# data structure options 
[[data structure]]
### binary search tree  
- good options, heigh O(logn)
 **but**, tree might be imbalance, leads to high tree
 ### b-tree 
 - most popular option, enforce a set of rules to ensure the tree is **fat** 
 - lookup (comparison), stills similar to **binary tree**, but since each node stores multiples value (usually size of disk page) 
 -> each read from disk loads more value
 -> less disk I/O 
 -> faster
**keynotes**: btree is similar to binary, but balance (makes tree fat), allowing disk to take in more values/read & reducing tree height, -> less disk I/O -> more speed
[Hiểu về Clustered index](https://viblo.asia/p/hieu-ve-clustered-index-bJzKmw9Bl9N)



# Database indexing & B-tree
--- 
# Refererences 
[Index trong database là gì? Tại sao cần Index Database?](https://viblo.asia/p/index-trong-database-la-gi-tai-sao-can-index-database-Az45bYDzlxY)
[B-Tree Indexes - YouTube](https://www.youtube.com/watch?v=NI9wYuVIYcA)
[SQL Index |¦| Indexes in SQL |¦| Database Index - YouTube](https://www.youtube.com/watch?v=fsG1XaZEa78)
[sql - Database indexes and their Big-O notation - Stack Overflow](https://stackoverflow.com/questions/4694574/database-indexes-and-their-big-o-notation)

2022 08 26 21:38
#literature  [[database]] [[computer science]] [[backend]]
