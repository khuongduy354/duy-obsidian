[[OOP]] [[design pattern]]
2022 07 16 21:50
Tags: #ideas 
# Composition over Inheritance
# Composition 
> Like [[inheritance]], but instead of creating an object by what it is,
> objects are create by what it does 

```javascript 
class Parent{
method1:()=>{}
} 
class Child extends Parent{
method2:()=>{}
} 

// composition 
function method1Maker(state)=>{
return method1 
}
function method2Maker(state)=>{
return method2 
}
function objectMaker()=>{
//combining methods above
return object_has_both_method 
}

```
## Core
- combine based on functionalities, state is passed around 
- works like Traits (Traits are still cleaner)
- avoid Inheritance trees which may caused future problems 






--- 
# Refererences 
[Composition over Inheritance - YouTube](https://www.youtube.com/watch?v=wfMtDGfHWpA)
