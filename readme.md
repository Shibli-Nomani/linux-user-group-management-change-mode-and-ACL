# 🚀 Ubuntu 20.04(Linux): Master File Access: User Creation, Group Management, and Permissions with chmod 🔒 and ACL

**🌟 "Terminal Environment: Running Ubuntu on Windows with VSCode – All Systems Go!"**

- Install WSL and Ubuntu 20.04 on Windows 💻.
- Set up VSCode for remote development through WSL 🖥️.
- Run Linux commands and manage files within Windows 🗂️.
- Develop cross-platform projects using a unified environment 🌍.

**`chmod`** stands for "change mode" and is a command in Unix/Linux systems used to change file and directory permissions. It controls who can read, write, or execute a file or directory.

**`sudo (short for "superuser do")`** is a command in Unix/Linux that allows a permitted user to execute a command as the root (superuser) or another user, providing elevated privileges temporarily.



**Permission Types:**

- Read (r) – View the contents of the file.
- Write (w) – Modify or delete the file.
- Execute (x) – Run the file as a program/script.

**Permission Categories:**

- User (u) – The owner of the file.
- Group (g) – A group of users.
- Others (o) – Everyone else.

**Permission Values with Icons:**

- r (Read) 📖 = 4
  - Allows viewing or listing file contents.
- w (Write) ✍️ = 2
  - Allows modifying or deleting files.
- x (Execute) 🚀 = 1
  - Allows running the file as a program or accessing a directory.

**Combining Permissions:**
- rwx (📖✍️🚀) = 7 – Full access
- rw- (📖✍️) = 6 – Read and write
- r-x (📖🚀) = 5 – Read and execute
- r-- (📖) = 4 – Read-only

### Change to current directory to the root directory
```sh
cd \
```
### Create and change directory
```sh
mkdir linux-usr-grp-change-mode
cd linux-usr-grp-change-mode
```
### Create and edit new file readme.md

```sh
touch readme.md
vi readme.md
```
### Create group and access prvilage

```sh
sudo groupadd full-privilage 
cat /etc/group
```
  - output: full-privilage:x:1004:

### Create group and user
```sh
sudo groupadd mid-access-team
cat /etc/group
```
- output: mid-access-team:x:1005:

```sh
sudo useradd John -g 1005
sudo useradd Jeson -g 1005
cat /etc/passwd
```
- John:x:1003:1005::/home/John:/bin/sh
- Jeson:x:1004:1005::/home/Jeson:/bin/sh
- name:pass:uid:gid:: directory

### Attach a Directory to a Group and Check Group Permissions and Status:

```sh
mkdir read-only-directory
ls -ld read-only-directory
```
note: drwxrwxrwx 1 shibli_nomani shibli_nomani 4096 Nov 28 21:53 read-only-directory


### Readonly directory Access with chmod (numeric value) 
```sh
chmod 750 read-only-directory
```
- Owner: Full permissions (d(rwx)) - 7 (4+2+1) 
- Group: Read and execute permissions (r-x) - 5 (4+1)
- Others: No access (---) - 0 (0)
**note:** directory need execute permission to access the file inside directory

- Check the folder status
```sh
ls -ld read-only-directory/
```

### Attached group with that directory
```sh
sudo chgrp mid-access-team read-only-directory
```
- sudo chgrp group-name directory-name(applied chmod)

### Check the group and user permission status for that directroy
```sh
ls -ld read-only-directory
```
- output: drwxr-x--- 2 shibli_nomani mid-access-team 4096 Nov 28 21:53 read-only-directory

- Owner: Full permissions (d(rwx)) - 7 (4+2+1) -> d refers as directory
- Group: Read and execute permissions (r-x) - 5 (4+1)
- Others: No access (---) - 0 (0)
### 🚀 Create a text file and access from account owner: shibli_nomani
```sh
vi test.txt
```
or 
```sh
touch test.txt
```
**note:** vim for file create (i - insert; Esc - To escape after writting; :wq - write&quite)

### Go to other user and access file
```sh
sudo su - John
whoami  
ls
```
### Access test.txt file
```sh
vi test.txt
```
![alt text](../screenshoot/group-access-1.png)

**note:** to quite from readonly condition: :q!

### Exit and back to owner directory
```sh
exit
```
## Create Directory and File permission 

### Directory
```sh
mkdir individual-file-access
```
### Group
```sh
sudo groupadd -g 1050 individual-file-access
```
### User aG(assign Group)
note: John and Jeson have already exist. check users
```sh
cat /etc/passwd
```
- Jeson:x:1004:1005::/home/Jeson:/bin/sh
- Jack:x:1005:1006::/home/Jack:/bin/sh
```sh
sudo usermod -aG individual-file-access John
sudo usermod -aG individual-file-access John
```
#### Groups where John exists
```sh
groups John
```
- John : mid-access-team individual-file-access
##### Delete and Add New User in a Group 
```sh
sudo userdel David
```
```sh
sudo useradd -g individual-file-access David
cat /etc/group
groups David
```
### Create Files and Assign Individual Access

```sh
cd filewise-chmod-directory/
touch john-write.txt jeson-write.txt david-write.txt
ls
```
#### edit text/others file using VIM
```sh
vi davidwrite.txt
vi jeson-write.txt
vi john-write.txt
```
### Give Individual file access to each user
John, Jeson, David

#### access permission by applying chmod
- Owner: rw- (4+2) = 6
- Group: r-- (4) = 4
- Others: --- (0) = 0
```sh
chmod 640 david-write.txt
chmod 640 jeson-write.txt
chmod 640 john-write.txt
```
#### Assign read-write permission for different user different text file
If you want to assign specific permissions to individual users on a file without changing the group and without using setfacl, the standard chmod command won't give you this fine-grained control.
- Install acl
```sh
sudo apt install acl
```
```sh
setfacl -m u:David:rw david-write.txt
setfacl -m u:Jeson:rw jeson-write.txt
setfacl -m u:John:rw john-write.txt
```

![alt text](..\linux-usr-grp-change-mode\screenshoot\user-individual-access.png)

### Go to user account and edit
```sh
sudo su - David
whoami  
ls
```
# access file
```sh
$ vi david-write.txt
$ vi jeson-write.txt
```
jeson-write.txt" [Permission Denied] 


# 🚀 Summary of Steps:
- Create and manage directories and files 🗂️.
- Create and manage user and group accounts 👥.
- Assign permissions using chmod 🔒 (numeric and symbolic).
- Assign specific user access with ACL (Advanced Access Control) 🔐.
- Verify and manage user/group permissions 🔍.
- This setup ensures you have a flexible system for controlling user access and modifying file permissions effectively.

## Authors

- [@LinkedIn Khan MD Shibli Nomani](https://www.linkedin.com/in/khan-md-shibli-nomani-45445612b/)
