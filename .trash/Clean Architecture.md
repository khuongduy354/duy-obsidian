2022 07 14 16:17
Status: #concept 
Tags: [[system design]], [[architecture]] 
# Clean Architecture 
- Exetended full
- Presentation logic - **Infra**
    
    The UI, HTML CSS Javascript 
    
    Can split into 2: 
    
    - Dumb (plain) components
    - Smart (dynamic) components
    
    Can be SSR, CSR,... 
    
- Data access / Adapter logic - **Adapter**
    
    3rd party stuffs, 
    
- App logic / Use cases - **Domain** ← **USE CASE DRIVEN**
    
    > Featurs of app
    > 
    
    ### **CQS / CQRS**
    
    Use Cases are either `COMMANDS` or `QUERIES` if we're following the [Command-Query Segregation](https://khalilstemmler.com/articles/oop-design-principles/command-query-segregation/) principle.
    
    **Use Cases are application specific**
    
    - examples
        
        Admin panels
    







--- 
# Refererences 

[Organizing App Logic with the Clean Architecture [with Examples] | Khalil Stemmler]
