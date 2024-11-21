
# Jobs Preps

### Juicy
- System design whiteboard
- Authentication flows
- DATABASE DESIGN (SQL) 
- Behavior  
- DSA  
- Strive inn unknown

## Language Ecosystem specific
### nodejs simple
1. Explain event loop 
3. Explain promise
Promise is a wrapper around some data, that is not yet available at the moment.
4. Nodejs advantages 
- Non-blocking design: good I/O
- Javascript fe can move to be 
Disavantages 
- it's not designed for computational heavy task originally, since it use Js which is a interpreting language
5. Streams in nodejs 
- objects allow to read or write data from one place to another, it's like piping
6. worker threads 
7. Clustering
8. Buffer: a class to store and interact binary data in Js  
### Worker Thread, Cluster Module of Nodejs

- Worker thread = multiple tasks same process (multithreading) ultilizing Event Loop for concurrency. 
- Cluster = many Nodejs instances different process (multiprocess)
- PM2 

### Event loop
- Flow:
Async function -> (push to) libuv (nodejs) or Web API (browser)
-> (done processing)  push to Event Queue / Callback Queue
->  Event loop run (endlessly) to poll check if tasks in Event Queue done and main stack empty. 
-> Take callback to main stack and run, else proceed checking next event in queue.

- Event loop (libuv) on nodejs is differs from browser ( Chrome's event loop )
- Handlers on Nodejs (OS worker threads pool) is differ from browser (Chrome's web workers)

### Multithreading

- number of workers = number of cores (16)
    -\> beware hyperthreading, which may have more cores (8 physical cores but 16 cores to work with)

NodeJS: js language is single thread. But it can multithread (in C/C++) then pass result to JS through callbacks. 
On Nodejs, libuv do multithread, on browser has Web Worker APIS

> Node is not single threaded, JavaScript is. The node application itself runs the V8 JavaScript engine, and is written in C++. All the system calls are themselves multithreaded, but the responses for these calls are queued up and come back into the JavaScript through the event queue, and results passed off to the various JavaScript callbacks waiting.


# Backend Nitty Gritty   + System design

##  [[database]]

## [[system design]]

## Cache + strategies(practice exponential)

https://github.com/ByteByteGoHq/system-design-101#cache
https://github.com/ByteByteGoHq/system-design-101#top-caching-strategies
[[Caching 101]]
## OSI model

## Terms & Patterns

https://github.com/ByteByteGoHq/system-design-101#mvc-mvp-mvvm-mvvm-c-and-viper
https://github.com/ByteByteGoHq/system-design-101#18-key-design-patterns-every-developer-should-know
https://github.com/ByteByteGoHq/system-design-101#a-nice-cheat-sheet-of-different-databases-in-cloud-services
https://github.com/ByteByteGoHq/system-design-101#8-data-structures-that-power-your-databases
- Threads and Process  [[Rust Essence#Multithread in rust]]
> Processes share same memory, threads don't
-  gRPC
> 5x faster JSON cuz encode & network optimization
   server to server, but complex prototypes hence not user for client server
- Webhook  
- distributed stuffs
# [[networking]]

# Database  
- why this and that 
- indexing
- N+1 problems
- ACID locks and stuffs
   


## Authentication

### Cookie

- Server set cookie if successfully auth.
- Session based: cookie store in client (local storage), got extracted when makin' a request,
- has expiry

### JWT 

- header payload signature
- grants user a JWT token if successfuly logged in (has expiry).
- Store user data in a token, that token is sent when making request
- Backend can decode that token, to extract and authorise user

### Password based

- Take incoming password -> hash, compare that to the hashed one in database

### OAuth 

- client request -> server redirect to OAuth
- client logged in successfuly through 3rd party page -> redirect to server (callback_url) with a auth token
- server use auth token request 3rd party for Access Token
- after Access Token granted, server can request again with that token for user data

### SSO

### SSH key

## Optimization
- Pagination
- Caching
- Rate Limiting (limit requests made) & Throttling (limit data transferred)
- Connection pool (for database)
    Database connection is costly for each request, so when a request done, instead of fully close, just temporary close it, and reuse it with less cost. There's a real close, after conditions met: expire time
- Payload compression
- Efficient DB query and DB storage (CDN for static assets)
    -\> Use monitoring tool to optimize most critical part

## Security
https://github.com/ByteByteGoHq/system-design-101#how-do-we-design-effective-and-safe-apis

# Questions & fun facts

- gRPC is much faster REST, why not replace REST
    -\> REST is way more simple, with gRPC, you must know who is requesting your server
- How about GraphQL
    -\> offload queries into frontend -> u dont know what query is gonna made -> dont know how to optimise db query
    -\> Use REST as standard and add others if necessary
- SQL ensure ACID: Transaction pass to Lock Manager ensure lock mode
    https://github.com/ByteByteGoHq/system-design-101#how-is-an-sql-statement-executed-in-the-database
- Cookies and Authorization header
    -\> cookie set autoly when Set-Cookie response header is set, and is send automatically if available in browser
    -\> Authorization must be explicitly set
    -\> Cookies is key-value, while session id is just a key.

**networking buzzs**

- What happens when enter a URL to a browser
    -\> Browser lookup IP from dns servers
    get IP -> establish TCP connection to server -> send request -> receive response
- HTTP, HTTPS, TCP/IP (protocol suite) is different from, TCP (a layer), IP (ip address)
    -\> The Internet Protocol Suite is the collection of protocols that implement the TCP/IP model:
    TCP, UDP, IP, ICMP, ARP, DHCP, DNS, HTTP, FTP, SMTP (these span in different layers)
    -\> TCP is a Network layer
    -\> Things not use TCP/IP: RF, Bluetooth (non-Internet things)

**security**

- How https use http and add security
    TCP handshake -> SSL/TLS handshake -> send request
    SSL/TLS because previously it's SSL.
    Website owner (server) holds a certificate (cert = public key)
    TLS 1.2 use both asymmetric and symmetric key
    TLS handshake flow:
    Client init hello -> server return cert (public key) -> client create another (premaster symmetric) key and use the cert (asymmetric key of server) to encrypt the client key -> send encrypted key to server -> server take and gen a master key -> client, server both have that symmetric key and now we can talk securely.
    https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/
    **Notes**: during first init hello and response, client gives algo options, and server pick algo.
    -\> Sometimes client need to send a cert, and that is called a MUTUAL TLS (mTLS)
    https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/
    
- why burp need cert
    -\> Client - Burp Proxy - Server, when https, client check server's cert, however since it receive from Burp.
    Hence Burp need a cert
    

- CSR, SSR, SSG, pros and cons 
- Why NoSQL over SQL 





# Behavior
Obvious questions
# Introduce 
- Name, age, school 
- Coding since high school 
- Notable projects: GSoC Joplin, VSCode Obsidian, backends...
- Build projects for sake of love building stuffs and learning 
- Relate to company: this company has XXX that aligns with my interest.... 

### After questions 
- salary stuffs
https://www.techinterviewhandbook.org/final-questions/









# BACKEND + SYSTEM DESIGN FULL PACK
--- 
# Refererences 




2024 05 21 22:44
#literature [[backend]] [[low level]] [[computer science]] [[system design]]