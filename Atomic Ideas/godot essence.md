# Implementations             

### screen shake 
```python
extends Camera2D


@onready var shake_timer = $ShakeTimer


@export var shake_amount: float = 5.0

# Called when the node enters the scene tree for the first time.
func _ready():
	set_process(false)
	SignalManager.on_player_hit.connect(on_player_hit)
	SignalManager.on_game_over.connect(on_game_over)


# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(_delta):
	offset = get_random_offset()
	

func get_random_offset() -> Vector2:
	return Vector2(
		randf_range(-shake_amount, shake_amount),
		randf_range(-shake_amount, shake_amount)
	)
	
	
func shake() -> void:
	set_process(true)
	shake_timer.start()


func on_player_hit(_lives: int) -> void:
	shake()
	

func reset_camera() -> void:
	set_process(false)
	offset = Vector2.ZERO


func on_game_over() -> void:
	reset_camera()


func _on_shake_timer_timeout():
	reset_camera()

```
### trajectory 
[2D Animated Trajectory Line in Godot (with collisions!) - YouTube](https://www.youtube.com/watch?v=Mry6FdWnN7I)
ricochey2
[Dungeolf Demo Vid - YouTube](https://www.youtube.com/watch?v=zFhvChcy3hw&t=104s)  
- even the tutorial is buggy at the corner
#### also used for spotting empty spots (for spawner)
### Soft collison
- when multiple enemies of same layer overlap, it looks like 1 enemy 
[Make an Action RPG in Godot 3.2 (P19 | Enemy Soft Collisions + Profiling) - YouTube](https://www.youtube.com/watch?v=ffXx0dPejWY&list=PL9FzW-m48fn2SlrW0KoLT4n5egNdX-W9a&index=21)
### Animation tree & other animation 
- see [godotminigames/heartbeastplatformer/Entity at master · khuongduy354/godotminigames · GitHub](https://github.com/khuongduy354/godotminigames/tree/master/heartbeastplatformer/Entity), which contains Anim FSM + FSM mechanics code.
- use use NodeStateMachine as root 
- use Blend2D space, input vector -> animation 
- animation (like idle) should loop 
- remember to click auto-start (the => icon in animationtree panel)
```python
animation_tree = $AnimationTree
animation_state = animation_tree.get("parameter/playback")
var dir = Vector2().normalized()

# set both input 
animation_tree.set("parameters/Idle/blend_position",dir) 
animation_tree.set("parameters/Walk/blend_position",dir)
#but only use one animation at a time 
animation_state.travel("Walk")
```
##### other
```python
yield($AnimationPlayer,"animation_finished")
```
### Steering / Chasing 
```python 
1. ONE VECTOR 
Enemy: 
	position = position.move_toward(target_pos,speed) 
	#or 
	position = global_position.direction_to(target_pos)*speed

2. TWO VECTOR (Homing missle)
Enemy: 
	#get 2 vectors 
	desired_vec=global_position.direction_to(target_pos)*speed
	current_vec = #available when first launched

	#converge
	change =(desired_vec-current_vec)*drag
	current_vec += change

	#move
	position += current_vec 
  
3. Very complicated AI (see youtub)
```


## advanced Keyboard input
- look_at(get_global_mouse_position()) // rotate according to mouse 
- movement see above
- for custom input:
```python
unc _unhandled_input(event: InputEvent) -> void:
    # mouse 
    if event is InputEventMouseMotion:	
    # press 
    if event.is_action_pressed("click"):		
    if event.is_pressed(): #any key pressed 
    if event.is_echo(): #last key hold 
```
## advanced jumping techniques
```python 
func move(dir): 
	veloc.y+=GRAVITY
	# coyote allows jump after falling a bit
	if was_on_floor and !is_on_floor(): 
		coyoteTimer.start()
	# double jump reset 
	if is_on_floor():
		double_jump=1

# pressed instead of just_pressed for buffer jump
	if Input.is_action_pressed("space"):
		if is_on_floor() or !coyoteTimer.is_stopped():
			veloc.y=-jump_height
			bounce_jump=false
		else: 
			# just_pressed here, to prevent above to consume
			if Input.is_action_just_pressed("space") and double_jump > 0: 
				double_jump-=1
				veloc.y=-jump_height
			bounce_jump=true
			$BounceJump.start()
	move_and_slide(veloc,Vector2.UP)
```

## VFX singleton 
see [[godot cheatsheet#VFX]]
```python
extends Node
class_name GameAudio

const PLAYER_JUMP = preload("res://mixkit-retro-game-notification-212.wav")

# allow multiple vfxs
func play_audio(type):
	for audio in self.get_children():
		if not audio.playing:
			audio.stream = type
			audio.play()
			break
	

# consume 
GameAudio.play_sound(GameAudio.PLAYER_JUMP)
```

# AI Artificial intelligence  + Algo 
### pathfinding  
- A* A asterisk
[A* Pathfinding (E01: algorithm explanation) - YouTube](https://www.youtube.com/watch?v=-L-WgKMFuhE)
- for grid: [line pathfinding](http://www.roguebasin.com/index.php?title=Bresenham%27s_Line_Algorithm)
### patrol / wander  / chasing    
- see [[GitHub - khuongduy354/godot-ricochet](https://github.com/khuongduy354/godot-ricochet)]
**advanced chasing**
[make chasing fun ](https://www.youtube.com/watch?v=l1LrJ8saxc0) 
[Advance Enemy AI in Godot - YouTube](https://www.youtube.com/watch?v=-y3MOsBetow) 
**if else patrol**
[Creating our Enemy AI - Creating a Horror Game in Godot 4 Part 2 - YouTube](https://www.youtube.com/watch?v=l1LrJ8saxc0)

### multiphase
detect player-> prepare -> attack -> player still there? -> repeat attack 
									-> retreat (roam or idle)

# SERVER 
Basic connection
```python
extends Node

const DEFAULT_PORT = 28960
const MAX_CLIENTS = 6

var server = null
var client = null


func _ready() -> void:
	if OS.get_name() == "Windows":
		ip_address = IP.get_local_addresses()[3]
	elif OS.get_name() == "Android":
		ip_address = IP.get_local_addresses()[0]
	else:
		ip_address = IP.get_local_addresses()[3]

	#above might not work, so 
	for ip in IP.get_local_addresses():
		if ip.begins_with("192.168.") and not ip.ends_with(".1"):
			ip_address = ip
	
	get_tree().connect("connected_to_server", self, "_connected_to_server")
	get_tree().connect("server_disconnected", self, "_server_disconnected")
	get_tree().connect("connection_failed", self, "_connection_failed")

# main functions 
func create_server() -> void:
	server = NetworkedMultiplayerENet.new()
	server.create_server(DEFAULT_PORT, MAX_CLIENTS)
	get_tree().set_network_peer(server)
	#broadcast (see below)
	
func join_server() -> void:
	client = NetworkedMultiplayerENet.new()
	client.create_client(ip_address, DEFAULT_PORT)
	get_tree().set_network_peer(client)

# signals
func _connected_to_server() -> void:
	pass

func _server_disconnected() -> void:
	pass

# helpers 

func reset_network_connection() -> void:
	if get_tree().has_network_peer():
		get_tree().network_peer = null
```
Player 
```python 
func _process(): 
	if is_network_master(): # local system master of this node
		move_and_slide()

# update player params every tick (0.03s for e.g)
func _on_Network_tick_rate_timeout():
	if get_tree().has_network_peer():
		if is_network_master():
			rset_unreliable("puppet_position", global_position)
			rset_unreliable("puppet_velocity", velocity)
			rset_unreliable("puppet_rotation", rotation)
```

### listener and broadcast
```python 
# LISTENER
known_servers = {} # listener keep tracks of all servers. 
func _process(delta):
	if socket_udp.get_available_packet_count() > 0:
		var server_ip = socket_udp.get_packet_ip()
		var server_port = socket_udp.get_packet_port()
		var array_bytes = socket_udp.get_packet()
		
		if server_ip != '' and server_port > 0:
			# newly created server, take request
			if not known_servers.has(server_ip):
				var serverMessage = array_bytes.get_string_from_ascii()
				var gameInfo = parse_json(serverMessage)
				gameInfo.ip = server_ip
				gameInfo.lastSeen = OS.get_unix_time()
				known_servers[server_ip] = gameInfo
				emit_signal("new_server", gameInfo)
				print(socket_udp.get_packet_ip())

			# known server
			else:
		      known_servers[server_ip].lastSeen=OS.get_unix_time() 

# connect timer node to this  
# if after wait_time, no request from known server, remove that server
func clean_up():
	var now = OS.get_unix_time()
	for server_ip in known_servers:
		var serverInfo = known_servers[server_ip]
		if (now - serverInfo.lastSeen) > wait_time:
			known_servers.erase(server_ip)
			print('Remove old server: %s' % server_ip)
			emit_signal("remove_server", server_ip)
```
broadcast
```python
func _enter_tree():
	broadcast_timer.wait_time = broadcast_interval
	broadcast_timer.one_shot = false
	broadcast_timer.autostart = true
	
	if get_tree().is_network_server():
		add_child(broadcast_timer)
		broadcast_timer.connect("timeout", self, "broadcast")
		
		socket_udp = PacketPeerUDP.new()
		socket_udp.set_broadcast_enabled(true) 
		# 255.... means if hit a router, forward it to port
		socket_udp.set_dest_address('255.255.255.255', broadcast_port)

# broadcast every wait_time 
# server broadcast to every devices connected to its port
func broadcast():
	server_info.name = Network.current_player_username
	var packet_message = to_json(server_info)
	var packet = packet_message.to_ascii()
	socket_udp.put_packet(packet)
```
### usage listener, broadcast
```python 
# listener signals
func _on_Server_listener_new_server(serverInfo):
	var server_node = Global.instance_node(load("res://Server_display.tscn"), server_container)
	server_node.text = "%s - %s" % [serverInfo.ip, serverInfo.name]
	server_node.ip_address = str(serverInfo.ip)

func _on_Server_listener_remove_server(serverIp):
	for serverNode in server_container.get_children():
		if serverNode.is_in_group("Server_display"):
			if serverNode.ip_address == serverIp:
				serverNode.queue_free()
				break

# connect JOIN button
func _on_Join_server_pressed():
	Network.join_server() 

func _on_Create_server_pressed():
	if username_text_edit.text != "":
		Network.current_player_username = username_text_edit.text
		multiplayer_config_ui.hide()
		Network.create_server()
	
		instance_player(get_tree().get_network_unique_id())
```



see this for more details [GitHub - HelamanWarrior/Godot-Multiplayer-Shooter](https://github.com/HelamanWarrior/Godot-Multiplayer-Shooter)
# godot essence
--- 
# Refererences 




2022 12 11 15:29
#literature    [[godot]] [[gamedevlog]]