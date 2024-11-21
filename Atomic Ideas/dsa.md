# Priority queue  
- store items, each item has priority  
- pop: take item with highest prior 
- peek: see item with highest prior 
- insert: insert item, adjust position 
### implements   
array, linkedlist: ordered list    
bst
heap: is a bst, but bst has order, heap not, just min/max value at root, also is balanced 

# LRU cache 
> remove least recently used to make space
- get(key): retrieve (change its priority since it's accessed/used) 
- insert(key,val): insert and remove LRU data 
### implements  
- dequeue + hashmap: dequeue track priorities, hashmap track availability   
- counter: frequency n-1 = most recent, 0 = lru  


# Forest 
Binary tree: basic, ordered, no balanced, asc or desc 
AVL Tree: it's a Binary tree with balancing feature (search balanced tree)
Red-Black Tree: similar to AVL, but sacrifice balance for faster insertion 
(insertion/deletion balanced tree) 

Heap: unordered, only min/max matter (at root), balanced   
BTree: 1 node has many items, ordered, balanced
![[Pasted image 20240229170805.png]]
 Trie(prefix tree): it's cute, basically it's a bst, but data are retrieved in sequential nodes 
![[Pasted image 20240229171828.png]]













# dsa
--- 
# Refererences 




2024 02 29 16:43
#literature  [[computer science]] 