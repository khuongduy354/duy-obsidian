[[programming language]]
2022 07 16 10:29
Tags: #idea  




# decorators 
wrap a function and modify its behavior 

```python
def toUpper(func): 
    def wrapper(): 
        return func().upper()
    return wrapper

@toUpper
def sayHello():
    return "Hello world"

print(sayHello())


```

- coroutine   [[coroutine, generators, thread]]
# package, module
- module is a single file 
- package is a collection of modules   
# [[coroutine, generators, thread]]



--- 
# Refererences 
dabaez
mcoding youtube  

