2022 07 14 16:37
Status: #concept 
Tags: [[file structure]]
# Hexagonal

- example
    

- Visualization
    - The core logic and services are inside,
    - They communicate with various external actors (*ends*) using Ports and Adapters,
    - Those can be divided into driving and driven actors, which again is easy to depict using a symmetrical shape.

**Ports** are interfaces, can server many **adapters** 

**Adapters** actively make communication between external entities and core

**Driving side** start the interaction, UI for example, 

**Driven side** are used by core app, Databases for example,

- explain
    
**Ports**  & **Adapters** on drive side are called primary 

**Ports** & **Adapters** on driven side are called secondary









--- 
# Refererences 
