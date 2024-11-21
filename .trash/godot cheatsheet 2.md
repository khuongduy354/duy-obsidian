[[godot essence]] for more code/design/implementations 
[[game juice]]
# Cheatsheet
```python
# SIGNAL
func ready(): 
	self.connect("signal_name",self,"callback")

# GROUPING
add_to_group("name")
get_tree().call_group("name","func") 

# INSTANCE
var pipes = preload("res://Pipes.tscn")
func main: 
        pipe = pipes.instance()


# NODE ACCESS 
get_node("HUD/ScoreLabel") # same as ./HUD/ScoreLabel
$HUD.ScoreLabel # shortcut
"../../Player" # or get_parent()
get_node("/root/Main/Player") # absolute path

yield(get_tree().create_timer(2),"timeout") 
ield( get_node("AnimationPlayer"), "finished" )
# add child / remove child
add_child(node) 
remove_child(node) 
ONE NODE CANT BE CHILD OF 2 PARENTS, so remove first 

# variables
export(PackedScene) var lever_instance 
```

# Theory      
## event 
- \_input  :main
- \_gui_input  : if use Control node 
- \_unhandled_input: rest 
[Using InputEvent — Godot Engine (stable) documentation in English](https://docs.godotengine.org/en/stable/tutorials/inputs/inputevent.html)
## set_deffered 
- set a signal at next physic frame  
## user path
- see in Projects > User data folder 
- not user res because res is readonly, user data can read-write 
## Autoload
Projects -> Autoload -> use as global variable
## Init functions
 _ready() is run top to bottom, children to parent
    =\> parent manages children, if children needs access to parent, put that code in parernt in stead. *
- init > enter_tree > ready > process

## variables 
```
(export var) -> (onready var) -> ( _ready() ) -> var/const
```
setget: call custom function when the variable got accessed.
use for setting a value in min,max range1
for bigger pieces of code, check [github](https://github.com/khuongduy354/godot-cheatsheet.git)
## Screen size
## References & Objects  

extends References or Objects 
References: less functions, auto memory management 
Objects: reverse (variable = null after used or memory leaks)

- screen settings in Project -> Projects Setting -> Window
    is base size (not real resolution)
- Stretch Mode
    - Viewport: size exactly resolution
    - 2D: size cover screen (according to Stretch Aspect)
- Stretch Aspect:
    - determine aspect after stretching

## Resources
```python 
//RESOUCE 
1. Make a script.gd 
extends Resource 
export(int) var GRAVITY = 2 
2. Right click in empty space, make a new Resource -> attach script
3. In scene need resource: 
export(Resource) var ResourceName
#or
load("path to .tres")
Put that .tres just created into the scene
```
- a class (like Nodes), contains thing not in GUI
- examples: Texture,scenes (tscn), Script are Resources instance
- [custom Resource](https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html#creating-your-own-resources)
## Positions 
- global_position
- position : relative to its parent 
- set_as_top_level: make child independent of parent 
example: spawn bullet, set it to global, or else it depends on the gun 
## VFX 
see [[godot essence#VFX singleton]]
```python 
1. AudioStreamPlayer2D node 
2. drag.wav to that 
3. $AudioStreamPlayer2D.play()
```
# Niche cheatsheet  
## Json file & other filesystem 
```python


func load_game(path):
	var file = File.new()
	if not file.file_exists(path):
		print("cant open file")
		return
	file.open(path, file.READ)
	
	var data = parse_json(file.get_as_text())

	file.close()

	return data


func save_game(path,data:Dictionary):
	var file = File.new()
	file.open(path, File.WRITE)
	file.store_line(to_json(data))
	file.close()


{
  "Sword": {
    "attack": 12,
    "stackSize": 1
  }, 
  "Pants":{
    stackSize:32
  }
  "Shirt":{
    stackSize:32
  }
}

```
## RemoteTransform2D 
```python 
# use node from far distance by absolute path 

# in child
func connect_cam(path)
	$RemoteTransform2D.path = path()
	
# in parent 
func _ready():
	child_instance.connect_cam($Camera2D.get_path())

```
## Viewport & Canvas layer 
## collisons
https://docs.godotengine.org/en/latest/tutorials/physics/index.html
https://godotengine.org/qa/4010/whats-difference-between-collision-layer-collision-masks
### Layer and Mask 
- Layer: can collide others 
- Mask: can be collided by.12
## Pathing
- Path2D can draw path
- PathFollow2D inside above, unit_offset -> move child along path 

- animation player can also move child, without script 
## Animation
- AnimationSprite: single animation frame, REGION
- Animation on Sprite directly (for convenient)

- AnimationPlayer: spritesheet (use with Animation Tree)
- AnimationTree: animation State machine

- we can save animations as .tres (AnimationSprite & Player nodes), then change different frames (skin for example)  
see [[godot essence]] for animation tree
## Parallax
- 1 parralaxbackground, with parrallaxlayer each (set mirroring to size of background).
- use camera 2D to track object movement. (or need to config offset with code)
  - scale = speed of that layer










## Raycast 
```python
.is_colliding():  
.get_collider():
.force_update_raycast(): #update right now instead of next physics processp
```
## Area2D  
```python 
	var bodies = get_overlapping_bodies()
```


# godot cheatsheet
--- 
# Refererences 




2022 12 26 23:01
#cheatsheet  [[godot]] [[godot essence]]