### **What is Privesc?**
Privilege Escalation usually refers to going from a low level permission to a higher permission. It means exploiting of a vulnerability or a design/configuration flaw in an OS and in return gaining access to earlier restricted aread. Why need? Cause usually it is very very very rare to have system/root level of access out of the box. 

**Two Variants -**  
_Horizontal Escalation_ - In this we expand our reach by taking over another account of same privilege level. It might come handy like for when some other user has SUIDs in their home  
_Vertical Escalation_ = In this we gain higher privileges. Meaning taking over the root/system account. 

**Enumeration using LinEnum -**  
LinEnum is a bash script which performs common commands related to privesc. [Here's the repo](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh). To get it to the target, we can use python server or we can also just copy pasta it over using vi or nano.

**Finding and Exploiting SUID Files -**  
SUID/GUID files are those which have their SUID/GUID bit set. Meaning that the file can be run with the permissions of the file's owner/group. In some cases, even super-user. We cam find the SUID fines using - ```find / -perm-u=s-type f 2>dev/null```  

**Exploiting a writable /etc/passwd -**  
It's pretty simple. Just make a new entry. and we are pretty much done :). I hope you already know the syntax to write in the file.

**Exploiting Cron Jobs -**  
If a file owned by root or another user is exexuted by a cron job, and and we can write to this said file, Then we can just create a command which will return shell and just copy pasta in the file. When file runs, we root.

**Exploiting PATH Variable -**  
What is PATH variable? In Unix/Linux OS which specifies where the executable programs are kept. When any command in run in the terminal, it looks up the executables with help of the PATH variable in response to commands.

We can view the PATH with help of ->```echo $PATH```

_How to exploit it?_  
Let's say we have an SUID binary. Running it, we can see that it’s calling the system shell to do a basic process like list processes with "ps". Unlike in our previous SUID example, in this situation we can't exploit it by supplying an argument for command injection, so what can we do to try and exploit this? We can re-write the PATH variable to a location of our choosing! So when the SUID binary calls the system shell to run an executable, it runs one that we've written instead! As with any SUID file, it will run this command with the same privileges as the owner of the SUID file! If this is root, using this method we can run whatever commands we like as root!



