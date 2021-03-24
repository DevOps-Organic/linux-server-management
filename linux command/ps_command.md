Linux provides us a utility called **ps** for viewing information related with the processes on a system which stands as abbreviation for **“Process Status”**. ps command is used to list the currently running processes and their *PIDs* along with some other information depends on different options. It reads the process information from the virtual files in **/proc** file-system. /proc contains virtual files, this is the reason it’s referred as a virtual file system.

**ps** provides numerous options for manipulating the output according to our need.

**Syntax –**  
> ps [options]

**Options for ps Command :**

### 1. **Simple process selection** : Shows the processes for the current shell – 
> [root@localhost ~]# ps  
> &emsp;PID TTY&emsp;&emsp;&emsp;&emsp;TIME CMD  
> &emsp;12330 pts/0&emsp;&emsp;00:00:00 bash  
> &emsp;21621 pts/0&emsp;&emsp;00:00:00 ps  

**PID** – the unique process ID  
**TTY** – terminal type that the user is logged into  
**TIME** – amount of CPU in minutes and seconds that the process has been running  
**CMD** – name of the command that launched the process.  

### 2. **View Processes** : View all the running processes use either of the following option with ps – 
> [root@localhost ~]# ps -A  
> [root@localhost ~]# ps -e  

### 3. **View Processes not associated with a terminal** : View all processes except both session leaders and processes not associated with a terminal. 
> [root@localhost ~]# ps -a  
> &emsp;PID TTY&emsp;&emsp;&emsp;&emsp;TIME CMD  
> &emsp;27011 pts/0&emsp;&emsp;00:00:00 man  
> &emsp;27016 pts/0&emsp;&emsp;00:00:00 less  
> &emsp;27499 pts/1&emsp;&emsp;00:00:00 ps  

**Note** – You may be thinking that what is session leader? A unique session is assing to evry process group. So, session leader is a process which kicks off other processes. The process ID of first process of any session is similar as the session ID.

### 4. **View all the processes except session leaders** : 
> [root@localhost ~]# ps -d  

### 5. **View all processes except those that fulfill the specified conditions (negates the selection)** : 
> [root@localhost ~]# ps -a -N  
OR  
> [root@localhost ~]# ps -a --deselect  

### 6. **View all processes associated with this terminal** : 
> [root@localhost ~]# ps -T  

### 7. **View all the running processes** : 
> [root@localhost ~]# ps -r  

### 8. **View all processes owned by you** : Processes i.e same EUID as ps which means runner of the ps command, root in this case – 
> [root@localhost ~]# ps -x  

## **Process selection by list**

### 1. Select the process by the command name. This selects the processes whose executable name is given in cmdlist. There may be a chance you won’t know the process ID and with this command it is easier to search.
**Syntax** : ps -C command_name
> Syntax :  
> ps -C command_name  
>
> Example  
> [root@localhost ~]# ps -C httpd  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;11696 ?&emsp;&emsp;&emsp;00:00:26 httpd  
> &emsp;67039 ?&emsp;&emsp;&emsp;00:00:00 httpd  
> &emsp;67040 ?&emsp;&emsp;&emsp;00:00:53 httpd  
> &emsp;67041 ?&emsp;&emsp;&emsp;00:00:52 httpd  
> &emsp;67042 ?&emsp;&emsp;&emsp;00:00:54 httpd  
> &emsp;67279 ?&emsp;&emsp;&emsp;00:01:23 httpd  

### 2. Select by group ID or name. The group ID identifies the group of the user who created the process. 
> Syntax :  
> ps -G group_name  
> ps --Group group_name  
>
> Example  
> [root@localhost ~]# ps -G root  

### 3. View by group id : 
> Syntax :  
> ps -g group_id  
> ps -group group_id  
>
> Example  
> [root@localhost ~]# ps -g 1

### 4. View process by process ID. 
> Syntax :  
> ps p process_id  
> ps -p process_id  
> ps --pid process_id  
>
> Example  
> [root@localhost ~]# ps -p 67039  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;67039 ?&emsp;&emsp;&emsp;00:00:00 httpd  

You can view multiple processes by specifying multiple process IDs separated by blank or comma –
> [root@localhost ~]# ps -p 67039 67279  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;67039 ?&emsp;&emsp;&emsp;00:00:00 httpd  
> &emsp;67279 ?&emsp;&emsp;&emsp;00:00:00 httpd

### 5. Select by parent process ID. By using this command we can view all the processes owned by parent process except the parent process. 
> [root@localhost ~]# ps -p 766  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;766 ?&emsp;&emsp;&emsp;00:00:06 NetworkManager
>
> [root@localhost ~]# ps --ppid 766  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;19805 ?&emsp;&emsp;&emsp;00:00:00 dhclient  

### 6. View all the processes belongs to any session ID. 
**Syntax** :  
ps -s session_id  
ps --sid session_id 

> [root@localhost ~]# ps -s 1248  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;1248 ?&emsp;&emsp;&emsp;00:00:00 dbus-daemon  
> &emsp;1276 ?&emsp;&emsp;&emsp;00:00:00 dconf-service  
> &emsp;1302 ?&emsp;&emsp;&emsp;00:00:00 gvfsd  
> ...  

### 7. Select by tty. This selects the processes associated with the mentioned tty : 
**Syntax** :
ps t tty
ps -t tty
ps --tty tty

> [root@localhost ~]# ps -t pts/0  
> &emsp;PID TTY&emsp;&emsp;&emsp;TIME CMD  
> &emsp;4922 pts/0&emsp;&emsp;&emsp;00:00:00 bash  
> &emsp;117126 pts/0&emsp;&emsp;00:00:00 ps   

### 8. Select by effective user ID or name.
**Syntax** :
ps U user_name/ID  
ps -U user_name/ID  
ps -u user_name/ID  
ps –User user_name/ID  
ps –user user_name/ID   

## **Output Format control**
These options are used to choose the information displayed by ps. There are multiple options to control output format. These option can be combined with any other options like e, u, p, G, g etc, depends on our need. 

### 1. Use -f to view full-format listing. 
> [root@localhost ~]# ps -af  

### 2. Use -F to view Extra full format. 
> [root@localhost ~]# ps -F  

### 3. To view process according to user-defined format. 
**Syntax** :  
ps --format column_name  
ps -o column_name  
ps o column_name  

> [root@localhost ~]# ps -aN --format cmd,pid,user,ppid  

### 4. View in BSD job control format : 
> [root@localhost ~]# ps -j 

### 5. Display BSD long format : 
> [root@localhost ~]# ps l 

### 6. Add a column of security data. 
> [root@localhost ~]# ps -aM 

### 7. View command with signal format. 
**Syntax** :  
ps s pid_number

### 8. Display user-oriented format 
> [root@localhost ~]# ps u pid_number

### 9. Display virtual memory format 
> [root@localhost ~]# ps v pid_number

### 10. If you want to see environment of any command. Then use option **e** – 
> [root@localhost ~]# ps ev pid_number

### 11. View processes using highest memory. 
> [root@localhost ~]# ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem

>>  – print a process tree  
>> [root@localhost ~]# ps --forest -C sshd

### 12. List all threads for a particular process. Use either the **-T or -L** option to display threads of a process. 
> [root@localhost ~]# ps -C sshd -L
