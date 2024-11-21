# How electron works ? 
node main.js entry 
- A main process runs with Nodejs
- electron module includes all natives stuffs (Windows, Mac, Linux) 
- another process called renderer which renders html with Chromium 
```js
const mainWindow = new BrowserWindow();
mainWindow.loadFile("index.html") 
//attach the index.html to mainWindow objects 
// which is then handle by the renderer process 
``` 
- electron doesnt allow accessing native module (electron module) from renderer, (so does [[tauri]]) 
### Main-Renderer Communication patterns 
- preload scripts: only runs be4 renderer (cant be runtime) 
- IPC: typicall runtime, need definitions for safe access 
- remote module: [[joplin oss]] does this, we can think of it as direct bridge  to electron module from renderer process 
### frontend framework 
- electron render html  
- so any web framework (react.js) -> bundled (webpack) into a html, can be used with electron 
- **NOTES**: react.js code are then bundled in js and run in Chromium (renderer process), so something like import "electron" won't work in React, use communication patterns above









# electron.js
--- 
# Refererences 




2024 03 04 15:44
#literature  [[desktop]][[framework]] [[computer science]] [[low level]]