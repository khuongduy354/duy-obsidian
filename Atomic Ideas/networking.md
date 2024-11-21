[Computer Network - Cheat Sheet - GeeksforGeeks](https://www.geeksforgeeks.org/computer-network-cheat-sheet/)
### localhost  
- on same machine, use localhost works 
- on different machines of same local network, must use private ip of this device 
- on different machines of diff network (internet), only public ip works (forward this  machine private ip to public)   
- localhost is same as 127... loopback  IP 
- loopback?  
>A loopback address has been built into the IP domain _system in order to allow for a device to send and receive its own data packets_.
``` 
machine A: localhost:3000 , 
machine B(LAN): A_PRIVATE_IP:3000 
machine C(WAN): A_ROUTER_PUBLIC_IP:3000 (assumed set up NAT forwarding (see below))
 
``` 

[[https://github.com/khuongduy354/obsidian-visualizer-vscode]]
```markdown
# Make sense of networking

### ground level
We need a bunch of computers, connected together to share stuffs. That's called a **network** of computer. **Internet** is the biggest network.

They can communicate far distance through **electromagnetic** **waves** (the higher wavelengths, the further) or **wires**

Some **devices** are used to generate the connection: hub, switch, router, modem,... 


### Internet 
Biggest network. To be a citizen of it. You need a IP address ( a public one).
It has a DNS.

### how data should be shared 
One way is, one PC ask, and the other provide, a client server (or simplex).  
Another way is, both connected (full or half duplex connection), and data shared 2 ways. 
This may dependent on protocols below

### protocols 
The way (standard) how computers talk through a network is called a **Protocol**

The **Protocols** are devided into categories, depend on abstracted it is from the physical level. There're 2 models to categorized protocols: **OSI** and **TCP/IP** models

You can get a flatten list of **protocols** in link above.




# Some nasty keywords 
client, server 
tunnel
ip address, mac address, dns 
vpn, proxy
```

# Devices
### Modem ( DSL )
> a device with 1 (public) IP adress 
### Router
> a device that split  public and  private IP address
-   The router has 1 public IP, while the machines connected shares the same private IP
-   Use for multiple devices, private IP adress cant be connected directly
-   Port forwading: 
NAT from private IP to public IP (internet): 192...:8080: PUBLIC_IP:8080
**diff**: 
-> *the modem connects your home to the Internet, while a router creates the network inside your house*. in today world, these are usually combined
# Protocols
### http  (app layer )
```python 
Set-Cookie: tracking=tI8rk7joMx44S2Uu85nSWc  #response
Cookie: tracking=tI8rk7joMx44S2Uu85nSWc #next request
Connection: Keep-Alive #should close TCP  or not  

# payload  
username=daf&password=foo #form
{"hi":"asdsa"} # json
Content-Type: application/x-www-form-urlencoded 
# or 
			  multipart/form-data
```
### https  (app layer)
> has SSL, handshake (connection header above)

### tcp (lower layer)

# Concepts
## IP address  
[[ip address]] [[bug bounty hacking journey]] 
IPV4: 255.255.000.000 
11111111.11111111.00000000.00000000 
32 bits (4bytes) in total,  so max of each 2^8-1 = 255
possibles combos: 2^32 addresses
## MAC address

## CORS 
- preflight http: http go first (automatically by browser) to check for CORS  through headers  
## DNS
→ A Domain Name System (DNS) translates a domain name such as [www.example.com](http://www.example.com/)  to an IP address.
- [routes in DNS](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1866b10a-1dd5-4f76-a4b7-7e1ae9e33977/Untitled.png)
### resolve (translate) DNS
→ recursively requests other servers
→ requests protocol can be TCP (encrypted TLS), or UDP,
see how protocols work in
## Rust implementation
    1.  Convert **domain name** → **IP address**
    2.  Input: **domain name**, **DNS server**
    3.  Prepare **query** (called **message**) from **domain name**
    4.  setup **UDP** connection, and send **request** to **DNS server**
    5.  get **response!**
    
    → How [UDP]([https://www.geeksforgeeks.org/udp-server-client-implementation-c/#:~:text=In UDP%2C the client does,data to the correct client.)](https://www.geeksforgeeks.org/udp-server-client-implementation-c/#:~:text=In%20UDP%2C%20the%20client%20does,data%20to%20the%20correct%20client.)) server client works
    
    → How **DNS server** [lookup](https://www.youtube.com/watch?v=2ZUxoi7YNgs) IP
    
    -   code (conceptual, may not work)
        
        ```rust
        fn main() -> std::io::Result<()> {
        //  input: dns_server_raw , domain_name_raw  
        
        //parse string to datatype represents domain name 
         let domain_name = Name::from_ascii(&domain_name_raw).unwrap(); ❸ 
         //empty req,res 
         let mut request_as_bytes: Vec<u8> = Vec::with_capacity(512); ❶
         let mut response_as_bytes: [u8; 512] = [0; 512]; ❷
        
        //query message for dns 
         let mut msg = Message::new(); ❹
         msg
         .set_id(rand::random::<u16>())
         .set_message_type(MessageType::Query) ❺
         .add_query(Query::query(domain_name, RecordType::A))
         .set_op_code(OpCode::Query)
         .set_recursion_desired(true); ❻
        
        // add message to request, encode it
         let mut encoder = BinEncoder::new(&mut request_as_bytes); ❼
         msg.emit(&mut encoder).unwrap(); ❼
        
        //setup UDP connections 
        // 0.0.0.0 means listen to all addresses, at random port (port chosen by OS)
        // in UDP, both must act as client and server, therefore sender need to bind to a port to receive back
         let localhost = UdpSocket::bind("0.0.0.0:0").expect("cannot bind to local 
        socket"); 
         let timeout = Duration::from_secs(3);
         localhost.set_read_timeout(Some(timeout)).unwrap();
         localhost.set_nonblocking(false).unwrap();
         let dns_server: SocketAddr = format!("{}:53", 
        dns_server_raw).parse().expect("invalid address");
        
        //send request to dns server, and get results
        localhost.send_to(&request_as_bytes, dns_server).expect("socket 
        misconfigured");
        localhost.recv_from(&mut response_as_bytes).expect("timeout 
        reached");
         let dns_message = Message::from_vec(&response_as_bytes).expect("unable to parse 
        response");
        
        //print result 
         for answer in dns_message.answers() {
         if answer.record_type() == RecordType::A {
         let resource = answer.rdata();
         let ip = resource.to_ip_addr().expect("invalid IP address received");
         println!("{}", ip.to_string());
         } 
         } 
        }
        }
        ```
        
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0faecc4-c5cb-4bd7-bdef-b159624d625f/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f64f0b5-13cc-4939-9011-55ba8f3f1715/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/476e4e4d-4fa6-45ed-9305-05d8f0438e19/Untitled.png)
    

### Cons

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ddcc0bd-812e-4fe8-bfcf-0a1f3dcb11ff/Untitled.png)

[](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197427(v=ws.10)?redirectedfrom=MSDN)[https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197427(v=ws.10)?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197427(v=ws.10)?redirectedfrom=MSDN)


> a physical address (unlike IP), cant be changed, mainly used for local networks

###  Rust implementation
    
    ```rust
    extern crate rand;
    use rand::RngCore;
    use std::fmt;
    use std::fmt::Display;
    #[derive(Debug)]
    struct MacAddress([u8; 6]); ❶
    impl Display for MacAddress {
     fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
     let octet = &self.0;
     write!(
     f,
     "{:02x}:{:02x}:{:02x}:{:02x}:{:02x}:{:02x}", ❷
     octet[0], octet[1], octet[2], octet[3], octet[4], octet[5] ❷
     ) 
     } 
    } 
    impl MacAddress {
     fn new() -> MacAddress {
     let mut octets: [u8; 6] = [0; 6];
     rand::thread_rng().fill_bytes(&mut octets);
     octets[0] |= 0b_0000_0011; ❸
     MacAddress { 0: octets }
     } 
     fn is_local(&self) -> bool {
     (self.0[0] & 0b_0000_0010) == 0b_0000_0010
     } 
     fn is_unicast(&self) -> bool {
     (self.0[0] & 0b_0000_0001) == 0b_0000_0001
     } 
    } 
    fn main() {
     let mac = MacAddress::new();
     assert!(mac.is_local());
     assert!(mac.is_unicast());
     println!("mac: {}", mac);
    }
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4cd989d0-b437-4f29-87f4-de5fbb9315c7/Untitled.png)
    

**Casting Types:**

-   Unicast
    
-   Multicast
    
-   Broadcast
    
-   explain
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb358807-3e70-4361-b1b2-30a4b1e4eaab/Untitled.png)
    

**Universal/local:**

-   _universally administered addresses →_ register by manufacturer
-   _locally administered addresses →_ create MAC address, without registration

### **Differs from IP:**

-   IP mostly used for Internet, MAC used for Ethernet
-   IP is used more globally, while MAC is more locally

→ In layer 2, computers talk to each other locally through **Switch,** uses **MAC** address,

but in layer 3, when talking with outside world, through **Router,** uses **IP** address.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f7c704d3-62d2-40c8-a728-5746864a6ce8/Untitled.png)

[](https://www.youtube.com/watch?v=N7dM_kD28dM)[https://www.youtube.com/watch?v=N7dM_kD28dM](https://www.youtube.com/watch?v=N7dM_kD28dM)

## CDN
### Push CDN: cdn updates when server change
### Pull CDN: cdn updates when client request













# networking
--- 
# Refererences 




2022 08 26 21:55
#literature  [[computer science]]