- very simple project structure
[ferium/src at main · gorilla-devs/ferium · GitHub](https://github.com/gorilla-devs/ferium/tree/main/src)  
- src/cli.rs    -> cli stuffs 
	  subcommands -> subcommands 
	  download.rs  -> helpers 

- dependencies: 
	- libium: backend, provide types and functions correspond to cli subcommands 
	- furse,modrinth: api wrapper for 2 websites
# Overview 
install with cargo 
Install mods, modpacks from Curseforge, Modrinth, Github
Multithreaded 
Configurable profiles for mods
- online modpacks have auto profile 

- ferium upgrade install all mods to ./minecraft (or other custom path)
- ~/.config/ferium/config.json

# Code
```rust 
// main.rs
fn main(){
pack runtime (tokio)
spawn a thread from tokio 
thread_on(actual_main)
} 

use cli::Ferium
use subcommands

fn actual_main(app:Ferium){   
app.parseSubcommands 

match app.subcommands{
Add =>  ...
List => ...
	for mod in profile.mods 
		match mod 
			curseforge => subcommands::curseforge(mod)
			modrinth => ...
}

} 
//cli.rs 
struct Ferium 
struct Subcommands
// subcommands/list.rs 
fn curseforge(mod){   
println(mod) // format and stuffs
} 


```
- libium contains struct for Profile, Mod  (backend of ferium) 
- furse and ferinth are curseforge and modrinth API wrappers

# Issue  
### alphabetical sort
[Show verbose listing of mods in alphabetical order · Issue #315 · gorilla-devs/ferium · GitHub](https://github.com/gorilla-devs/ferium/issues/315)
```rust   
ferium list // sort 
ferium list -v //dont sort  

ferium add //has sorting code at the end  
//hence profile.mods is sorted 

//but  
ferium list -v //use spawn threads 
```
### install from zip
[Install a modpack from a zip file · Issue #179 · gorilla-devs/ferium · GitHub](https://github.com/gorilla-devs/ferium/issues/179) 
```rust  
ferium add id 
ferium upgrade 

# add and upgrade flow 
add gonna push mod into the profile  
upgrade then check for to_downloads and to_install mods 

to_install mods are mods inside the minecraftmod folder that has a .jar
to_download mods are mods inside the profile list 

these 2 are parsed into 2 lists consist of Downloadble. 
and passed to fn download()

download() do both install and download. 
- install: tokio copy into path 
- download: downloadable has a download fn

then the 2 lists are passed into the download(,...) 

furse wrap the API call to the sites,
//for e.g furse.get_mod_files(id) gonna return all mod files 
// fetched from the curseforge based on that mod_id

MODPACK  

same above, but modpack info add to config instead of profile
(have a look at profile and config struct) 

// install flow  
ferium modpack add => add modpack to config 
ferium modpack upgrade => upgrade receive config.active_modpack   
fn upgrade(): 
	match active_modpack.identifer: 
		curseforge: 
			modpack_file = download_curseforge_modpack()
			manifest = extract_manifest(modpack_file)
			files = get_files(manifest.file_ids)
			to_download = files.into<Downloadable>   
			to_install = read_overrides(manifest.overrides)   
			//to_install are new mod configs, that are about to overrides old mod config 
			//cuz new mod config are stored in tmp   
			//when new mod installed, whether it overrides current user config or not is chosen by user 
			
			download(to_download) 

			//notes: most of modpack files are listed in to_download. the to_install is just some weird mechanics, of overriding. inside /.config/tmp/manifest/overrides..., there are mod files, these are moved into main minecraft mods through download()

			




QUESTIONS 
//[ferium/upgrade.rs at main · khuongduy354/ferium · GitHub](https://github.com/khuongduy354/ferium/blob/main/src/subcommands/upgrade.rs#L59)
pub async fn get_platform_downloadables( // need to pass in 
// Furse instance, why? because it has an api key 
)

2. explore that arc and mutex inside get download_able mods  
```
[Install a modpack from a zip file · Issue #179 · gorilla-devs/ferium · GitHub](https://github.com/gorilla-devs/ferium/issues/179)
- curseforge zip file only   
- it has no project id of whole modpack inside manifest  
- only contain projectId to mods that required   

1. use manifest.name to search for curseforge, use the top result id, download and install it 
2. download all mods using their projectId in manifest.files which would be easier if we have this  
[Add multiple mods at once #175](https://github.com/gorilla-devs/ferium/issues/175) 



# Learned 
- CLI apps should have structure aligned with its commands, subcommands  
- frontend (TUI), backend (complex tasks) which invoke API , even API calls (wrappers)
- leverage threading and async   
- callback for a download function, to update progress :)  
# Ferium rust
--- 
# Refererences 




2023 05 29 19:30
#literature [[Atomic Ideas/Rust]] [[OSS]] 