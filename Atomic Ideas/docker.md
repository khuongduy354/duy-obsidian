Docker
# Docker 
https://viblo.asia/p/dockerize-ung-dung-nodejs-RnB5pxEG5PG
# Hello World flow   
1. Create a Dockerfile + Dockerignore
2. Build  
3. Push (optional)
**notes**: multiple services = multiple containers + compose
### basic setup 
```d
**Dockerfile 

FROM node:latest //from prebuilt image, see others node:13-alpine for ex
WORKDIR ./app   //mkdir app && cd app
COPY package*.json ./  // copy dep first for docker to cache
RUN   npm install && npm build  //run when build  

COPY . .        //copy everything to ./app

ENV PORT = 8000 //process.env.PORT in node
EXPOSE 8000 //inner container port

CMD ["npm","start"] //run when run container, only 1 allow
// use .sh or Makefile or EXECPOINT for multiple commands



**Multiple CMD** 

ENTRYPOINT ["sh", "/var/www/html/.docker/docker-entrypoint.sh"]

CMD supervisord -n -c /etc/supervisord.conf

**Build**
docker build -t learning-docker/node:v1 . 
docker images // show images
docker run // run image on machine 
docker pull  
docker push 


```
![5e953b543eab348a6a554b08c1c188e9.png](:/6c4f32dd26cd4e29afa1e91e35af605d)
# Explain
- expose and mapping port diff: https://viblo.asia/p/dockerize-ung-dung-laravel-vyDZOao75wj
- dockerignore 
- cache
    - each step docker will attempt to cache if nothing changes,
    - therefore, we should install dependencies first, then copy app,
    - this help us when we change code but not dependencies, dep wont need to be reinstalled
### Compose (more detail than docker run)
  ```jsx
- Run many containers at the same time (1 process for 1 container)
COMPOSE
Compose
docker-compose  //better than docker-run 

//make a file docker-compose.yml (in same with Dockerfile) 
content:
	version: "3.7" //docker version
	
	services:  //make a service named app
	  app:
	    image: learning-docker/node:v1
			ports: - "80:80" // machine port : docker port
	    restart: unless-stopped  
    environment: //env instead of in Dockerfile which need rebuild
      PORT: 6666
		depends_on: //use for multiple containers wait 
			- redis 
			- db



docker-compose up    // run container
docker-compose down  //stop running container

docker-compose exec app sh //get into docker world (linux os)
cat /etc/os-release //see docker running os 
  
```
    

```jsx
COMPOSE
**Compose**
docker-compose  //better than docker-run 

//make a file docker-compose.yml (in same with Dockerfile) 
**content:**
	version: "3.7" //docker version
	
	services:  //make a service named app
	  app:
	    image: learning-docker/node:v1
			ports: - "5000:80" // machine port : docker port
	    restart: unless-stopped  
    environment: //env instead of in Dockerfile which need rebuild
      PORT: 6666
		depends_on: //use for multiple containers wait 
			- redis 
			- db

docker-compose up    // run container
docker-compose down  //stop running container

docker-compose exec app sh //get into docker world (linux os)
cat /etc/os-release //see docker running os 


# ================ HOW IT COMPOSE 
![2742db72aadee4bfd409cacc1876d4db.png](../../../_resources/2742db72aadee4bfd409cacc1876d4db.png)
```

- load from .env file
    
    ```jsx
    //.env
    PORT=8888
    PUBLIC_PORT=9999
    
    //docker.compose
    version: "3.4"
    
    services:
      app:
        image: learning-docker/python:v1
        ports:
          - "${PUBLIC_PORT}:${PORT}"
        restart: unless-stopped
        environment:
          PORT: ${PORT}
    ```
    
- Other ways to use copy
    
    ```docker
    COPY app.js .  # Copy app.js ở folder hiện tại vào đường dẫn ta set ở WORKDIR trong Image
    COPY app.js /abc/app.js   ## Kết quả tương tự, ở đây ta nói rõ ràng hơn (nếu ta muốn copy tới một chỗ nào khác không phải WORKDIR)
        
    # Copy nhiều file
    COPY app.js package.json package-lock.json .
    # Ở trên ta các bạn có thể copy bao nhiêu file cũng được, phần tử cuối cùng (dấu "chấm") là đích ta muốn copy tới trong Image
    
    ADD cũng làm được điều tương tự nhưng nó có thêm 2 chức năng đó là:
    
    ta có thể copy từ một địa chỉ URL vào trong Image
    ta cũng có thể giải nén một file và copy vào trong Image
    ```
    
- Porting
    1. **Mapping ports** 
    
    > Container port and OS port are diff
    > 
    - Map outside (container) to inside port
    
    Chú ý **bên trái** là môi trường gốc (bên ngoài), bên phải là cổng của container. Ở đây cổng ở môi trường gốc ta có thể chọn tuỳ ý, nhưng cổng của container thì phải là 3000, project NodeJS của chúng ta chạy trong container ở cổng 3000. Do đó các bạn có thể thay đổi như: **"3001:3000" hay "5000:3000"**, tuỳ ý nhé, nhưng thường mình sẽ để cổng giống nhau luôn.
    
    ```docker
    ports:
          - "3000:3000"
    ```
    
    1. **Port expose** 
    - Allow containers to communicate through a specific port
    
    ```docker
    EXPOSE 9000
    ```
### Volumes
```docker
##  2 ways mapping
    1. We shouldnt map everything 
    because outside doesnt have node_modules -> loss of node_modules in container 
    
    2. container might pollute host env with node_modules during runtime (but it's fine) 
    ```
    

### Debugging

### Pushing

1. **Gitlab**

→ Make gitlab account, with project 

```jsx
docker login registry.gitlab.com
// to push to gitlab, name format: registry.gitlab.com/<username>/<tên repo>
//to fit the format, 2 ways: 
1. rebuild image with fitted format name 
2. change image name

docker tag old-name new-name
//docker tag learning-docker/python:v1 registry.gitlab.com/khuongduy354/learning-docker
docker push image-name
```
explain:     ![f5447b77005735286b7e0e904a59205d.png](:/dd28a8254e6b42dda2c335b2d8e0580d)
### Use with nginx
```jsx
**//2 stages** 
# build stage
FROM node:13-alpine as build-stage
WORKDIR /app
COPY . .
RUN npm install  
RUN npm run build

# production stage
FROM nginx:1.17-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html   //takes only html (latest built) file -> help reduce source files
CMD ["nginx", "-g", "daemon off;"]  // start nginx 

//***Notes: this is a fully frontend project (Vue.js), so we use nginx, because vue cant 
//serve itself, in dev, there's a built-in webserver for that task but in prod we need 
//a better one

//**same as above except nginx port
**version: "3.7"

services:
  app:
    image: learning-docker/vue:v1
    ports:
      - "5000:80" //nginx default port is 80
    restart: unless-stopped

//intermidiate container (no stages needed) 
-> Container delete after finished its job**
docker run --rm -v $(pwd):/app -w /app node:13-alpine npm install && npm run build // explain below

//in compose
version: "3.4"

services:
  app:
    image: nginx:1.17-alpine
    volumes:
      - ./dist:/usr/share/nginx/html // outer : inner container ,mapping
    ports:
      - "5000:80"
    restart: unless-stopped
```
explain: ![118a67cd0c4df726dd3700f004043d82.png](:/9175e32296434e9fbb435c9fecc26214)












# docker
--- 
# Refererences 




2024 03 07 22:54
#literature  [[devops]] [[backend]] [[system design]] 