- unlike a function (subroutine) which has an entry and exit, 
-> coroutine, can have multiple entries. 
### python example 
```python 
`def` `print_name(prefix):`

    `print``(``"Searching prefix:{}"``.``format``(prefix))`

    `while` `True``:`

        `name` `=` `(``yield``)`

        `if` `prefix` `in` `name:`

            `print``(name)`

# nothing happens first run 
`corou` `=` `print_name(``"Dear"``)`
# run function -> yield called -> pause function, back to main program
`corou.__next__()`

# continue code after yield. 
`corou.send(``"Atul"``)`
`corou.send(``"Dear Atul"``)`

# Output 
Searching prefix:Dear
Dear Atul

#exit
corou.close() # this throw Error in corutine, can caught by try except

```

### generators 
- is a coroutine, but simpler. 
- generator cannot take arguments (except when first initialized), a.k.a no send method. 
### compare to multithreading 
- context switching is determined by machine 
- generators/coroutine is determined by programmer. 











# coroutine, generators, thread
--- 
# Refererences 




2022 09 10 09:31
#ideas       [[programming language report]] [[multithreading]]