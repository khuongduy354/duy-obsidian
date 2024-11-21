# Nodejs Chat App
## BytebytesGo
- both servers, both databases just like me
- relay: user online = send, else PN 
- message queues
- service discovery
- multiple ws servers
- time based nosql db for messages
https://bytebytego.com/courses/system-design-interview/design-a-chat-system

### Tech Stack
- Nodejs
- Websocket
- React.js
- MongoDB
- PostgresSQL
- Elastic Search
- Cloud static storage

### Apps Description
Like messenger

- Real-time people chat
- Groups
- Multimedia supported: images, files, videos, audio,...
- themes: single img bg, parallax bg, color wheel, msg colors, emoji  
- Authentication
- Persistent chat
- Searching users & messages ? 
- Friends
- Online status 
- Video call 

### Detailed 
- User sign in (OAuth)
- Just a list of recent chatted users, and main chat panel
- Find by user ID, or name 
- Chat with text or other medias
- Can customize themes and stuffs



# Implementation  
### friend request 
- FriendRequest table, Friend Table  
-> send a request, add to FriendRequest table (asymmetric), have an accept request endpoint, where check if request available, and proceed add friend  
POST /me/friend/request  
GET /me/friends/request  
POST /me/friend/add

https://socket.io/get-started/private-messaging-part-1/  
### Id generation   (identification)
- current uid4 enough  
- do not use OAuth as id, as there're many different providers, each have diff ids. Try associated 1 user with 1 oauth only
- attempt to create a user logged into user if not existed 
### group room 
- when created, group has an id  
- but i have no way to retrieve that!! 
### 1 1 messaging  
- use user id not socket.id  
-> auto join on connect user  
- use session, 2 user same session  
- join self id as room on socket -> more intuitive 3  
- use a separate table for 1-1 chat, as querying exactly 2 members in Group table is complex, i gotta scale it later anyway     
- another way is to use Friend middle table id for dm chat room  
-> unfriend = lose chat ? NO way 
 
- group endpoint, join and get, as user usually just join from friend list, and not sure if room available yet
-> avoid extra query on frontend 
- can i do the same for group? NO, because group must be created in a different operation, and then join
### group messaging 
- same above, instead of auto join group, have a separate event handler when player join a group 
### friends 



### websockets
 ![[nodejschatapp.jpg]]
 
 - decouple ws handlers

### Database design
- messaging app, so we want it as realtime as possible, NoSQL for faster read write 
- May use SQL for themes, user and stuffs   


- Cassandra handle large scale very good but slower MongoDB  


**individual message**
user {key-value<user_id, conversation>}
conversation {sender, content, medias,...}
-> query much quicker, cost more space

or
(separate conversation)
conversation{sender, receiver, content, medias...}
-> query slower, but less storage

**group message**
group{users, conversation}
user{groups}
-> embed in both side

https://www.youtube.com/watch?v=xL_tYrEcP9M&list=PLZDOU071E4v6epq3GS0IqZicZc3xwwBN_&index=8
-> traditional SQL

### message pagination
- get_messages: retrieve 10 recent message of specified user, when scroll up get 10 more, limit and order_by query
-> react has to keep track of the chunk
should i do this as Http or Websocket? 

### online  
- socket.io hearbeats automatically 
- just need to handle on disconnect and connect  
- make seperate handler for this  


1. database: write and emit mongodb change event, then update based on that-
- when server down, and up, can still update status 
- last online time   
- other part can use this status: like typing effects, analytics, other ws server (scaling) 


2. update status immediately on disconnect   
- server down? Boom, all client still stuck on previous one![[]] 
-> but much easier, since i dont need a very database, user can just refresh page, use this method

### theme  & emojis 
theme consist: 
- background: either parallax or normal image, or single color 
- sender text, and the rest text   
emojis consist:     
- each user can have an array of emojis 


### supabase
- i shouldn't have this, but the auth is crazy 
https://github.com/supabase/supabase/issues/491
https://supabase.com/docs/reference/javascript/auth-getuser
- literally, what it does is that supabase allow login with OAuth, and then return an access token (JWT),  
- but no docs to verify that 
-> very simple: just call getUser(jwt) to verify 
- no need to pass token to socket, use supabase.getAuth  

- flow: client OAuth-> post /signin (create if not available) 
- update: above not possible anymore, supabase client and server diff, so have to pass token, and check for each socket as usual
### supabase cloud storage  upload 
- multer take formdata -> buffer
- i use nodejs buffer, upload, but had to specify content type  though, 
-> buffer without content type,not renderable
- if i upload blob, whether content type or not, it would still be  octet byte stream (which is not renderable)

# Message Queue    
[[message queue]]
- decouple 
- async: 
+ error tolerance: can retrieve lost message (retry feature)
+ decouple: PN service, DB store service, I/O service 
[[scaling,scale websockets with message queue]]

### which one? Redis Pubsub, Kafka, RabbitMQ 
RabbitMQ redis see below 
- Redis in-memory, Kafka disc => redis faster  
- Redis dont need dealing storage like Kafka 
- Create a topic in Kafka is costly 
-> Kafka persists however, but we can do that with db 
-> Kafka scales better (high throughput) 


# Multiple chat (websocket) instances 
1. Nodejs cluster: more complex code
2. Nodejs worker thread: nope, cuz it's CPU bound  
3. Nginx: yep, maybe more efficient, see references 
 https://www.reddit.com/r/node/comments/11p9hhc/worker_thread_vs_cluster_with_websockets/ 

# Learnt   
- file codec: from client formdata, to multer buffer, nodejs  stuffs 
- is form data multipart ? 
- combination constraint: 2 keys unique, symmetry unique,..  
- write a rpc postgres 
- Unclear how to speed up one to many from the one side  
- What is relationship in sql diagram for ? (SQL diagram differs from ERD) -> for entities (rows) number
- Enum is better to have another mapping table 
- best id generator? 
- online implementation (choose no database method)
- friends symmetrical M2M rel, make union query    
- supabase RLS is ????!!! 

how to design messages    
- timed id(mongodb)   
- content  
- sender id  
- group id (dm is group with 2 people)  

how to query 
when we load group -> find X msgs s.t group_id == msg group id 

- SQL atomic: friend request accepted = addfriend and delete request 



```database schema
Theme {
	id integer pk
	bg integer > Background.id
	msg_sender_color string
	msg_other_color string
	author integer *> User.id
}

Emoji {
	id integer pk
	img_url string
	author integer >* User.id
}

Background {
	id integer pk
	type integer > BackgroundTypes.id
	single_img_url string
	single_color string
}

BackgroundTypes {
	id integer pk
	name string
}

ParallaxImage {
	id integer pk
	img_url string
	speed int
}

BackgroundParallax {
	bg_id integer > Background.id
	parallax_img_id integer > ParallaxImage.id
}

User {
	id integer pk
	name string
}

Group {
	id integer pk increments
	name string pk increments
	admin integer > User.id
	current_theme integer > Theme.id
}

Friend {
	user1_id integer pk > User.id
	user2_id integer > User.id
}

GroupUser {
	user_id integer pk increments > User.id
	group_id integer pk increments > Group.id
}

GroupEmojis {
	emoji_id integer > Emoji.id
	group_id integer > Group.id
}
```


# Upgrade
- push notification React 
- msg queue  done
- nginx + 2 ws servers

# Nodejs Realtime chatapp
--- 
# Refererences 

https://fullstackmastery.com/page/38/why-you-should-not-use-rabbitmq-for-realtime-messaging 
https://bytebytego.com/courses/system-design-interview/design-a-chat-system 
https://www.reddit.com/r/node/comments/11p9hhc/worker_thread_vs_cluster_with_websockets/



2023 12 24 11:28
#literature  [[backend]] [[system design]] [[]]