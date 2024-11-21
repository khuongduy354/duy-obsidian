- shoot enemy ricochet to other places
- ricochey to spawner to deal dmg 
- hold to aim (guided dots)

- spawner I,II,III
a bunch of features due to number of ricochets 


# FULL PLAN 
# Ricochey Game Mechanics 

# Ricochey 
- Bullet hits target bounces off
- Bullet of a weapon must bounces off X times before able to deal damage. X is called Ricochey 
-> For example: Bow has a ricochet of 1: its arrow will bounces once when hit target, then the next time it hit, it deals damage instead of bouncing
- It cannot bounce when hit walls, or else it'll break the game
- Based on ricochey we can do different things
1. high ricochey high damage weapon (more complex = more reward) 
2. low ricochey low damage 

# Weapons 
- Bow: low ricochey, low damage
- AK: high ricochey, but medium damage with AoE (2 max) 
- HandGun: med ricochey, medium damage 
# Characters
- ZelBoy: default
- Dinosaur: tougher than zelboy

# Enemies & Spawner mechanics 
- Tier 1 spawner spawn only tier monsters 
 
### Tier 1 monsters
- Zombie
- Orc
- Red Demon 
### Tier 2 monsters (weapon)
- Skelly 
### Tier 3 monsters (weapon + ability) 

### Spawner algorithms 
- It has a Donut spawn zone 
- after X seconds spawn a random mob (of that tier) in a random position
- dont spawn if position already has a mob 

### Enemy AI
- Idle, Patrol, Chasing states 
- Stay idle after X secs, patrol in specified zone
- If player enter chasing zone, switch to Chasing state immediately
- If player goes out of chasing zone after Y secs, switch back to idle state

# World Map 

### One big map 
-Player enclosed in walls, inside, there're spawners


### Campaigns 

### Procedural

# Extras
- All spawners down win
- Scoring systems
- Endless Mode (to farm $$)
- Ricochey count more satisfying 
- effect when full ricochey reached
- 


# V.2 
RELOAD
T3 mobs and bosses
 Speial ability that involve ricocheys: 
 - change direction 
 - reduce number of ricochey











# Ricochet
--- 
# Refererences 




2022 12 27 22:57
#literature  [[game]]  [[ideas (games, apps)]] [[godot]] [[gamedevlog]]
