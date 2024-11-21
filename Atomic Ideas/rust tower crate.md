- Service is a function map Request -> Response 
- a simple Services can wrap other services  
- abstract over protocol 
# hyper crates  
- a crate that implements http server (protocol stuffs) to support tower 
- hyper crate has an implementation of tower Service 
# Backpressure


- service wont take request if it is not ready (return error instead of response)
- has function poll_ready to make sure that it's ready. \
- 

# designing decisions 
- we want to take request -> make response -> log response -> return response
```rust 
    type Future = Ready<Result<Self::Response, Self::Error>>;

    fn call(&mut self, req: Request<Body>) -> Self::Future {
		let future_res  = Response::builder();
        info!(
            "finished building response: {:?}",
        );
        resp
    }

```
- but response is a future, hence log may go before response , 
- for things to run in order, await needed 
```rust 
resp.await 
info!() 
```
- but we need an async {} for await to be allowed 
- wrap things in async {} causing the fn to return wrong type 
```rust 
    type Future = BoxFuture<Ready<Result<Self::Response, Self::Error>>>;
fn call(){
Box::pin(async move{
resp.await 
info!()
})
}
```
-> this call a problem: making allocation on each request 

So what I was trying to do is to Write something  








# rust tower crate
--- 
# Refererences 
[Rust live coding - Tower deep dive - YouTube](https://www.youtube.com/watch?v=16sU1q8OeeI&t=25s)



2022 09 28 21:33
#literature [[Atomic Ideas/Rust]] [[OSS]] [[Rust Essence]]