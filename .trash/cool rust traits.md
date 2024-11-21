# Send & Sync traits
## Send
> this trait can transfer ownership between threads
-   rc doesnt have this trait
-   most primitives data type has

## Sync
> this trait can allow referenced between threads
-   any type `T` is `Sync` if `&T` (an immutable reference to `T`) is `Send`
# Display trait 
# From trait 
# Deref
- return address of a type  when use * (dereferencing) 
-> only use for smart pointers 
```rust 
struct A{} 
*A // we can do this
*(A.deref()) // equal this 
```



# cool rust traits
--- 
# Refererences 




2022 09 13 20:29
#cheatsheet [[Rust]]