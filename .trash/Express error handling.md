[[express.js]] [[error handling]] [[middleware]]
2022 07 17 16:15
Tags: #cheatsheet 
# Express error handling
### Using middleware
```javascript 
//middleware error handler
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {});

```

- See [[next in middleware]] to pass error 
- See [[Express error handling#Using custom Error]] to handle database error 
### Using custom Error 
[my error handler](https://github.com/khuongduy354/MEN-radish/blob/master/src/error.ts) 

### Uncaught error 


--- 
# Refererences 
[Express error handling](https://expressjs.com/en/guide/error-handling.html)