# Continue
reading docs on advanced  
try fixing a few bugs  
compare with reqwest in Rust 
# Features 
- Keep-Alive & Connection Pooling    
![[Pasted image 20230923124258.png]]
- compression gzip 
- codec : multipart, json serialized
### session    
> persist parameters across request
```python
s = requests.Session()
s.headers.update({'x-test': 'true'}) # both requests below has this header
r1 = s.get('url')
r2 = s.get('url2') 
``` 
### authentications  
> can implement more 
```python 
from requests.auth import HTTPBasicAuth

basic = HTTPBasicAuth('user', 'pass')

requests.get('https://httpbin.org/basic-auth/user/pass', auth=basic) 

# the code above add username, password into Authorization header request  

```



# Flow   
### send a request
/src/requests/\_\_init\_\_.py  
init take get, post funcs from  .api 
.api make request due to Session.request 
Session.request -> Session.send() -> adapter.send(), adapter is what sending the request     
-> adapter, making the urlconnection through urllib3  (urlopen method) 
### session.send() is keypoint   
it send request using adapter, handle redirect, cookies, history  



# Patterns 
#### ADAPTER PATTERN 
send request above, adapter, between SESSION  & URLLIB3   
### Python 
- polymorphism???? cookie dict and cookie jar both received by .get
-> actually, it's not, python has \*\*kwargs which can take any      
- polymophism: various authentication class 
- split exceptions: network failure, server failure, client failure  
-> python has no enums, so use class for error   
- cookies jar: wrapper for more powerful dict for cookie
->set domain on each cookie   


# Codebase architecture 
> VERY SIMPLE, everything inside src/requests, no nested 
- api.py: expose api: get, post,... 
- models.py: contain most class (except session, auth,cookies,... )  
- structures.py: special data structures (CaseInsensitiveDict for example)  
- adapter (above) between SESSION and URLLIB3
### enum likes  
- status_code.py: in dictionary style 
- exceptions.py: tons of classes inheriting BaseError 

### custom behaviors
- hook.py hook system 
- 


 


# notes  
- sometimes exposing a few code from child to parent (repeating) is fine   










# request
--- 
# Refererences 




2023 09 23 12:35
#literature  [[Python]] [[OSS]] [[low level]] [[computer science]]