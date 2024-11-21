Python cheatsheet  
  
  
# env  
conda create --name py35 python=3.5: create env  
conda env list: list all envs  
conda create --clone py35 --name py35-2: copy env list: list packages of current env  
conda env remove --name bio-env: rm env  
conda deactivate/activate  
  
# cheatsheet  
[https://www.pythoncheatsheet.org/cheatsheet/oop-basics](https://www.pythoncheatsheet.org/cheatsheet/oop-basics)  
```python  
slice()  
list comprehesion  
iterators: map, filter  
enumerate  
>>> a, b = 'table', 'chair'  
"sthing" in iterator == True  
datetime  
  
# strins  
f"{greeting} {name}"  
r"Hello there!\nHow are you?\nI\'m doing fine."  
"""multi  
line"""  
rjust(), ljust() and center()  
split, join  
strip(), rstrip(), and lstrip() #no args = whitespace by default  
  
  
#1st class funcs  
>>> add = lambda x, y: x + y  
>>> add(5, 3)  
  
# lists  
list[-1] # last element  
list.index("sthing")  
# comprehension, replace map and filter  
new = [x+1 for x in old]  
new2 = [x for x in old if True]  
  
#parallel looping  
list1=[]  
list2=[]  
for (i1,i2) in zip(list1,list2):  
...  
  
>>> furniture = ['table', 'chair', 'rack', 'shelf']  
>>> table, chair, rack, shelf = furniture  
  
#tuples, same as list, but immutable  
furniture = ('table', 'chair', 'rack', 'shelf')  
furniture[0]  
len(furniture)  
  
  
# dictionary  
my_cat = {  
'size': 'fat',  
'color': 'gray',  
'disposition': 'loud'  
}  
my_cat["size"]  
.keys()  
.values()  
.items() #as tuples  
.get("key") #none instead of error  
  
pop,popitem,del,clear  
  
# deconstruct  
dict_c = {**dict_a, **dict_b}  
  
# sets  
>>> s = {1, 2, 3}  
>>> s = set([1, 2, 3])  
.add(1), update(3,4,5)  
.remove(), .discard() # discard = remove but no error.  
  
s1.union(s2) # or 's1 | s2'  
s1.intersection(s2, s3) # or 's1 & s2 & s3'  
s1.difference(s2) # or 's1 - s2'  
s1.symmetric_difference(s2) # or 's1 ^ s2'  
  
# errors  
try:  
except Exception as e:  
print(e):  
finally:  
  
  
>>> def some_function(*args, **kwargs):  
#args is array  
# kwargs dict  
... pass  
  
>>> some_function(arg1, arg2, arg3="value")  
```  
### libs  
```python  
[https://www.pythoncheatsheet.org/cheatsheet/file-directory-path](https://www.pythoncheatsheet.org/cheatsheet/file-directory-path)  
file IO  
json.load(),dump  
```  
### OOP & typing  
```python  
class Something(inherit):  
	def __init__(self,arg):  
		self.prop1=arg  
  
@abstractmethod #has no implementation  
@abstractclass #cannot be instan, only inherits  
inst = Something()  
  
#data class  
#custom typing, like struct in rust  
@dataclass  
class Product:  
... name: str  
... count: int = 0  
... price: float = 0.0  
  
obj = Product("Python",1,2.2)  
obj.name
```  
### TESTING  
```python  
pytest (file start with test_ or tests folder)  
# [test_something.py](http://test_something.py)  
def test_stuffI():  
assert 1==1  
```  
### FILE STRUCTURING  
```python  
import module (module = same dir)  
from package import stuffs  
(package = folder has __init__.py)  
  
from kundi import sth #absolute startin from process  
from ..kundi import sth #relative  
```  
  
# Advanced  
### async await simulated by coroutines  
[https://fastapi.tiangolo.com/async/#in-a-hurry](https://fastapi.tiangolo.com/async/#in-a-hurry)