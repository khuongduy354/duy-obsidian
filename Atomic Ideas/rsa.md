- create a pub key and a private key based on 2 large prime number 
- a key is a pair of number (e.g exponent=3, modulus =12)  
- pubkey is like the locker, used foir encryption 
- private key is the key, used for decryption  
-> we send lockers everywhere, but only the one 1 with private key (us) can unlock 
### why? 
-> useful for any services we haven't met  (dont need initial request)
- if we're able to make request to server,then 1 key is enough, use that for both encryption and decryption, client only received encrypted (hashed) data, and show it (basics of [[jwt]]) 
- however, by using RSA key pair, we can show the public key on site, client no need for initial request, just take they key, encrypt data and send to server 
-> no expiry problem, so no refresh,  encrypt on every request 









# rsa
--- 
# Refererences 




2024 01 24 09:45
#literature [[cryptography]] [[computer science]] [[Atomic Ideas/maths]] [[low level]]