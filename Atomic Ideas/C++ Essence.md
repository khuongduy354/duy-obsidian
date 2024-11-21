[[C++ Qt]] 
[[cmake]]  
### pointer 
`void*` can be cast to any other pointer type implicitly 
### reading file 
```c++
  std::ifstream ifs;
  ifs.open(sFileName, std::ifstream::binary);

struct sHeader{a;b;c;d}header;
  if (ifs.is_open()) {
  char* result; 
    ifs.read(result, sizeof(sHeader)); 
   
   //bonus: read from struct 
    ifs.read((char *)&header, sizeof(sHeader));
 
}     

```

### String is dynamically created, while str[] is statically 

→ str[] may waste memory, string not

→ str[] may has [array decay](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/), string not

[new operator](https://www.geeksforgeeks.org/new-vs-operator-new-in-cpp/#:~:text=new%20keyword,memory%20to%20the%20pointer%20variable)
→ allocate on **heap,** return address of that heap

### Why dynamically allocation is needed 
- as above string and str explained 
- also: it gives you the largest memory (as can you as much as possible until delete) and freedom 
- 
### Array decay 

→ when pass array into function through pointer, the sizeof() return pointer size, not array size

→ pass in length as another parameter, dont use sizeof() 

### virtual 
```c++ 
class Parent{
void A(){}
}
class children{
void A(){};// to overload, parent needs to be virtual
}


virtual void A() = 0 ; 

Sweeper *sweeper1 = new Sweeper(500,FORWARD)
```
# OOP  
## init an object 
- static -> build-time, remove when out of scope 
- dynamic -> run-time, delete manually
[Different ways to instantiate an object in C++ with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/different-ways-to-instantiate-an-object-in-c-with-examples/)
```cpp

	// Creating automatic storage object
	example obj1;
	example obj1(10);
	obj1.set(5);

	// Creating dynamic storage object
	example* obj2 = new example();
	example obj2 = example(8); //if has param 
	obj2->set(10);

	example* obj3 = new example(20);

	delete obj2; 
	delete obj3; 

```
## copy constructor  
- continue above (init object) example 
- copy dynamic -> dynamic 
- copy static -> static 
```cpp  
class example {
    int x;
 
public:
    // Parameterized constructor
    example(int y)
    {
        x = y;
        cout << "The value of x is: "
             << x << "\n";
    }
 
    // Copy constructor
    example(example& obj)
    {
        x = obj.x;
        cout << "The value of x in "
             << "copy constructor: "
             << x << "\n";
    };
};

    // Instantiating copy constructor
    // using Method 1
    example obj1(4);
    example obj2 = obj1; //deep copy
    example obj3(obj1); //deep copy
 

    // using Method 2
    example* obj4 = new example(10);  //dynamic 
    example* obj5 = new example(*obj4); //deep copy of dynamic
 //hence obj5 is dynamic
delete obj4; 
delete obj5; 
```





# C++ Essence
--- 
# Refererences 




2022 11 17 17:13
#literature  [[Atomic Ideas/C++]] [[programming language]]  
