# Writeup for Room "Simple CTF"

This is the writeup for the box Simple CTF

## 🔍 Enumeration Phase

## 1. WebSite
- The website is running on Apache.
- The following information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/01.png)

## 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files, the following images show the results:
  
    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/02.png)

- Dirsearch was used to find more directories and files, the following images show the results:

    ![dirsearch](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/03.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 21 is open. **FTP**
  - The port 80 is open. **HTTP (Apache)**
  - The port 2222 is open. **SSH**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/04.png)

- I used the command ```nmap``` with ```--script vuln``` to check for vulnerabilities.

    ![Nmap Scan-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/05.png)

## 4. What WebSite?
- whatweb gives me:

    ![whatweb](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/06.png)

## 🌐🖥️ 5. WebSite
- I navigated to the ```simple``` route and I found the following:

    ![simple](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/07.png)

- This machine is a cms

    ![cms](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/08.png)

## 6. Searchsploit - Reverse Shell 
- I use searchsploit to search exploits for cms.

    ![searchsploit](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/09.png)

> [!NOTE]
> I downloaded the exploit for the cms named ```CMS Made Simple < 2.2.10 - SQL Injection webapps/46635.py```.

- I have used python to execute the exploit:

    ![exploit](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/10.png)

- I have gotten the following output:

    ![output](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/11.png)

- I have used [cipher-identifier](https://www.dcode.fr/cipher-identifier) to decrypt the text and I got the following output:

    ![decrypted-text](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/12.png)

    ![decrypted-text-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/13.png)

> [!NOTE]
> I have gotten a password ```secret```

## 🔑 7. SSH Access.
- Let's try logging in via SSH with the obtained credentials:

```
user: mitch  
password: secret
```

![ssh-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/14.png)

- I have used ```bash -i``` to improve the shell

    ![bash-i](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/15.png)

## 🐚💻🚀 8. Privilege Escalation

- I have tried to use the command ```sudo -l``` to check the privileges of the user ```mitch```:

    ![sudo-l](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/16.png)

- I executed the command ```sudo vim -c ':!/bin/sh'``` and I got to be root user:

    ![root-user](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/17.png)

> [!NOTE]
> The command for vim has been gotten from https://gtfobins.github.io/

- I have gotten the second flag ```root.txt``` with the command ```find / -type f -name root.txt 2>/dev/null```

    ![root.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/simple-ctf/screenshots/18.png)