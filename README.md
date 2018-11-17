# pindown
Linux Security Module-PinDOWN

PinDOWN module consists of the four hooks listed below which are a part of the Linux Security Modules framework:
1)	inode_permission 
2)	bprm_set_security 
3)	task_alloc_security 
4)	task_free_security 

This project uses the above LSM hooks to enforce a simple access control policy. The primary access control decision is made in the inode_permission hook. If the inode does not have an XAttr for “security.pindown", the given file is accessed. If there is an XAttr for “security.pindown", the access is allowed only if the value string of the XAttr matches the program path string you stored when the binary program was loaded.

LSMs use the security field of the task struct to store security information for each process. Security is allocated in the task_alloc_security hook. The security is then deallocated in the task_free_security hook. This structure contains the pathname of the program which will be compared to the policy on the file. The current process's security values is copied into the child process in task_alloc_security and updated to the executed program in bprm_set_security.

The setfattr and getfattr system utilities can also be used to manage extended attributes. If using these utilities the pindown security namespace must be specified [-n security.pindown] along with the value [-v] for Pindown LSM to process and enforce the attribute. 
