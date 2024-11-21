# Rules of thumb
- **Call down, signal up.**
- 

# Details
- _ready() is run top to bottom, children to parent
=> parent manages children, if children needs access to parent, put that code in parernt in stead. *
- init > enter_tree > ready > process
### accessing nodes
```python

get_node("HUD/ScoreLabel") # same as ./HUD/ScoreLabel
$HUD.ScoreLabel # shortcut


"../../Player" # or get_parent()
# avoid going up (parent), use signal instead




# absolute path
get_node("/root/Main/Player")

```
### communications
- signal  
- get_node
- groups


### filessystem
[Saving/loading data :: Godot Recipes](https://kidscancode.org/godot_recipes/basics/file_io/)