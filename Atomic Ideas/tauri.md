# Tauri usage 
```rust  
ca create-tauri-app    // run this to scaffold   

//generated
src-tauri 
	ca.toml
	.rs files  
src 
	frontend 

ca tauri dev    
// same as
npm run tauri dev 


docs:
- own error type 
- rust async issue  
- features: 
	- window 
	- multiwindow 
	- trays 
	- custom events (from front to back)
	- state, window events like click, apphandle (backend) 
// BASICS =============================
struct Database;

#[derive(serde::Serialize)]
struct CustomResponse {
  message: String,
  other_val: usize,
}

async fn some_other_function() -> Option<String> {
  Some("response".into())
}

#[tauri::command]
async fn my_custom_command(
  window: tauri::Window,
  number: usize,
  database: tauri::State<'_, Database>,
) -> Result<CustomResponse, String> {
  println!("Called from {}", window.label());
  let result: Option<String> = some_other_function().await;
  if let Some(message) = result {
    Ok(CustomResponse {
      message,
      other_val: 42 + number,
    })
  } else {
    Err("No result".into())
  }
}

fn main() {
  tauri::Builder::default()
    .manage(Database {}) //default state
    .invoke_handler(tauri::generate_handler![my_custom_command])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}
//js 
invoke('my_custom_command', {
  number: 42,
})
  .then((res) =>
    console.log(`Message: ${res.message}, Other Val: ${res.other_val}`)
  )
  .catch((e) => console.error(e))


// EVENTS    ===============================
// 1. Global 
//js
import { emit, listen } from '@tauri-apps/api/event'

const unlisten = await listen('click', (event) => {})
emit('click', {
  theMessage: 'Tauri is awesome!',
}) 
//rs  
fn main() {
  tauri::Builder::default()
    .setup(|app| {
      let id = app.listen_global("event-name", |event| {
        println!("got event-name with payload {:?}", event.payload());
      });
      app.unlisten(id);

      app.emit_all("event-name",  "sstring".into()).unwrap();
      Ok(())
    })
    .run(tauri::generate_context!())
    .expect("failed to run app");
}  



//2. WINDOW specific 
use tauri::{Manager, Window};
//js 
import { appWindow, WebviewWindow } from '@tauri-apps/api/window'
//current window which js is the client 
appWindow.emit('event', { message: 'Tauri is awesome!' })  

//to another window
const webview = new WebviewWindow('windowname')
webview.emit('event')  


//rs 
#[tauri::command]
fn init_process(window: Window) {
  std::thread::spawn(move || {
    loop {
      window.emit("event-name", payload).unwrap();
    }
  });
}

fn main() {
  tauri::Builder::default()
    .setup(|app| {
      let main_window = app.get_window("main").unwrap();

      let id = main_window.listen("event-name", |event| {
		let payload=event.payload();
      });
      main_window.unlisten(id);

      main_window.emit("event-name", payload ).unwrap();
      Ok(())
    })
}

```
# Tauri  contribute
[tauri/ARCHITECTURE.md at dev · tauri-apps/tauri · GitHub](https://github.com/tauri-apps/tauri/blob/dev/ARCHITECTURE.md)
[Tauri Plugins | Tauri Apps](https://tauri.app/v1/guides/features/plugin/)

- cli: for various stuffs (icon generation, dev, build,...)   [[tauri cli]]
- bundler create final .exe (platform specific) ,for end users 
-  API: (written in js, typescript) rust API wrapper 


- tauri-runtime: glue tauri & webview. 
-> tauri-runtime-wry = tauri-runtime +wry.
- tauri-utils: general tools to use across crates. 
codegen (parse configs -> Config, prepare assets (icons, images), make Context) 
- tauri-macros 
- tauri-codegen 
- tauri-build
-> macros (make marcros for stuffs above)
-> build (apply macros at build time)

- wry: Webview rendering lib, attached Tao [[tauri tao wry]]
- tao: windows creation lib 


# tauri
--- 
# Refererences 




2022 09 14 21:36
#literature [[OSS]] [[framework]] [[Atomic Ideas/Rust]]