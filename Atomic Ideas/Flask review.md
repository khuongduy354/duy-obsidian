- app.py <- main file glue everythings  
```python 
app = Flask(__name__) #create a Flask instance 
# name is used for resource lookup 

```
# The flask class 
- inherit Scaffold, for shortcut properties. 
- create a flask instance which set/get metadatas: 
-> cookies, cache age, ... 
- typical stuffs: 
	- constructor setup 
	- jinja (has template filter)
	- .env 
	- blueprint supports 
	- Routing  
	- exceptions, logging.  
	- main wsgi app, supports middleware.
	- request: get request context, match URL, return View or ErrorHandler. 
	- response: make_response from request above. 
	- async: see [async flask](https://flask.palletsprojects.com/en/2.2.x/async-await/#performance)
# The blueprint class    
> modular code
```python
simple_page = Blueprint(...)  # customize stuffs here
@simple_page.route(...) 

# instead of 
app = __Flask__(__name__) 
@app.route(...)
```
- consists of routes & functions
-> can help making funcs w/out flask instance
-> which can be then registered into the instance 
- has many similar methods in Flask class (json encoder,decoder)
# Nitty gritty 
- has 3rd party typing
- convert methods -> decorators (routing, templating)







# Flask
--- 
# Refererences 
[flask/app.py at main · pallets/flask · GitHub](https://github.com/pallets/flask/)



2022 09 10 09:51
#literature [[Python essence]] [[framework]] [[OSS]] [[Python]] [[low level]] [[computer science]]