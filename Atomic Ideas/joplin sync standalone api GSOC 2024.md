# Worklog    
### 17/5-10/6
==================== REPORT BONDING ================
- how to jest with typescript and import module  ? 
- why use shim clearInterval/timeout stuffs?   (timers module, now part of Nodejs) 
============================== 
17/5/2024
- iterated 1st time Synchronizer.uploadItem() 
- omit what is needed   
- FileSystem sync target
18/5/2024   
- Iterated 2nd time up to item uploader part (stuck due  to access db needed) 
- omit lock (can be implement easily later) 
20/5/2024  
- First item push to sync target   
- Added lock
21/5/2024 
- uploaded item from library pulled  successfully
- uuid generation  
- added memory sync target 
- uuid gen during upload & response  
22/5/2024 
- report upload items (cleanup processes) 
- implemented first mail client  

======================= WORK PERIOD ====================
- client id for lib? apptype needed ?  (current desktop) ?     
-> user input or generate new
================================================
4/6
- init sync info v3 (migration handler) for testing only 
- fix memory sync target, disable multiput, to skip itemUploader.preuploadItem, for testing 
.../6  
- GET Items method planning  
10/6 
- GET items metadata since a timestamp with basicDelta 
- GET single item (wrapper of GET apiCall) 
- docs get items case to joplin forum 
- 
### 11/6-14/7
11/6 
- planned attachments 
12/6 
- first iteration of upload attachments 
- brainstorming 2 problems: blob sync time check (optimize performance) && resource status 
16/6: 
- added driver code for resource upload
- bug found: only blob file uploaded, no metadata file found for a resource  
22/6: 
- researched e2e flow and addressed place need supports  
26/6: 
- BaseItem.serializeForSync added which supports preupload and item upload encryption
-  download function (without comparing on locals) 
27/6 
- split download into 2: mass download items (like a helper) based on a given ids list (add GET api call to a concurrent task queue),  and apply delta to sqlite database (which compares and insert) 
- mass download items done
- issues: comparison need loading content in  synchronizer :911
- issue: mass download may be now less accurate (because of user comparison) 
- resource path encryption, if E2E mode is on 
- add verifySyncInfo(), which do sync info, app version compatability check before doing operations, also set E2E to on or off (based on remote)
- issue: what if the ppk changes on remote, and local rely on that, but the encryption from local is not that key -> wrong password to decrypt 

2/7
- lock unit test 
- CRUD test 
- update item method: return newItem (for next sync), oldItem( for backup), remoteItem in conflict (for resolving)
- OCR service work flow  (plan)
- pull item metadatas  unit test
- time unified: create, comparision


```

# flow
to setup E2E:  
- setup e2e on 1 client (password, configs,...)
- submit that configs to remote 
- sync data, which use the configured e2e setup and encrypt all notes and upload it.
- have e2e check on all your endpoints (push and pull) 



 
``` 
8/7: 
- Found encryption bug: apply_encryptioned set to 0, but still triggers  (see E2E/index) , time bug when update (create works fine but bug in update)
- update bug fixed: no await, fix get bug: path/id only get once. 
- create now generate time, at same step when generate id
- OCR service WORKING!!!  
9/7: 
- attempting E2E but it's a mess, stuck on merge keys  
- create limited to 20 (explain this)
- delta output limit, sanitize options a bit  (report and explain  this)
- cleared up big chunk of commenteds and code
10/7: 
- input output  
- E2E notify flow
# Now   

# Wishlist     
what important now? docs, harden
medium clean up, unit tests + deep questions  -> finish

#### queued unit tests 
- delta test with middle timestamp
### medium cleanup
- update: bug time: show invalid date
- does read need lock? joplin lock page (create has lock) 
- error handling: where errors go when thrown (Roman mentioned)
#### lowered 
- joplin server 
- apply delta to sql lite helper 
- 1,2 more sync target

# deep question   
- cancel while pulling (this.cancel() download queue, release lock,...)  
-  cancel mid delta operation (see conflicts)  
-> try cli download     
-> read this.cancel() code, which is triggered when user press cancel  (unmount) 
- try catch all 
# Finished product 
### core    
- CRUD items <DONE> 
	- mass reads items <DONE>
	- get items metadata on timestamp (for delta)<DONE> 
- resource 
	- Create Read Delete blobs  <DONE> 
	- Mass reads blobs 
	- Update blobs <DONE>
- e2e 
	- blob encryption 
	- items encryption <DONE>
	- verification and cases check <DONE>
- additional 
	- locks (needs check on expiry)
	- pagination (removed) <DONE>
	- Item models: currently working with plain js object + Joplin model, may need a unified typing
	- proper logging and error handling
	- Doc all aspects 
		- Each operations   <DONE>
		- E2E  
		- Deeper technical decisions (e2e, update,...)
		- Demos <DONE>
	- more sync target 
	-  locks   
		- implement locks <DONE> 
		- proper locks placement 
	- error handling
	- edge cases



# Encryption in joplin   

Password: encrypt E2EE data key and user private key (in ppk)
PPK: private public keypair, use to transfer secrets between users 
- sender: take receiver public (ppk) key to encrypt the (data) key, send the encrypted (data) key
- receiver:  take the encrypted (data) key, and decrypt it with my private (ppk) key
Master Key (data key): generated from EncryptionService (a random method), then encrypted by password, use to encrypt items, notes,...  

- joplin keep tracks a list of master keys, and decide which one is active based on timestamps.  
- private key is cipher text + encryption method
- in EncryptionService().encryption(mk) , if mk === '', then it gonna loads mk from activeMasterkeyId 



# Use cases  
https://discourse.joplinapp.org/t/sync-in-background/38097 (meh)
https://discourse.joplinapp.org/t/standalone-sync-api/36821/5  (mine is filesystem, s3 is not so related)
https://forum.obsidian.md/t/sync-api-way-to-access-syncd-data/25371 (good)

> Sync is low level and doesn't know about notes, folders, etc. just items identified by IDs


- Create new item sync target (without conflicting subsequent reads) 
> 	 Just run sync and get back a list of IDs they have been added or changed. 
- Update item in sync target (without conflicting write on server)
> 	 Just run sync and get back a list of IDs they have been added or changed.  
#### implementation 
```
input files  
check state -> started yet   
onProgress cb 
lastContext ?  
syncsteps omit  

init
		const syncTargetId = this.api().syncTargetId();

		this.syncTargetIsLocked_ = false;
		this.cancelling_ = false;

		const synchronizationId = time.unixMs().toString();

		const outputContext = { ...lastContext };

		this.progressReport_.startTime = time.unixMs();

event arch  ?  
index resource omit
share id omit 
item uploader ?  
sync info omit, check later 

sync target new, omit TODO: currently click sync first, omit later   
check can sync omit  
ppk omit now  

acquireLock
			syncLock = await this.lockHandler().acquireLock(LockType.Sync, this.lockClientType(), this.clientId_);

			// eslint-disable-next-line @typescript-eslint/no-explicit-any -- Old code before rule was applied
			this.lockHandler().startAutoLockRefresh(syncLock, (error: any) => {
				logger.warn('Could not refresh lock - cancelling sync. Error was:', error);
				this.syncTargetIsLocked_ = true;
				void this.cancel();
			});


UPLOAD  

check is cancelling  
filter out item need sync omit  
upload supports BaseItem class 
pull remote and check conflict  
-> remote empty? throw error cuz not sync or deleted 
-> remote available ? only valid updated_time get to push  
or if new ? -> push right away
local sync time? check if this is note specific 

handle base on branch action above 
-> ignore blob (resources) (very large chunk if local._type == RESOURCE) 
-> handle conflicts 
```

- Read from sync target (and compare changes in locals)  
> 	 And there must be a way to mark a local item as changed so that it will be pushed to the server

- Android: https://discourse.joplinapp.org/t/sync-in-background/38097 (very little affects)

- Enabling server side OCR 
	- Have a middle service for OCR (a separate server), which pull and push extracted text data into Joplin format see [joplin ocr](https://joplinapp.org/help/apps/ocr/)
	- Embedd OCR directly on server (open source sync target only)  

- Realtime data changes: webhook 

- Encryption:  same key if they access same sync target (E2E is on)
## Design
Core components: FileApi + Adapter + API, SyncTarget, Synchronizer, 
Database: use Joplin filesystem for now


## Questions   
- Encryption problem 
	-> after reading data, we need to decrypt, if another people use it, they will need to use same key 
- How joplin do revision, know which is synced (sync -> pause sync -> sync)  
  -> delta sync look into this  
  this is how data is tracked {"user_data":"","icon":"","master_key_id":"","is_shared":0,"encryption_cipher_text":"","user_updated_time":1709750683220,"user_created_time":1709750679287,"created_time":1709750679287,"title":"a"}

- What's differences from @joplin/lib sync
	- push directly to sync target (without conflicts or solveable conflicts of course) 
	- read-only from sync target  
	- guidance to solve conflicts when read

- How sync is used in CLi and desktop: 
	- both leverages joplin/lib in package.json 
	- but cli use syncing lib manually (checking target and stuffs,...)
	- while desktop use lib/command. and command trigger sync
- How joplin database should be integrated  
	- sync and database in same lib, so cli or desktop just trigger lib
	- considering without using a database model (use json)
	-> use joplin/lib at first 
    -> consider separate joplin/database with json 

- How to handle dependencies (related to database above) 
```
1. Copy sync logics (json, no model), then @joplib/lib use this to populate with model (@joplin/lib need a rewrite)
2. Copy sync logics + (probably database) + new features on top (breakdown) here (not maintainable) 
3. Use @joplin/sync use @joplin/lib as a dependency (yeah, doable but all apps need a rewrite)  


-> Currently 1 is most viable, at first, only use this for odd cases (push without pull or vice versa). If joplin need a full sync logics, use old @joplin/lib. In the long run, to unify 1 sync lib, @joplin/lib can use @joplin/sync and plug data model in (hydration). 

-> option 2 is similar to 1, but that will aim to replace @joplin/lib sync as fast as possible, it's like total same @joplin/lib sync + additional feature. 

```
- How revision is done for each item
 -> this is how data is tracked {"user_data":"","icon":"","master_key_id":"","is_shared":0,"encryption_cipher_text":"","user_updated_time":1709750683220,"user_created_time":1709750679287,"created_time":1709750679287,"title":"a"} 
 -> notice some nitpick comments in syncing (there're 2 updated_time fields)

- How to push without conflicts  
-> new items seems fine (return new ids) 
-> update ? check updated time

- How to pull without conflicts 
-> return in json, user handle themselves 
-> consider option to resolve (mark local items as changed)




# Joplin Synchronization 
### Architecture
in packages/lib 
```typescript   
# CORE 
class BaseSyncTarget // takes Synchronizer + FileAPI
class Synchronizer 
class FileAPI // take driver, determine items CRUD when sync
// also take Database from below 

class OneDriveDriver // take OneDriveAPI below 
class OneDriveAPI // low-level API 

=> Adapter patterns 

# Inputs 
class Setting{ 
	load(){ 
		// read a sqlite db and parse into setting 
	}
}

# Database

class Folder; class Note; class Tag...  // inherit from BaseItem
class BaseItem // use BaseModel
class BaseModel // use JoplinDatabase, like python Models  

class JoplinDatabase // use Database 
class Database // use sqlite3, and raw SQL here


# How to use sync module 
CLI => get synctarget from Settings, and call synctarget.sync()
lib => command.exec("sync"), instead of calling sync() directly 
-> Command pattern
```

### Process 
1. Outside sync  
- event manager singleton
- dispatch to React state (omittable) 
- ResourceService 
- SharedItems service
- itemUploader component (use file api)
- Encryption service


2. sync start
- 3 steps sync: upload remote, delete remote, and delta
- sync only starts when state === idle (not in middle sync)  
state = idle | in_progress
- config = { 
  onProgressCb, # function to track progress 
  context # last sync context  
  syncSteps # dont know, replace above 3 steps of sync

} 
- use syncId (WATERMARKKKKKK) = Date.now();  
- sync_items are stored in a table, with a sync_disabled col (set to true when cannot sync)  

- index resourceservice 
- update share ids before sync 
- item_uploader to use file_api (instead of embed code right into syncer)



3. 3 steps start 

localinfo & remoteinfo: has a timestamp 
- check target existed on remote (remoteSyncInfo.version === nulll? ) -> create version (currently sync version:3 )  
- check if current remote can be synced with current app sync version (3.0) 
+ 2 checks: min supported sync version for all apps, and current apps min version 
- setup keys (maybe not related to E2E) for local and remote infos 

**KEY STEP**  
- local & remote infos equal 
-> if not, merge them into newInfo, setup e2e 
-> acquire lock 
-> upload newInfo
-> save  newInfo to local 
-> release lock 
-> resetup e2e 


- toUploads = BaseItem.itemsThatNeedSync(syncTargetId); // each Item has a updated time field  
- toUploads contains a neverSyncItems list  (items not existed on remote)
  
-> 1. first time sync this item 
-> 2. a conflict: remote deleted but local modified (logic to check: local has a sync time, but not existed on remote) 
 (delete conflict)

- proceed to check remote & local WATERMARKKKKKK 
-> not fit === conflict (conflict type update)
-> fit === update remote  
-> (start sycning by categorizing resource and actions and stuffs)  
for e.g: CREATE REMOTE, UPDATE REMOTE, or UPDATE RESOURCE,... 

- save watermark  
- handle conflict 

**NOTES**: have very special time logics here  (WATERMARKKKKKK) to track

- acquire lock   
(have a dedicated table to stored deleted items)  
- delete remote item based on path
-> get all deleted local items, take path of each  
-> call fileapi delete that path


 STEP 3: delta (apply locally)
 
4. Others 
- lock target when sync (this.isSyncTargetLocked)
- cancelling state (this.isCancelling) 















# joplin sync standalone api GSOC 2024
--- 
# Refererences 




2024 05 15 22:39
#literature [[computer science]][[low level]][[OSS]]
