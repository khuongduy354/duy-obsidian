## industrial
- Qt: too heavy framework, everything must use Qt, even IDE, stupid standard -> negative work experience. 
-> Qt is widely used in OSS 
- Gtk: Lighter Qt, but written in C 
-> also widely used OSS   
- Gtkmm: C++ but not in OSS  
 
## the lightweight
- ImGui: lightweight, not in OSS 
- WxWidget: lightweight but not use in OSS 
- Fltk: toplevel callback, ignore this 

-> Qt, Gtk, WxWidget, Fltk: same idea: 
```
1. make a class inherits some sort of widget
2. add items to it 
3. event modify functions/variable of that class or globally


// however 
Qt has a model view approach, which seperates logic and view  
but it depends too much on the Qt lib
```
-> which was hard to seperate    
- Imgui a bit different 
`still code logic in UI but less dependable `

## the werid path
- Wasm + Emscripten
- Rust tauri  
- cli
# Conclude
### oss 
- RUST for OSS: GUI, Cli 
### something that works (grow): 
- Imgui, : very fast to learn
- Cli : business logic seperation  
- Wasm + Empscripten: fun 
- Different frontend backend: anki, laplace for example






### notes 
Overall, [[GUI]] in low level sucks. Use HTML CSS, React. QML. ImGui, or even Godot. Which has a frontend of some kinds, and map that to backend, in any language.  








# comparing gui
--- 
# Refererences 




2023 02 28 20:12
#ideas [[Atomic Ideas/C++]] [[Atomic Ideas/Rust]] [[low level]] 