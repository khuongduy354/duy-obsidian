- store blobs 
- use system that points to blobs
- history management 

# Core 
```rust 
struct TreeDir{ 
	curr_abs_path // use for making blob from file
	hash: string //tree name generated from content
	blobs: BTree<FilePath,Blob> 
	child_trees: BTree<FilePath,TreeDir> 
}

struct Blob{ 
	curr_abs_path //later may use for searching easier 
	hash: string //file name generated from content  
	data: bytes //content 
}

struct Commit{ 
	tree_hash: string //commit points to a tree index
	messages: string
	parent_hash: string //prev commit
	
}

```







# git clone rust
--- 
# Refererences 

[Fetching Title#n9qx](https://github.com/khuongduy354/git.rs)



2023 04 17 20:11
#literature  [[Atomic Ideas/Rust|Rust]] [[git]] [[low level]] [[computer science]] 