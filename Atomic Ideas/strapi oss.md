# Structure  
- in packages/core 
- strapi is main folder, which use other plugins (see its package.json)  
- other plugins: content-manager, admin, database, permission   

- most have form of route -> controller -> service 



# Behaviors  
- required field: able to create entry no matter what, just problem if it's publishable or not   
### collection
- if a component field is not required, we can clear all of components' fields, (make it a null object) 
-> then it's publishable both bulk and single. 
- vice versa, component is required, then it can't be null object, and cant be published either bulk or single
# Issues  
## bulk publish 
https://github.com/strapi/strapi/issues/18810 
- unable to publish bulk edit, due to missing required fields 
- but required fields is supplied, just that it inside a component, maybe component hid it.  

- collection -> required component -> required field (image), supply image  
- cant bulk publish, but single works 
```python
Component Thumbnail { 
  image: bin file, required
} 

Collection Test {   
  thumbnail: component, required
} 
 

test entry, publish all errors 
publish single works 
-> data structure is stored correctly and validated 
but something different happens in bulk edit
``` 
### sol 
packages/core/strapi/src/services/entity-validator 
packaages/core/content-manager/server/src/services/entity-manager 

## others 
https://github.com/strapi/strapi/issues/19106 
https://github.com/strapi/strapi/issues/12911









# strapi oss
--- 
# Refererences 




2023 12 27 15:16
#literature  [[oss]] [[backend]] [[low level]]