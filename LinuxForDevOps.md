# IMP Theory:

## Boot Loader

**Job:** Starts the operating system.

```
Power On
   ↓
Boot Loader (GRUB)
   ↓
Loads Linux Kernel
```

**Example:** GRUB

---

## 2. Kernel

**Job:** The core of the operating system.

It manages:

- CPU
- Memory
- Processes
- Files
- Devices (keyboard, disk, network)

```
Apps
   │
Shell
   │
Kernel
   │
Hardware
```

---

## 3. Shell

**Job:** Interface between you and the kernel.

You type commands into the shell, and it asks the kernel to perform them.

Example:

```
lsmkdir project
dockerps
```

Common shells:

- Bash
- Zsh
- Fish

## Linux System Architecture

!Screenshot from 2026-07-11 12-15-09.png

# Commands for Information About Hardware:

<aside>
💡

Command  ⇒ top 

</aside>

used to display all processes by OS, along with displays memory usage and CPU usage 

<aside>
💡

Command  ⇒ ps

</aside>

used to display the Process_Id(PID) of the bash/terminal, we running command on

<aside>
💡

Command  ⇒ kill -9 ProcessID(PID)

</aside>

used to kill a specific Process 

<aside>
💡

Command  ⇒ df -h

</aside>

used to display all disk sizes and Usage, how much storage is left, how much is occupied 

<aside>
💡

Command  ⇒ du folderPath/home/etc/CPProgram

</aside>

will give the folders along with hidden ones and folder size in bytes 

<aside>
💡

Command  ⇒ fuser folderPath/home/etc/CPProgram

</aside>

used to tell about the fileSystem of Folder (DO chatgpt for more)

<aside>
💡

Command  ⇒ vmstat OR vmstat -a

</aside>

will print the RAM memory usage 

<aside>
💡

Command  ⇒ free -h

</aside>

used to display Ram/memory usage 

<aside>
💡

Command  ⇒ nohup commandWhichWillgenerateOuputEG:free -h

Simply; nohup free -h

</aside>

It will store the ouput of the free -h command in a  file and will name it as nohup.something and if we again run this nohup with something like nohup df -h , Now it will store the output at the bottom of the file nohup.something. This way the previous output will remain where it was and this will new ouput will come below this

# Linux File System:

Everything in linux starts from root folder means ‘/’ . This cd /  folder has a number of folders inside it like var lib etc home etc 

# Linux Basic Commands:

1. mkdir ⇒ makes directory
2. ls - l ⇒ lists directories and tells us all the details 
3. pwd ⇒ current location
4. touch newfile.txt ⇒ will create a file with the name of newfile 
5. rm filename ⇒ to delete a file
6. rmdir foldername ⇒ to delete an empty directory
7. rm -r folderNme ⇒ to delete a directory (which has some data/files inside it) 
8. cat filename ⇒ to see the content inside file
9. zcat filename.zip ⇒ to see the content inside a zip file
10. echo “Hello King” ⇒ prints/outputs the content inside “ ” in this case Hello King
11. echo “Hello King” > filename.txt  ⇒ will take the output and store it inside the filename.txt file (NOTE : If there is no file with the name of filename.txt it will create one )
12. head  filename.txt ⇒ will print/output the first 5 line of the content inside filename.txt file
13. head -n 8 filename.txt ⇒ will print/output the first 8 lines of the content inside filename.txt file because we did -n for number of lines and specified 8 it will print 8 lines 
14. tail filename.txt ⇒ will print/output the last 5 line of the content inside filename.txt file
15. tail -n 8 filename.txt ⇒ will print/output the last 8 lines of the content inside filename.txt file because we did -n for number of lines and specified 8 it will print last 8 lines 
16. tail -f filename.txt ⇒ will do same as tail filename.txt plus will print newly added lines too, the terminal wouldn’t be closed here as the new content will be added in this file it will print it rightaway. USED in logs analysis 
17. less filename.txt ⇒ will print the content page by page, used when we want to read a huge file so it will automatically show us page by page
18. more filename.txt ⇒ will print the content in big page size
19. cp sourceDestinationOfFile whereToPutThatfile ⇒ example : cp mat.txt  /deb/Program . What will happen is it will copy the file mat.txt and will paste it inside the deb/Program folder ,BUT the file wouldn’t be deleted from where it is now, a copy will be made at /deb/Program directory
20. cp -r sourceDestinationOfFolder whereToPutThatfolder ⇒ example : cp -r devFolder  /deb/Program .  -r is used for doing the same as 17. but for directory
21. mv sourceDestinationOfFile whereToPutThatfile ⇒ example : mv mat.txt  /deb/Program . What will happen is it will move the file mat.txt and will paste it inside the deb/Program folder ,BUT the file will be deleted from where it is now, will be move  at /deb/Program directory
22. mv -r sourceDestinationOfFolder whereToPutThatfolder ⇒ example : mv -r Pet  /deb/Program . What will happen is it will move the folder Pet and will paste it inside the deb/Program folder ,BUT the file will be deleted from where it is now, will be move  at /deb/Program directory
23. wc fileName.txt ⇒ it will tell 3 things about the file First will be how many lines, second will be how many words and then the last will be bytes size of file 
24. cut -b 1-4 filename.txt ⇒ this will give the content inside the file only 1 to 4 bytes because of -b flag and we said 1-4 so it will give only content that fits in 1 to 4 bytes
25. echo “Hello” | tee fileName.txt ⇒ this tee command will print the Hello as well as save it inside the fileName.txt file 
26. sort fileName.txt ⇒ This command will sort the content inside the file …
27. diff file1.txt file2.txt ⇒ this command will give us the content difference   b/w these 2 files 
28. vi filename.txt ⇒ will open the file in text editor (Inside the text editor now if I press i the text-editor will go in insert mode means will allow me to do changes)

## Hard Link & Soft Link (SUPER IMPORTANT FOR INTERVIEW)

Link means creating a short cut Just like we used to create a shortcut of any file/folder in our desktop in windows same as that we create a shortcut or link in any directory we want to create that shortcut. 

### ***TYPES***:

### Hard Link

Imagine we have a folder /etc/home/DevEngine we created a Hard Link of it at /home/games .Now if our /etc/home/DevEngine gets deleted our shortcut at /home/games will remain as it is safe, and will be accessible any time we want but in our /home/games directory

COMMAND: ln  /home/path_where_that_file_is.txt  NameOfShortCut 

This command will create a hardLink of the .txt file with the name of NameOfShortcut in our current directory where we right now.

### Soft Link

Imagine we have a folder /etc/home/DevEngine we created a Soft Link of it at /home/games .Now if our /etc/home/DevEngine gets deleted our shortcut at /home/games will also be deleted,  become in-accessible , it will not remain up there in that case

COMMAND: ln -s /home/path_where_that_file_is.txt NameOfShortCut 

This command will create a softLink of the .txt file with the name of NameOfShortcut in our current directory where we right now.

“ls -ltr” command will show what files are linked along with normal files  

# Connecting with other Machines: (IMP)

## Setting Up Connection Using SSH

### What is SSH?

SSH is a secure network protocol used to:

- Log in to a remote computer or server over a network.
- Execute commands on a remote machine.
- Securely transfer files (using tools like SFTP or SCP).
- Manage servers and network devices with encrypted communication.

The Way we establish a remote connection using SSH to a remote machine is that we need Private key and a Public Key. So the Public key will be used by the server/machine we want to connect to and Private key will be used by the machine we are connecting from. So, Connect to should have a Public key and connect from machine should have a private key

  

## Generating Private Key By Our Self

### Step 1: Open your terminal

### Step 2: Run this command

```
ssh-keygen-t ed25519
```

### Step 3: Press Enter three times

You'll see something like:

```
Enter file in which to save the key:
/home/taha/.ssh/id_ed25519
```

Press **Enter**.

Then:

```
Enter passphrase (empty for no passphrase):
```

Press **Enter** (or enter a passphrase if you want extra security).

Then:

```
Enter same passphrase again:
```

Press **Enter** again.

---

## Done! 🎉

SSH creates two files:

```
~/.ssh/id_ed25519       ← Private key (KEEP SECRET)
~/.ssh/id_ed25519.pub   ← Public key (share this with servers)
```

- **Private key (`id_ed25519`)** stays on your computer.
- **Public key (`id_ed25519.pub`)**

## View your public key

```
cat ~/.ssh/id_ed25519.pub
```

It will look something like:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBx... taha@your-computer
```

Copy this entire line wherever you're asked to add your SSH public key.

## Connecting with a Cloud Server (ec2)

When connecting to a cloud server and want to access the cloud machine we can use SSH to remotely control that server. The Private key will be generated by the cloud service provider automatically when we setup a virtual machine it generates a key pair. The private key will be downloaded in the machine we are connecting from and Public will be in the connected to machine

Now, to connect we to Server through this command

```jsx
COMMAND ---> ssh -i pathTOPrivateKey_/home/etc/key.pem (LinkGivenByAWS)usernameOFconnectToMachine@publicDNS
```

BUT If we are connecting to a Windows system we use Putty instead of SSh

# User & File Management:

## System Info commands

1. uname ⇒ tells which system you are on Linux, Darwin(used by MacOS) …
2. uptime ⇒ tells from how much time your system is up
3. date ⇒ tells the date
4. whoami ⇒ gives the current username you are logged in from (IMP FOR INTERVIEW)
5. who ⇒ gives at what time what user logged in (IMP FOR INTERVIEW, DIFF b/w ) 
6. which toolName OR which forge OR which bash OR which java ⇒ this will tell where the command bash or forge is installed , gives it location like /etc/home/forge this is where forge is or java is 
7. shutdown ⇒ to power off the system
8. reboot ⇒ to restart the system 
9. apt ⇒ it is not a command, it is a command Line package manager, used to manage packages from within system  (IMP FOR INTERVIEW, DIFF b/w 10 )
10. apt-get ⇒ it is also not a command, it is a command Line package manager, used to manage packages from Internet , used to install , reinstall , remove and more (IMP FOR INTERVIEW)
11. apt-get update ⇒ updates the whole system
12. ctrl+r (IN TERMINAL) ⇒ will give us a search type area to search from previous commands
13. sudo useradd -m NewUserName ⇒ this will create a new user but if we didn’t use this -m flag it would have worked without it too but then it wouldn’t have created a directory of our new user as we have for other user like if you see /home directory  you will see a taha name directory which is because my username is taha so it created a directory for it to store user specific stuff. 
14. sudo passwd NewUserName ⇒ this will allow you to set the password for the new user
15. su NewUserName ⇒ this command will allow you to change the user 
16. exit (in terminal) ⇒ this command will change your user to your primary user(The very first user of system) ONLY if we are accessing system as different user then primary user. IFF we use this exit command in our normal primary user this will close the terminal
17. cat /etc/passwd ⇒ at the end of this list you will get the users ID
18. sudo userdel Arjin ⇒ will delete the User arjin  
19. sudo groupadd DevEngineer ⇒ this will create a group with the name of DevEngineers
20. cat etc/group ⇒ this will give the list of all groups with their Ids and also remember every individual user group is also created when we creates a new user. So, along with the groups we will also see the users because every user creates a group, whenever we create a new user.
21. sudo gpasswd -a userName GroupName ⇒ this command will add the ‘userName’ user in the group ‘GroupName’ 
22. sudo gpasswd -M userName1,userName2,userName4 Groupname ⇒ this command will push 3 users in the group
23. sudo groupdel GroupName ⇒ this will delete the group ‘GroupName’. The users inside that group will not be deleted only the group will be deleted

## File Management

READING FILE PERMISSIONS (SEE FILE PERMISSION CHART in GOOGLE FOR PERMISSIONS NUMBER)

!image.png

FILE PERMISSION NUMBERS CHART

!image.png

**COMMANDS**

1. ls -l ⇒ to list down the file permissions along with files and the owner of file and groupOfUser that file is in
2. chmod 745 fileORDirectoryName ⇒ this will change the permission of the given directory/file to first 7 means for the user who has created it and 4 means permission for the group and then 5 means for other users who aren’t in the group.
3. umask ⇒ this will print the by default permissions of the file you creates, this will tell what will be the permissions when you will create a file or directory 
4. cat .bashrc ⇒ locate this file in your system this .bashrc file contains the umask variable which you can change to change the default permissions of your file (IMP FOR INTERVIEW)
5. sudo chown NewOwner_Username FileName ⇒ this command will give the ownership of the file/folder to new owner .
6. sudo chgrp NewGroupNameFile FileName ⇒ this command will give the ownership of the file/folder to group. USED to change the ownership of group file/folder

ZIP FILES:

1. zip wantTocreateFile.zip fileName/ ⇒ this will zip the file and will name it as ‘wantTocreateFile’ 
2. zip -r  wantTocreateFolder.zip folderName/ ⇒ this will zip the folder and with -r mean recursively , -r used for folders, and will name it as ‘wantTocreateFolder’ 
3. cp zipFile.zip /etc/home/Cp ⇒ will copy the zip file
4. unzip zipFile.zip ⇒ will unzip the file 

JUST LIKE ZIP we have a tool called Gunzip which does the same work just the .zip is replaced with .gzip

The tar command in Linux (short for Tape Archive) is a powerful tool used to create, view, extract, and manage archive files.  Tar  compress archives using gzip(Gunzip).

USING TAR

1. tar -cvzf ZippedFileNameWantTocreate.tar.gz FolderNameWantTOZIP ⇒ this will zip in .tar.gz form . -c means compres , -v means verbose, -z means use gunzip to zip , -f  for file/folder
2. tar -xvzf ZippedFileNameWantTocreate.tar.gz ⇒ will extract means unzip the tar.gz file, the -x means extract  

# Sending Files From Local to Cloud (IMP FOR INTERVIEW)

COMMAND TO BE EXECUTED IN LOCAL :

1. scp -i “/pathOfPrivateKey.pem_File_InCLOUD_Example/home/etc/downloads/privateKey.pem” FileNameWANtTOSend  cloudDNSLinkGivenBYAWS.aws.com:/PATHOF_LOCATIONTO_STORETHE_INCOMINGFILE_Example/etc/home/cp  ⇒ ⇒ 

This command is used to send a file/folder from local to cloud , -i is for authentication used to enter path of private key stored in cloud then file/folder name want to send then the link of CLOUD with : after this colon we enter the path of where to store that file  … IF WANT TO SEND A FOLDER use -r too

# Sending Files From Cloud to Local (IMP FOR INTERVIEW)

COMMAND TO BE EXECUTED IN LOCAL : (YES)

1. scp -i “/pathOfPrivateKey.pemFileInCLOUDExample/home/etc/downloads/privateKey.pem”  cloudDNSLinkGivenBYAWS.aws.com:/PATHOF_LOCATIONTO_COPYFILE_FROMExample/etc/home/cp  /LocationOf_LOCAL_TOSTORE_TO ⇒ ⇒ 

 This command is used to send a file/folder from cloud to local , -i is for authentication used to enter path of private key stored in cloud tthen the link of CLOUD with : after this colon we enter the path of where file is stored THEN enter the path of local where to copy to  … IF WANT TO SEND A FOLDER use -r too

## RSYNC  COMMAND : ****(IMP FOR INTERVIEW)

- DO IT AFTER UNDERSTAND BOTH CLOUD TO LOCAL AND LOCAL TO CLOUD
1. rsync -e “ssh -i /pathOfPrivateKey.pemFileInCLOUDExample/home/etc/downloads/privateKey.pem ” -avz folderPathInLocal cloudDNslink.aws.com:/pathofFolderInCloud ⇒ 

rsync is used when we have a same folder in cloud and local both , we might have added a newfile in local or cloud now we want both cloud and local to have the newly added file So we use rsync command now -e we set ssh and with -i we do authentication then we have -a means archive -v means verbose and -z means zip the file then enter the path to local folder after that add the DNSOFCLOUDAWS then : after colon right path where that folder is in cloud  -z means it wouldn’t be zipped when send to just for while sending it is zipped , after -e and -i we enter the source then destination . so you can do vice versa too 

# NETWORKING COMMANDS:

1. ping google.com ⇒ we ping a website/IP to check whether it is up or not, the way it checks is by sending or receiving data packets
2. netstat ⇒ It's commonly used to see **which ports are open and which processes are using them**.
3. ifconfig ⇒ Used for **all network interfaces** (Ethernet, Wi-Fi, etc.). It shows/configures IP addresses and network settings.
- iwconfig ⇒ Used **only for Wi-Fi (wireless)** interfaces. It shows/configures wireless settings like SSID, signal strength, frequency, and mode.
- traceroute website.comORIPADDRESS  ⇒ it tell the exact route like from your computer request went to this server then this server and then to the desired websites server. Shows the **path (routers/hops)** packets take to reach a destination.
- tracepath website.comORIPADDRESS ⇒  Simpler and nice presented way same as traceroute
- mtr website.comORIPADDRESS ⇒ combine the ping and traceroute command, mtr means my trace route
- nslookup website.comORIPADDRESS ⇒ So you should know that at every port number we can setup a domain , so this will tell the **IP address of a domain name** (or the domain name from an IP).
- telnet website.comORIPADDRESS 444 ⇒ So you should know that at every port number we can setup a domain, so this will tell us the IP of Domain at that specific port number we have given in this case 443 , we can change it and can see for other port numbers as well like 80
- hostname ⇒ Displays your computer's **hostname** (its name on the network). tell the machine's name.
- cat /etc/hosts ⇒ this file contains the localhost ip like 127.0.0.1 So if you want to change your localhost you can change this 127.0.0.1 to any you want Eg:  127.0.0.1:443
- **ip address show ⇒** Displays all network interfaces and their IP addresses. Check your computer's IP address. See if a network interface is up or down.
- ss ⇒ does same as netstat JUST LEARN FOR INTERVIEW IF ASKED FOR ALTERNATIVES
- whois  website.comORIPADDRESS ⇒ find details about the domain, from where bought the domain when bought it
- ifplugstatus ⇒ tells whether your network interfaces are working or not
- arp ⇒ tells the mac address of the network Interface Card
- curl -X GET APILINK | jq ⇒ so curl is used to call api endpoints from terminal GET is the request type and | is to execute next command and jq is to beautifully represent the api output
- wget LinkFromInternetOfFileToDownload ⇒ wget and then enter the link of the file to download from internet
- watch -n 10 commandEg:top ⇒ this will run the given command every 10 seconds and will display the output
- route ⇒ used to see the internet gateway(see more on chatgpt)
- iptables ⇒ see chatgpt for more info on this
- dig website.comORIPADDRESS  ⇒

It asks the DNS server:

"What is the IP address (or other DNS records) for google.com?"

Unlike nslookup, dig provides more detailed DNS information, such as:

A records (IPv4)
AAAA records (IPv6)
MX records (mail servers)
NS records (name servers)
TXT records
Query time and the DNS server that responded

It asks the DNS server:

"What is the IP address (or other DNS records) for google.com?"

Unlike nslookup, dig provides more detailed DNS information, such as:

A records (IPv4)
AAAA records (IPv6)
MX records (mail servers)
NS records (name servers)
TXT records
Query time and the DNS server that responded

It asks the DNS server:

> **"What is the IP address (or other DNS records) for `google.com`?"**
> 

Unlike `nslookup`, `dig` provides **more detailed DNS information**, such as:

- A records (IPv4)
- AAAA records (IPv6)
- MX records (mail servers)
- NS records (name servers)
- TXT records
- Query time and the DNS server that responded

# AWK, SED & GREP COMMANDS (Imp)

## “ V.IMP FOR INTERVIEW DIFF B/W SED & AWK”

IF DATA IS FORMATTED/structured  LIKE DATA IN .csv (comma seperated value) or in tab seperated value .tsv we use awk , otherwise if data not formatted we use sed(stream editor) used for real time data   

see —> https://www.youtube.com/watch?v=e01GGTKmtpc&t=19251s

# Linux Volume Management:

TO BE DONE WHEN WILL LEARN AWS  

BUT VIDEO is in LINUX ONE SHOT
