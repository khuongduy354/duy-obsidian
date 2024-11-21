- usage: [CLI | Tauri Apps](https://tauri.app/v1/api/cli/)
- cli.rs contains logics for a cli. 
- cli.js is a wrapper for that  
# cli.js 
- write code in Rust with napi 
-> generate Nodejs code (called addons)  
#### overview of how it works 
root: [cli.js repo](https://github.com/tauri-apps/tauri/tree/dev/tooling/cli/node)
- in /src/lib.rs contains a run function. that run code from cli.rs 
 -> run func is converted to napi:
	- /npm contains all the platform which will convert from Rust to Js. (see napi docs) 
- after converting, index.js import that converted **run** function (now in JS).  
- main.js wrap **run** in error handling and promise. 
- tauri.js consumes it, takes commands from terminal and pass to it.  
# tauricon  (deprecated), now in cli.rs
> a cli to generate icons in js. 
- index.js to take cli arguments   
- help, log => display info 
- else has a path => generate icons
#### tauricon class for generating functions
1. tauricon.make(): validate -> icns -> build -> minify   
#### tauri.config 
1. the default settings options for all generated icons


# cli.rs 
- main.rs: take in arguments , get first one as bin-name 
-> pass arguments, bin-name to tauri_cli::run() (from lib)    
- lib.rs: 
	- clap has a mechanism to pass in args and create a Cli instance defined by  Cli struct. 
	-> create one cli 
	- matches cli.command, and handle each command.
	-> each command is handled in different modules (icon.rs, build.rs,...) 
- icon.rs: 
- 







# tauri cli
--- 
# Refererences 




2022 09 14 21:34
#literature [[Atomic Ideas/Rust]] [[OSS]] [[Atomic Ideas/Nodejs]] [[tauri]]