Rust cheatsheet  
# libs  
serde  
clap  
tokio  
anyhow  
reqwest  
structopt  
  
# Cheatsheet  
### arbitrary stuffs  
```rust  
.as_ref() //convert &Option<T> to Option<&T>  
.canoncalize() //relative path, not include ~, and file must exist!  
Cow<>  
```  
### File structuring  
```rust  
Crate = bin or lib  
Package = anything that has a Cargo toml  
[Managing Growing Projects with Packages, Crates, and Modules - The Rust Programming Language](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)  
  
IMPORT EXPORT  
src  
├── [main.rs](http://main.rs)  
|-- [error.rs](http://error.rs)  
└── types  
├── [expr.rs](http://expr.rs)  
├── [mod.rs](http://mod.rs)  
└── [token.rs](http://token.rs)  
```  
``` Rust  
//[mod.rs](http://mod.rs)  
pub mod expr;  
pub mod token;  
  
//[main.rs](http://main.rs)  
//bring to scope  
mod types;  
mod error;  
//use items (fn, mod)  
use types::{}  
  
//external crate dont need bring to scope  
use rand;  
  
WORKSPACE & CARGO TOML  
// nested package has nested Cargo.toml  
//root cargo.toml  
[workspace]  
members = [ "adder", ]  
  
//member 1 access to other workspace  
[dependencies]  
add_one = { path = "../member2" }  
  
LIBARY [lib.rs](http://lib.rs)  
lib dont need to be included (by mod)  
// src/lib.rs  
pub fn something(){}  
// src/anywhere.rs  
use app_name::something  
  
```  
### TESTING  
```rust  
// root/tests or root/src/test.rs  
// inside root/src/test.rs  
#[cfg(tests)]  
mod test{  
#[test] //anything has this and inside src will be found by cargo test  
#[should_panic] // panic = test pass  
#[ignore] // not run unless specify (for expensive test)  
fn test_add  
assert!(something,"Fail msg")  
  
cargo test add // run test contains part of letters  
}  
  
//integration test, same level Cargo.toml  
tests/anything.rs  
//can only import [lib.rs](http://lib.rs)  
  
cargo test find the 2 location above  
```  
### SETUP shortcut  
```  
cargo new appname  
cargo login -> cargo publish  
cargo build  
cargo check  
cargo test  
cargo add  
```  
### File IO  
```rust  
// ================= SINGLE FILE ===================  
// open  
let mut file = File::open("path/to/file.txt").unwrap();  
  
//read full to string  
let mut s = String::new();  
file.read_to_string(s);  
  
//create  
let mut file = File::create("path/to/file.txt").unwrap();  
  
//write  
file.write_all("asda".as_bytes());  
  
// read by lines  
let mut file = File::open("path/to/file.txt").unwrap();  
io::BufReader::new(file).lines().unwrap() //iterator  
  
// ===================== FILESYTEM =====================  
fs::create_dir("a")  
fs::create_dir_all("a/c/d") //recursive create dir  
fs::remove_dir("a/c/d")  
fs::remove_file("a/c/e.txt")  
fs::read_dir("a")  
```  
### ERROR handling  
```rust  
// use thiserror crate for all below  
  
fn hi()-> Result<value,error>{  
let a = some_return_result?; //propagate  
let b = some_return_option.ok_or(Err); // propagate  
}  
  
  
// ============ CUSTOM ERROR TYPE  
// implent traits Error (fn source) ,Debug, Send, Display, Sync  
enum MyappError{  
In(std::io::Error),  
Out(std::io::Error)  
}  
  
// Type-erased Error, usage for non-useful error (algo implemented wrong)  
  
```  
### profiling, benchmarking  
criterion  
  
### Traits implementation  
Display  
Iterator  
  
  
  
  
### Strings and other data types  
See exercism  
```rust  
OSstring: native platform string, can be cheaply converted to string  
=> Linux, Window,... interpret string differently, so OSString is needed  
  
```  
### One liners  
  
  
  
  
# Advanced concepts  
## Async (see more in obsidian)  
```rust  
// basics  
enum Poll{Pending, Resolve}  
async fn // provided by rust  
Future{ //provided by rust  
type Output  
fn poll()->Poll{}  
}  
  
//Usage (tokio for I/O heavy, crayon for CPU heavy)  
async fn sads()->Future{  
async { } // sugar for return Future  
}  
#[tokio::main] // sugar to run async on main (main cant be async technically)  
async main(){}  
  
  
//return when 1 future done  
// partial copy problem  
select!{  
result<-fut1.await{ // run if resolve }  
result2<-fut2.await{ }  
}  
  
// wait till all future complete  
join!{}  
}  
```  
  
  
## bytes, buffer , usage in writing to file  
```rust  
byte = 8 bit  
buffer = a region of memory  
  
.write_all(b"adsfdas")//string convertible to bytes.  
.write_all(buffer)//buffer = chunk, write a chunk a time save performance  
  
but bytes is usage for real time, as info update more frequent, also to save buffer memory  
but buffer is performant, also for networking.  
```  
  
## string datatype  
string, &str,.. why one or the other.  
[https://www.youtube.com/watch?v=rAl-9HwD858&list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvOa](https://www.youtube.com/watch?v=rAl-9HwD858&list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvOa)  
![ede0dfed0b3907d0ee1f58ecb816141c.png](:/a3d3cf3ac3cb454795b2af1fd52ad567)  
  
-> performance wise  
## Data types, References  
```rust  
let &mut T = a //match &mut T of a  
let ref mut T =a //match mut T of a, then takes a reference  
  
  
```  
  
## Lifetime  
- avoid accessing variable that is dropped
- usually reference involve lifetime, but some can be guessed (inference) by Rust  
- examples lifetime needed: struct with refs, generic types, &str
```rust
struct MyStruct {
    data: i32,
}

fn get_data(s: &MyStruct) -> &i32 {
    &s.data
}

// lifetime of return value = struct lifetime because it's
// a field of Struct!

// however  

data: &i32 

// have to specify lifetime cuz it's might not be the same

```
[https://doc.rust-lang.org/rust-by-example/scope/lifetime.html](https://doc.rust-lang.org/rust-by-example/scope/lifetime.html)  
[https://www.youtube.com/watch?v=rAl-9HwD858&list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvOa](https://www.youtube.com/watch?v=rAl-9HwD858&list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvOa)  
```rust  
pub struct Skdjaks<'a>{  
stra:&'a str  
}  
impl<'a> Iterator for StrSplit<'a>{  
type Item =&'a str  
}  
  
// Lifetime tells how long variable live til it moved, drop, out of scope  
  
// Life time of orignal >= borrow's lifetime  
  
<'_>  
// annonymus lifetime, guessed by compiler  
  
```  
### multiple lifetime  
to be continued  
  
### green threads  
green threads = threads spawn by runtime (instead of OS), usually faster, more manageable.  
Rust dont have, cuz it has no runtime  
  
  
  
## Reference counting  
cuz it has no garbage collection. when ref count =0, it got destroyed, instead of garbage collector destroyed
