# TODO        
- Chat realtime

# MVP

- User Authentication 
- Create, Read, Update, Delete (CRUD) users endpoints
- CRUD organization endpoints
- Recommendation algorithm design and implementation
- Tracking users matching implementation
- Implement FTS (Full text search) searching using Elastic search
- How users contact ? Mail box or realtime chat or none?




# Algos 
Algos:  
- gale shapely: https://medium.com/@osmanelsoz/tinder-and-matching-algorithms-351b538ac83f
- TinVec A.I: https://rpubs.com/nancunjie4560/832396  -> represent swipes in 2D vector, to aggregate and train model 
-> consider transform ur SQL table (Boxes) to this. 

Technicals 
- Core: https://www.techaheadcorp.com/blog/understanding-system-design-architecture-of-tinder/ 
-> low latency recommendation engine 
-> Full text search for search & matching (2 swipes right) 
- A.I model: https://www.geeksforgeeks.org/predict-tinder-matches-with-machine-learning/ (with code)
- active cache (LRU)



# Features details 
- Authentication: Đăng nhập bằng Gmail, hoặc Facebook  

- Profile:  
1 account user sẽ có 2 mode là Employer và Employee   
Employer: personal profile, view applied employees, đăng post tuyển dụng
Employee: cv profile, view applied jobs 

Organization page: descriptions, recruitment posts, monitored by employers


- Tính năng Meet (swipe): 
Employee sẽ được recommend employee/employers khác. 
Swipe phải vào box thích, trái vào box ko thích. Có thể xem lại để connect

Nếu 2 bên match nhau thì có thông báo để encourage 2 bên connect. 


- How to Connect??:  
1. Tin nhắn realtime (như Messenger)  
2. Hộp mail (như Gmail)


Thuật toán recommend: 
// để tính sau


Candidates 
- Post cvs  
- View orgs, swipe to archieve liked & unliked   
- Have apply buttons 

Orgs    
- Post orgs info
- View cvs, swipe to archieve liked & unliked    
- Have invite buttons

Both LIKED === MATCH  
Recommendation algorithm
Searching 



### Potential features
- Notification system
- Profile scanning from a pdf/image  














# swipeswipe
--- 
# Refererences 




2024 07 23 00:27
#literature  [[backend]] [[computer science]] [[system design]] 