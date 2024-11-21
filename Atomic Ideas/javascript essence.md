# Vanilla JS frontend    
ref: https://dev.to/vijaypushkin/dead-simple-state-management-in-vanilla-javascript-24p0
```js   
class App {
constructor(countState = 1) {
this.state = {
countState,
setCountState: (val) => {
this.state.countState = val;
updateTree();
},
};

  

let initialHTML = `
<h1>Home page</h1>
<p id="count">${this.state.countState}</p>
<button id="button">Increase</button>
<button id="reset">Reset</button>

`;

}

  

render() {

return `

<h1>Home page</h1>

<p id="count">${this.state.countState}</p>

<button id="button">Increase</button>

<button id="reset">Reset</button>

`;

}

setup_event_handler(doc) {

doc.getElementById('button').addEventListener('click', () => {

this.state.setCountState(this.state.countState + 1);

});

doc.getElementById('reset').addEventListener('click', () => {

this.state.setCountState(0);

});

}

}

  

let app = new App(1);
updateTree();

// helpers

function gbid(val) {
return document.getElementById(val);
}

function updateTree() {
gbid('app').innerHTML = app.render();
app.setup_event_handler(document);
}

```
# Promise
```javascript
let promise = new Promise(function(resolve, reject) { 
resolve("done");
reject(new Error("…")); // ignored 
}
//run when got a result 
promise.then(fulfilledFunc, rejectedFunc) 



//error only 
promse.catch(rejectedFunc) 
//same as 
promise.then(null, rejectedFunc )
```
- Promise produce some **result** in the **future** 
-> **pending** state 
- when run resolve/reject 
-> **resolve**/**reject** state 
- `then()` can be used to **consume** the result





# asyncHandler to avoid trycatch

```javascript
const asyncHandler = fn => (req, res, next) =>
  Promise
    .resolve(fn(req, res, next))
    .catch(next)

app.get('/hello', asyncHandler( (req, res, next) => {
}) );
```



--- 
# Refererences 
[Full Javascript](https://javascriptinfo.com/)
[Tips and tricks Javascript || Anonystick]
[advanced-js-resources.md · GitHub](https://gist.github.com/amysimmons/1d3c6fa84800b50d6b515662b55d0cb9)
[33 js concepts](https://github.com/leonardomso/33-js-concepts) 

#literature  [[javascript]] [[programming language]]