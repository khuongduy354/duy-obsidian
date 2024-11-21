# Components 
> types of data 
```java
expression     → literal
               | unary
               | binary
               | grouping ;

literal        → NUMBER | STRING | "true" | "false" | "nil" ;
grouping       → "(" expression ")" ;
unary          → ( "-" | "!" ) expression ;
binary         → expression operator expression ;
operator       → "==" | "!=" | "<" | "<=" | ">" | ">="
               | "+"  | "-"  | "*" | "/" ;
```
- the above are the types of an **expression**, 
- to put them to AST correctly, we need to generate them by calling functions in order below 
# Calling precedence 
- Grammar Calling? Each function return expression of that precedence
![[Pasted image 20220809135741.png]]
- precedence from lowest to highest 

- each operator call an operator of higher precedence
-> allow highest precedence to run first! 
```Java
expression     → equality ;
equality       → comparison ( ( "!=" | "==" ) comparison )* ;
comparison     → term ( ( ">" | ">=" | "<" | "<=" ) term )* ;
term           → factor ( ( "-" | "+" ) factor )* ;
factor         → unary ( ( "/" | "*" ) unary )* ;
unary          → ( "!" | "-" ) unary
               | primary ;
primary        → NUMBER | STRING | "true" | "false" | "nil"
               | "(" expression ")" ;
```
- each operator above is a function 

**[[intuition]]**: 
### how to view it 
- structures: 
```javascript
expr = higherOrder()

// handle if right order: +- handled by term, /* handle by factor,etc 

return expr 
```
- each function pass expressions from lower order to higher ones 
- and is stopped at **stations**: equality, comparison,term,factor,power,.. to handle 
for e.g: parsing 1+2
1. 1 is handled by primary,   **passing**
2. stopped by `term()` to handle due to Plus    **stopped at station**
3. `term()` takes rhs, which is 2 from primary   **passing**
4. term parse the expression to binary<lhs,ops,rhs>
### expression() & primary() can be seen as 2 terminals
- expression call all the way up to primary 
- primary **call** expression **which** call **all** the way up to the **recursive** primary. 



# Desgin a parser
--- 
# Refererences 
[Parsing Expressions · Crafting Interpreters](https://craftinginterpreters.com/parsing-expressions.html#recursive-descent-parsing)


2022 08 09 13:58
#literature  [[computer science]] [[Anatomy of a programming language]] [[low level]] [[implementation]] 