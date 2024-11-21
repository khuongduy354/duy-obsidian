# Files, pipe and multiprocess
fork() 
pipe() 
execvp(const char * program, const char *args[])
• replaces current process with program 
• args[0] is the program file name
d. dup2(int newfd, int oldfd) – duplicates the oldfd by the newfd and closes
the oldfd
e. read(int fd, char *buff, int bufSize) – read from file (or pipe) 
f. write(int fd, char *buff, int buffSize) 


- generally, i takes input as stdin, output stdout, thesse are called file descriptors (or fd). 
- stdin, stdout, stderror = 0,1,2. Custom files open or pipes fd are > 2 
- multiprocess: use fork(). these run in parallel, in combine with exec family to run program from different sources 
- pipe: returns a pair of fd, 0 = read end, 1 = write end. Similar to stdin, and stdout, but these are assigned to terminal. 
-> to make pipe use stdin, stdout, use dup2 to bind it! because by default, pipe fd is a custom, so > 2






# C essence
--- 
# Refererences 




2024 01 23 09:42
#literature [[low level]] [[programming language]]