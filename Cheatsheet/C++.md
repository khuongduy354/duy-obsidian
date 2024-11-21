C++  
see pointers here: [[C++ Essence]]
```c++ 
#include <bits/stdc++.h> // use this for more tools  
  
int a,b;  
string x;  
cin >> a >> b >> x; //1 2 string or 1 2 also works  
// string  
while(cin >> x){} //input unknown amount  
  
long long x = 123456789LL; //LL means long long  
printf("%.9f",x); //9 decimal places  
  
typedef double int di;  
di a = 1; // shortened  
 k 
//macros  
#define PB push_back  
v.PB(); // instead of v.push_back()  
#define REP(i,a,b) for (int i = a; i <= b;i++) //macros with loop  
  
auto //for complicated type  
1e2 // =100  
1e3 // =1000  
  
```  
### Datastructure  
```rust  
array<int, 5> ar1{{3, 4, 5, 1, 2}};  
array<int, 5> ar2 = {1, 2, 3, 4, 5};  
arr.size()  
for(auto i: arr); //loop through array  
sort(ar1.begin(), ar1.end()); //iterator in C++   

//arr same as *arr[0]  
void myFunction(int *arr){} //pass array to function as pointer
  
vector<int> v;  
v.push_back(3);  
//loop,sort like arr  
  
set<int> s;  
s.insert(3);  
s.count(3);  
s.find(3);  
set<int>::iterator it = s.begin(); //set iterator, pointer to a posj in set  
//loop like arr  
  
multiset<int> s;  
s.erase(s.find(5)); //remove 1 instance of multiset  
s.erase() //remove all instance in multiset  
//loop like arr  

// map is a balanced binary tree, NOT a hash MAP 
map<string,int> m;  
m["monkey"] = 4;  
m.count("monkey");  
for (auto x : m) {  
cout << x.first << " " << x.second << "\n";  
}  
//if key not exist, its will be created with value = 0 when first called   

// HASHMAP = unordered_map  , implmented  by hashtable 
-> faster insert, delete, search than map  


  
bitset<10> s;  
bitset<10> s(string("0010011010"));  
s.count() //number of ones  
a|b a&b a^b //bitwise operation   
  
deque<int> d;  
d.push_back(5); d.push_front(3); d.pop_back(); d.pop_front();  
  
stack<int> s;  
s.push() s.pop() s.top();  
  
queue<int> q;  
q.push() q.front() q.pop();  
  
priority_queue<int> q;  
//like queue but add a max or min value  
``` 

# Compound structures
```c++  
class Graph{   
public: 
	map<int, array<int,100>> g; // props
	void addEdge(int u, int v){    //methods
	} 
}  

// ANOTHER WAY (abstract class) 
class Graph{  
public: 
	void addEdge(int u, int v);
} 
Graph::addEdge(int u, int v){ 
}  
Graph g;  
//Graph g(contructor arguments here); 
//see more in C++ essence

// struct 
struct Point{ 
int x;
int y;
} 
Point p; 
p.x = 1 
p.y = 2




```