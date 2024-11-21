# Supabase usage
How I use supabase for very basic things, in a simplest way, tech stack: React + Nodejs

### Database
- Setup database through GUI in supabase is fine
- SQL editor, can be used for tinkering with database query
- Supabase's SQL function supports is my favorite feature. Basically, we define a SQL function (a bunch of SQL statements), and invoke the function name only 
=> No worry for ORM, writing raw SQL,... 
![[Pasted image 20240123215040.png]]
- About RLS, I can't insert into table without turning off, maybe I'll check its configuration again.
- Creating Tables are kind of slow though, have to type in fields, would be great if can draw diagram, and it generate CREATE TABLE statements for me. 

 ![[Pasted image 20240123215122.png]]
- Just a visualizer, cant edit on mouse click  


### Authentication
- My goal is to use supabase to login with OAuth in frontend, then use that in my Nodejs server as well for Authentication, and database ( link the supabase auth user object to my Postgres User table ) 
```js
// Follow docs for initialization, 
// we get a supabase object
// In frontend, use these 
supabase.auth.signInWithOAuth()
supabase.auth.signOut()

// After login, we can use 
supabase.auth.getSession()).data.session.access_token;
// to get access token, right after logged in in frontend


```
- in server-side, pass that access_token (prefereably through header) for auth:
```js

const user = await supabase.auth.getUser(access_token) 
// this way we can check Auth, depend on whether user is null or not 
```

- usually, we need to link this to User Table in our database, and return that user object
```js
const { data } = await  supabase.from("User").select().eq("email", user.email);
// email is unique
// we can attempt to create user row if not available... 
``` 


 










# supabase usage blog
--- 
# Refererences 




2024 01 23 21:50
#literature [[blog]] [[nodejs]] [[backend]] [[system design]][[database]]