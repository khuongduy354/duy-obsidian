# desktop package  
bridge.ts: idk 
electron_wrapper.ts: electron native
app.ts: frontend states
main.js: combines all above 
## How to build executables 
for desktop: in packages/app-desktop: yarn electronBuilder

### auto update   
app.start() run checkForUpdates.ts   
-> bridge.openMsgBox to for user to open download url

Goal:   
> Click download start download in background, download done -> next startup apply updates. 
- make  updateInBackground.ts  
- when app start, check for available update, or resume
- when done, replace current version with update (search Electron background update)  electron-builder and electron-updater
-> consider conflicts, backups 
-> consider if while updates user close


https://github.com/khuongduy354/electron-updater-example?tab=readme-ov-file 
```js
//update
const {autoUpdater} = require("electron-updater")
autoUpdater.checkForUpdates() //that's it  



//publish guide
npm run publish // with electron-builder -p flag
//config in package.json
// or .git implicit
// must have GH_TOKEN env when publish

- but joplin use a git push & tag for release and trigger build with CI, not electron-builder -p
```

# Joplin synchronization  
See [[joplin sync standalone api GSOC 2024]]



# joplin oss
--- 
# Refererences 




2024 03 01 15:03
#literature  [[low level]] [[computer science]] [[backend]] [[system design]]