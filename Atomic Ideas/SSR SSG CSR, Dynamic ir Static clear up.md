# Tradition webs 
They generate a static site beforehand, then send to user, 
this is called SSG
# -> SSG is the traditional way of web -> static sites
- generate before runtime/ in buildtime/ before request in server 
- fucking fast 
- good SEO 

- but when updates, need rebuild, request to server
- repetitive components
### Incremental Static Regeneration: SSG that updates periodically. ISR

# CSR -> SPA 
- Run javascript in browser to generates HTML
- 1st load slow, then fast, interactive 
- fast navigation
- reusable  

- sucks SEO, web crawler dont execute js, hence no HTML for SEO 
# SSR -> Dynamic site
- generate on server per request  
- also called templating  
- no need rebuild, update on refresh/request to server
- still good SEO, cuz it can returns indexable HTML for web crawler 

- slower than CSR

![[Pasted image 20220728221418.png]]
#### SSR 
![[Pasted image 20220728221426.png]]
#### SSG 
![[Pasted image 20220728221439.png]]
# Comparison
![[Pasted image 20220728221851.png]]










# SSR SSG CSR, Dynamic ir Static clear up
--- 
# Refererences 
[Visual Explanation and Comparison of CSR, SSR, SSG and ISR - DEV Community](https://dev.to/pahanperera/visual-explanation-and-comparison-of-csr-ssr-ssg-and-isr-34ea)



2022 07 28 22:19
#literature   [[computer science]] [[web concept]]