2022 07 14 16:34
Tags: [[programming paradigm]] [[java]]
# OOP
- when initiating

- constructor

```java
Test t = new Test();
Test t2 = (Test)t1.clone(); //deep clone obj

//DESRIALIZATION 
FileInputStream file = new FileInputStream(filename);
ObjectInputStream in = new ObjectInputStream(file);
Object obj = in.readObject();

Test test = new Test();
test = new Test();  // this destroy old object, due to gc 

//annonymus object initated,  called and destroyed immediately 
```

# Advantages

1. Model real world 
2. Code reuse 
3. Scale 

# Disadvantages

1. Overengineer 
2. Tricky relationship of objects 





--- 
# Refererences 
