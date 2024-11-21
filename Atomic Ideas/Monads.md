[[design pattern]] [[functional]]
2022 07 16 20:02
Tags: #idea 
# Monads
> Handle wrapped types
```typescript
//monad handle wrapped types 
x:Wrapped<T>
run(x,square)

// allows better transform function def
function square(input:T):Wrapped<T>
//instead of this:
function square(input:Wrapped<T>):Wrapped<T>
//-> functions dont have to handle wrap 



//monad is chainable 
square(x) //normal 
run(x,square) //monad

addOne(square(x))
x.run(square).run(addOne) 

```
## Applied 
- Wrapped types like Option 
- Wrapped data with logs 
![[Pasted image 20220716202358.png]]
## Core ideas 
- **runWithLogs** handle **wrapping** part
-> **transform** **function** doesnt have to 
-> avoid duplicates code 

--- 
# Refererences 
[The Absolute Best Intro to Monads For Software Engineers - YouTube](https://www.youtube.com/watch?v=C2w45qRc3aU)


