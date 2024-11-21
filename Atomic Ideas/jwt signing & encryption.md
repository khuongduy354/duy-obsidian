https://stackoverflow.com/questions/38588319/understanding-rsa-signing-for-jwt
https://stackoverflow.com/questions/60538047/jwt-private-public-key-confusion 
[[rsa]]

### signing
- signing is different from encrypting  
- signing can be done with 1 key or keypair 
-> 1 key: 1 server, just request that server for key, and show signed token every request 
-> key pair: useful for microservices, we have a centralized public key, other services can use public key for sign (1 private key store on many microservices is insecured)  

### encryption
- for encrypt, either [[rsa]] or any thing with 1 key is fine  
- we don't usually need to encrypt jwts, since it should store id only (non-sensitive)
-> check sign is enough for auth
![[Pasted image 20240124095352.png]]




# jwt signing & encryption
--- 
# Refererences 




2024 01 24 09:44
#literature  [[jwt]] [[rsa]] [[cryptography]] [[Atomic Ideas/maths]] [[computer science]] [[low level]] [[jwt]]