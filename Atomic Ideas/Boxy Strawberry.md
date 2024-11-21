- a character inside dirt box
![[Pasted image 20230108112101.png]]
- plants grow and shoot at character 
- press WASD to rotate the dirt 
- only allow up down movement for character
# Spawn algo 
- Mobs: 
	- spawn_chance point (like AI level in FNAF) 
	- phases 
	- grow_time (per phases)
- Box: 
	- spawn_rate (in seconds): every X seconds choose a plant based on info above, may choose not to spawn
### comment 
- option above: spawn every X secs, increase plants only increase **diversity**, does **not** generate **more** mobs, but **increase** **spawn_rate** doess. 
- remaining empty tiles decide spawn, range 1% -> 98% of not spawning
-> remove recursive function  
- another option: remove spawn_rate, use a customized algo only depend on mobs spawn_chance -> more complex 
### what i done 
- 1st time: 
```
run spawn_algo() every X secs:

choose 1 random pos

if that pos not empty → stop

if empty → spawn 1 random mobs (from mobs list)
```
# Boxy Strawberry
--- 
# Refererences 




2023 01 08 11:20
#ideas   [[game]] [[gamedevlog]] [[ideas (games, apps)]] [[implementation]]