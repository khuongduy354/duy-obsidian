# Rust Essence      
- [[cool rust traits]] 
- Literatures  
# Rust learned from projects: 
[Search Rs](https://github.com/khuongduy354/search-rs) 
[GitHub - khuongduy354/git.rs](https://github.com/khuongduy354/git.rs)
[GitHub - khuongduy354/DuY-compiler](https://github.com/khuongduy354/DuY-compiler)


# Unsafe rust
![[Pasted image 20221211170639.png]]
# Multithread in rust  
### Threading
disadvantages of multithread
![[Pasted image 20220913201024.png]]
![[Pasted image 20220913201052.png]]
→ Rust is 1:1, resulting in low runtime cost
### Basic syntax for multithread
```rust 
use std::thread;
use std::time::Duration;

fn main() {
//a thread that run function 
//switch context when sleep. 
    thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
//when main thread ended, any spawned thread also stop
}

//wait for thread finish, no context switching
let handle = thread::spawn(...);
handle.join().unwrap();
```
# Sharing between threads
###  Messaging
> channel is like a stream, anything put upstream is received at the bottom
```rust
use std::sync::mpsc; //multiple producer single consumer,

fn main() {
    let (tx, rx) = mpsc::channel();  
	//transmitter, receiver

	thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();  //sending to upstream
    });


let received = rx.recv().unwrap(); //recv = receive synchronously
try_recv // receive asynchronously
}

RECEIVING MULTIPLE MESSAGES
//treat rx as iterator, synchrnously. 
for received in rx {
        println!("Got: {}", received);
    }

MULTIPLE PRODUCERS 
let tx1 = tx.clone(); //use tx1 in a different thread 
```
### Mutex 
> Allows 1 thread to access data at a time, unlike message consumes data.
**rules to use mutex:**: 
1. acquire lock before access data
2. unlock when done. (drop it)
```rust
let counter = Mutex::new(0);
let mut num = counter.lock().unwrap(); //return a MutexGuard after unwrap 
//MutexGuard implement Deref & Drop, which drop locks when out of scope
```
### Arc
> Mutex but for multiple references.
> Or Rc in multithreading.  
![[Pasted image 20220913202544.png]]

# Refcell/rc & mutex/arc similarities
![[Pasted image 20220913202751.png]] 

# RefCount
- single thread, mutiple pointers tracker. 
```rust
//example many treasure map has a reference to a treasure 
struct TreasureMap<'a> {  // 'a is needed 
    treasure: &'a Treasure,
}
//WHICH LEADS TO LIFETIMES EVERYWHERE, and SUCKS 
struct SpacePirate<'a> {
    treasure_maps: Vec<TreasureMap<'a>>,
}

// DO this instead 
struct TreasureMap {
    treasure: Rc<Treasure>,
}

main(){
let booty = Rc::new(Treasure { dubloons: 1000 });
let my_map = TreasureMap::new(booty);
let your_map = my_map.clone(); //your map is a pointer to my_map 
}

```
- Arc is Rc but multithreads. 
![[Pasted image 20220913202502.png]]
see: [[concurrency]]
# Associated type and generics
```rust 
impl Iterator for Counter { type Item = u32; fn next(&mut self) -> Option<Self::Item> { // --snip--
```
-> Item is a concrete type (u32), only implement traits on u32  
- if use generics -> traits can be implemented on other types (i32,string,...) per struct. 
- but associated -> only implement trait for 1 type per struct. 
```rust
pub trait Iterator<T> { fn next(&mut self) -> Option<T>; }
impl Iterator for Counter 
counter.next::<T>() //must specify T here if use generics 
```
# Newtype pattern 

- Some types are not known before runtime: making a programming language
-> evaluate its expression is an example. 
Expressions can be binary, unary, or literals consists of floats, integers,boolean, we don't know until the program runs 
#### Therefore, advanced type handling is needed 

## Newtype pattern
```Rust 
struct Wrapper(Vec!<i32>); //use tuple struct to wrap the type  
```
- for example implement Display to Vec! which is outside of crate, due to orphan rule, wrap it -> wrap it -> implements Display on Wrapper 
- cons: lacks Vec methods on Wrapper 
## Type alias 
- shorcut 
```Rust
type Thunk = Box<dyn Fn() + Send + 'static>;
```
 ## dynamic dispatch
 ```Rust 
 trait SomeTrait{}
 impl SomeTrait for i32{};
 impl SomeTrait for f64{};
fn()->Box<dyn SomeTrait>{} //means it can return i32 or f64
fn()-> impl SomeTrait{}// means it return i32 xor 64, depends on first one got inferred.  


```

[Advanced Types - The Rust Programming Language](https://doc.rust-lang.org/book/ch19-04-advanced-types.html)


# Rust async
[Crust of Rust: async/await - YouTube](https://www.youtube.com/watch?v=ThjvMReOXYM)
https://www.youtube.com/watch?v=NNwK5ZPAJCk&list=WL&index=104&t=38s
reference: [[Event Demultiplexer]]
```rust 
fn sads()->Future{
async {  }
}
// syntax sugar
fn sads2()->Future{
return Future
}


#[tokio::main]
async main(){} 
// syntax sugar
main(){runtime.block_on...} 


pin<box<>> // pin  means the pointer inside it CANT MOVE
// because future cant be moved during executing -> pin it

//return when a single computation complete
//if all is dropped, return a tokio:Cancelled 
select!{ //or race!
result<-fut1.await{//run if resolve }
result2<-fut2.await{}
}  
select
// SELECT is a bit special though. futures can be interrupted midway, unlinke inside async{sth.await} block
// => it pause the whole block, but select! switch branch instead of pausin 
// partial copy problem  

// another operation other than select, 
// wait till all future complete
join   

fn async(){ 
let a = some_async();
let b = some_async();  

//sequentially, unlike js 
a.await; 
b.await; 

// in js things are parallel when function (some_async()) 
// is trigger (event loop -> pass to executor, blahblah)

// in rust, await trigger (executor poll & pull) 
future::join(a,b) // to get same js behavior parallelism

}


//self made event loop! 
//keep repeating and resolving the event
loop{
select!{
result<-fut1.await{}
result2<-fut2.await{}
}
}
```
### why async example
```rust 
block_on  //block thread & wait for data (data must be ready)
.await  //attempt to get data but doesnt block thread  
//block => back to main thread

futures::join!(f1, f2); //multiple .await. 
//keep resolving each Future concurrently til both ready
//or both blocked -> proceed main thread
```
```rust 
EXAMPLE 
async fn learn_and_sing() {
//song and sing_song runs in order (relatively in this function), but (outside) they doesn't block thread.
//so it can be run asyncly with dance() below
    let song = learn_song().await;
    sing_song(song).await;
}
async fn dance();

async fn async_main() {

    let f1 = learn_and_sing();
    let f2 = dance();  

    futures::join!(f1, f2); 
}

fn main() {
    block_on(async_main());
} 
```

### tokio
I/O bound, hence => computational heavy, use rayon  
- implement Future, select and stream [Async in depth | Tokio - An asynchronous Rust runtime](https://tokio.rs/tokio/tutorial/async) 
- relate to multihreading (of link above)

async = cooperative schedule in one thread 
concurrency = cooperative scheduled in diff threads 
parallelism = parallelism

yield, not block thread
- pull based instead of push based: 
	- in js, event loop poll main stack (executor), and push (maybe a callback) to main stack if main stack free
	- in Rust main stack (executor) poll, and pull if  Future ready  

### implement Future  (for fun)
```rust 
// for fun  
// have initial future with some to be polled data
// build a thread that sleep, after certain time, return the data, and mark Future as Ready

```

### executor  
- executor hold a queue of Futures
- executor poll Future to check if it's done
if done return value inside  
else DON'T POLL that future again unless waken up (by the waker)
(still stay in queue thought just dont poll)
-> if future pending and no way to waken, it stay there (queue) FOREVER
-> between next future in a queue, and a previously pending future that is waken, depending on executor to pick which one first (FIFO or FILO,...)
![[Pasted image 20231125195816.png]]
# generator
[[coroutine, generators, thread]] 
![[Pasted image 20231125194550.png]]
- save states of each suspend (yield)
![[Pasted image 20231125194538.png]]









# Rust Essence
--- 
# Refererences 




2022 11 29 22:25
#literature  [[Atomic Ideas/Rust]] [[low level]] 

