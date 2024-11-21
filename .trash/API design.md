2022 07 14 16:40
Status: #cheatsheet 
Tags: [[system design]] [[best practice]] 
# API design
# [[Rest API ]]
### Naming conventions
1. use **nouns** for name (methods are actions) 
2. **lower** **case** , **hyphen - ,** 
3. / for hierarchy, **general** to **detail** 
4. no file **extensions**


- most complicated endpoint: 
_collection/item/collection_.
-> customers/1/orders/99/products changed to customers/1 and orders/99/products
- use plural for collections, {id} or {name} for item 

# Spotify is a perfect reference 
[Web API Reference | Spotify for Developers](https://developer.spotify.com/documentation/web-api/reference/#/operations/get-track)



--- 
# Refererences 
[https://s3.amazonaws.com/tfpearsonecollege/bestpractices/RESTful+Best+Practices.pdf](https://s3.amazonaws.com/tfpearsonecollege/bestpractices/RESTful+Best+Practices.pdf)

[microsoft api design]()





