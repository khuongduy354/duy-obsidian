2022 07 14 16:27
Status: #concept 
Tags:[[design pattern]] 
# Singleton 

> In object-oriented programming, a singleton class is a class that can have only one object (an instance of the class) at a time
> 

After first time, if we try to instantiate the Singleton class, the new variable also points to the first instance created

### How to make one

1. Make constructor private.
2. Write a static method that has return type object of this singleton class. Here, the concept of **[Lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization)** is used to write this static method.

### Benefits

Reduce the use of global variable

→ it limits namespace pollution and associated risk of name collisions









--- 
# Refererences 
