# fanout on write: 
## When a user post something:
1.  Get n follower
2.  Push (fanout) this post to n follower
## When a user get a feed:
1.  Get from `Newsfeed` collection

#  fanout on read: 
## When a user post something:
1.  Save
## When a user get feed
1.  Get n following
2.  Get posts from n following
# pagination 
- more applicable for fanout read
-> limit query -> faster lookup
- in fanout write, have to write beforehand
-> wasted writes due to skipped items in paginate  
# caching 
- cache active users

# infinite scrolling  
[[infinite scrolling duplicates problem]]









# newsfeed generation
--- 
# Refererences 
[node.js - Building a simple news feed in node + Mongodb + Redis - Stack Overflow](https://stackoverflow.com/questions/39940307/building-a-simple-news-feed-in-node-mongodb-redis)



	
2022 08 27 11:08
#literature   [[backend]] [[system design]] 
