# Writeup for Room "Agent Sudo"

This is the writeup for the box Agent Sudo

## 🌐🖥️ 1. WebSite
- The website.
- No useful information was found in `robots.txt`.
  
  ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/agent-sudo/screenshots/00.png)

## 🧪🔍⚙️ 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files but no useful information was found.
  
    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/cyborg/screenshots/01.png)

    ![Fuzzing-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/cyborg/screenshots/02.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 21 is open. **FTP**
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

  ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/03.png)

## 🔓🗝️💻 4. Password Cracking
- I cracked the password of chris using the following command ```sudo hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://ip-machine -v```

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/04.png)

  ```
  Username: chris
  Password: crystal
  ```

## 📁📤📥 6. FTP Access
- I try to use the following credentials to access the FTP by web browser and I get the following output:

  ![ftp-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/05.png)

- After downloading the files, the text file contains information stating that the photos are fake.

## 🕵️‍♂️🖼️ 7. Steganography

- I used binwalk tool to check if the image has hidden information and I could extract the files from the image cutie.png.

  ![stegonography](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/06.png)
  
  ![extract-files](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/07.png)

- In the folder extracted, I found a file named To_agentR.txt but this is empty, the zip file contains a file with the same name but this file is protected by a password.

  ![To_agentR.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/08.png)

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/09.png)

## 🔓🗝️💻 8. Password Cracking again
- I cracked the password of zip using the following command ```zip2john 8702.zip > hashed.txt``` and I used the following command ```john --format=zip hashed.txt``` 

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/10.png)

- The password for the zip is ```alien``` and the content of the file is : 

  ![file-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/11.png)

## 🔓🗝️💻 9. Decrypting
- I used [cipher-identifier](https://www.dcode.fr/cipher-identifier) for decrypting ```QXJlYTUx``` and I got the following output:

  ![decrypted-text](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/12.png)

## 🕵️‍♂️🖼️ 10. Steganography
- I am going to use steghide to extract the hidden information from the image ```cute-alien.jpg```.

  ![steghide](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/13.png)

- I have gotten the following information:

  ![user-password](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/14.png)

  ```
  Username: james
  Password: hackerrules!
  ```

## 🌐🖥️ 11. SSH Access
- I used the credentials to connect to the machine:

  ![ssh-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/15.png)

- I downloaded the file named ```Alien_autopsy.jpg``` located in the ```/home/james/``` directory using the command ```scp james@10.10.57.73:/home/james/Alien_autospy.jpg /home/kali/Desktop/```

- I used google search to identify the accident and the response is ```Roswell alien autopsy```.
  
  ![accident](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/16.png)

## 🐚💻🚀 12. Privilege Escalation
- I used the command ```sudo -l``` to check the privileges of the user ```james``` and I located a vulnerability in the sudoers file - https://blog.aquasec.com/cve-2019-14287-sudo-linux-vulnerability

- If I use the command ```sudo -u#-1 bash``` I have gotten to be root user:

  ![root-user](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/17.png)

- If I do a ``` cat /root/root.txt``` I get the following output:

  ![root-user](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/18.png)

> [!NOTE]
> The agent is named Deskel