# Writeup for Room "Library"

This is the writeup for the box Library

## 🔍 Enumeration Phase

## 1. WebSite
- The website has a blog.
- The following information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/01.png)

> [!NOTE]
> The User-agent indicates rockyou

## 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files, the following image shows the results:
  
    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/02.png)

- Dirsearch was used to find more directories and files, the following image shows the results:

    ![Fuzzing-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/03.png)

> [!NOTE]
> There is a directory named ```images```

## 📡 3. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/04.png)

## 4. WPScan and WhatWeb
- I have used WPScan and WhatWeb to find more information about the website.
  
    ![whatweb](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/05.png)


## 🔓🗝️💻 4. Password Cracking
- I cracked the password of meliodas using the following command ```sudo hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://ip-machine -I```

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/06.png)

  ```
  Username: meliodas
  Password: iloveyou1
  ```

> [!NOTE]
> I found the user while gathering information about the website

## 🔑 6. SSH Access
- I used the credentials to connect to the machine:

  ![ssh-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/07.png)

- When I executed a ```ls``` command, I saw the first flag.

  ![first-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/08.png)

## 🐚💻🚀 7. Privilege Escalation
- In the same directory, there is a file named ```bak.py```, which is a python script that creates a backup of the website as the root user.
  
  ![bak.py](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/09.png)

  ![bak.py-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/library-cms/screenshots/10.png)

> [!NOTE] 
> I tried to get a root shell by escalating privileges, but I couldn’t succeed even with a writeup. Look the writeups of TryHackMe rooms for more information.