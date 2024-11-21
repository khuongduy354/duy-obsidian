# Decision 
- string through rest api or file upload?
-> I think both, when create a file from react, string is easier to handle   
-> But if had .txt already just upload it.   
-> update on text, delete file and resave ?  
- use supabase cloud storage 
# Issue  
- DO NOT upload multipart data to backend then to Cloud storage (1gb to backend is heavy) 
-> instead get a presigned url of S3 through backend, then frontend upload directly to S3 cloud


# Sys design
- cache: LRU users, with most recent (paginated) files 
- search files frontend   
- pagination 

- folder hierarachy (not needed)





# LingoShelf
--- 
# Refererences 




2024 01 22 20:38 
#literature [[backend]]  [[Atomic Ideas/Nodejs|Nodejs]] [[database]] [[system design]]