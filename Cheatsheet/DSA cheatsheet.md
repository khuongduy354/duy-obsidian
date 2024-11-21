# bucketsort intro

# grouping anagram     
 1. sort each string
 ```python 
for each character: 
	sorted = countsort(character)
	if !map.has(sorted): 
		map[character]=[character] # hashmap of array 
	else: 
		map[sorted].push(character)  

```  
-> sort count takes  O(l+26)  = O(l) , drawback of it is space, but 26 chars isnt unbounded, l is avg length of string
-> O(n.l)  
2. hashmap 
```cpp
map<string, array> // array is char frequency
``` 
3. arrays`
```cpp 
array<array<int>> //array of array of char frequency
```
i forgot how 2,3 works





# DSA cheatsheet
--- 
# Refererences 




2023 09 25 21:58
#cheatsheet [[dsa]]