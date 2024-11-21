### private import
```python  
# by default  A import B, C import A contains B as well 
import kduycontribtest.testpy2 as testpy2 
now = testpy2.datetime.datetime.now() # datetime should not...
print(now)  

# use __all__ 
__all__ = ["needed function"]  # can be put in __init__.py
# by doing this, only needed function is imported 
``` 
### monkey patching  

### auto enable feature when another lib is in dependency  
```python 
#integrations.py
setup_integrations(...)

#client.py 
self.integrations = setup_integrations(
self.options["integrations"],
with_auto_enabling_integrations=self.options[
"auto_enabling_integrations"
],
)  

# auto_enabling_integrations option in setup_integrations = scan lib and auto enable it 

# self.options is generated from DEFAULT_OPTIONS  
# which has auto_enabling_integrations set to True




```









# python sentry oss contrib
--- 
# Refererences 




2023 10 24 16:22
#literature  [[oss]] [[computer science]] [[low level]] [[Python]]