# Writeup for Room "Agent Sudo"

This is the writeup for the box Agent Sudo

## 🌐🖥️ 1. WebSite

- The website.
- No useful information was found in `robots.txt`.
  
  ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/00.png)

## 🧪🔍⚙️ 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files but no useful information was found.
 
  ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/01.png)

- After spending a couple of minutes, the software found a list of files

  ![Fuzzing-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/02.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

   ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/03.png)

## 🌐🖥️ 4. WebSite again

- If I navigate to the list of files previously found with DirBuster:

  ![DirBuster](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/04.png) 

  ![DirBuster-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/05.png) 

- In addition, the web page has a part named archive where I can download a file with extension .tar.
  
  ![archive](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/06.png)

- If I extract the file, I get the following output:

  ![tar-extract](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/07.png)

## 🛢️💾 5. Borg Backup

- The README.md contains information about an application called Borg Backup.

  ![README.md](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/08.png)

- If I search for an application called borg using ```apt search borg```, I get the following output:

  ![apt-search](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/09.png)

- I installed the package using ```sudo apt install borg``` and I used the command ```borg```, which gives the following output:

  ![borg](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/10.png)

> [!NOTE]
> The process regarding Borg Backup will be resumed later due to a lack of information.

## 🌐🖥️ 6. WebSite again

- I navigate to url ```/etc/squid/``` and I get the following output:

  ![squid](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/11.png)

  ![squid-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/12.png)

> [!NOTE]
> More information about Squid [here](https://es.wikipedia.org/wiki/Squid_(programa))

## 🔓🗝️💻 7. Password Cracking

- I identify the hash by [hash-identifier](https://hashes.com/es/tools/hash_identifier)

  ![hash-identifier](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/13.png)

- I am going to use hashcat to crack the password:
  
  ![hashcat](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/14.png)

- The password is ```squidward```
  
  ![hashcat-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/15.png)

## 🛢️💾 8. Borg Backup again

- After cracking the password, I used the following command ```borg check home/field/dev/final_archive -v``` for checking the contents of the archive.

  ![borg-check](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/16.png)

- The Borg has an interesting parameter that can be used to restore the backup by mounting it on our system.

  ![borg-mount](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/17.png)

> [!NOTE]
> During the process of mounting the backup, it asked for a password. I used the one obtained in the previous phase.

- After mounting the backup, I found a list of directories 

  ![borg-mount](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/18.png)

- After analyzing the different folders, I located a credentials for a ssh access in ```/home/alex/Documents/note.txt```

  ![note.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/19.png)

## 🌐🖥️ 9. SSH Access

- I used the following command to connect to the machine ```ssh alex@ip-machine```

- If I use the command ```whoami``` I get the following output:

  ![ssh-connection](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/20.png)

- If I use the command ```ls -la``` I get the following output and I can see the flag named ```user.txt```:

  ![ssh-connection-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/21.png)

  ![ssh-connection-3](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/22.png)

## 🐚💻🚀 10 Privilege Escalation

- After getting the flag, I used the command ```sudo -l``` to check the privileges of the user ```alex```:
  
  ![sudo-l](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/23.png)

- The backup.sh file is located in the ```/etc/mp3backups``` directory and I used the command ```cat backup.sh``` to check the contents of the file:

  ![backup.sh](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/24.png)

> [!NOTE]
> This backup.sh file is a script that does a backup of the mp3 files in the directory ```/home/alex/Music```.


- After scanning the code, we can see that the script accepts a variable named c, and if this variable is set to ```/bin/bash -i```, it will spawn a shell with root privileges.

  ![backup.sh-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/25.png)

- If we run ```cat /root/root.txt```, we won't see anything, but if we execute the ```exit``` command, the flag from the ```root.txt``` file will be displayed: 
  
  ![root.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/26.png)