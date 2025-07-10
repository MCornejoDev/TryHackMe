# Writeup for Room "Brooklyn Nine-Nine"

This is the writeup for the box Brooklyn Nine-Nine

---

## 🌐🖥️ 1. WebSite
- The website is running on Apache.
- No useful information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/01.png)

## 🕵️‍♂️🖼️ 2. Steganography
- In the code source of the website, I have found a comment that says ```Have you ever heard of steganography?```.

    ![web-source](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/02.png)

- I have used differents tools to check if the image has hidden information.

    ![stegonography](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/03.png)

    ![stegonography-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/04.png)

    ![stegonography-3](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/05.png)

## 📡 3. Nmap Scan
The following information was found:
  - The port 21 is open. **FTP**
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/06.png)

## 📁📤📥 4. FTP Access
- I try to use the following credentials to access the FTP by web browser and I get the following output:

  ![ftp-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/07.png)

- The file contains the following information:

  ![file-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/08.png)

## 🔓🗝️💻 5. Password Cracking
- I cracked the password of jake using the following command ```sudo hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://ip-machine``` and I got the following output:

  ![password-cracked](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/09.png)

  ```
  Username: jake
  Password: 987654321
  ```

## 🌐🖥️ 6. SSH Access
- I used the credentials to connect to the machine:

  ![ssh-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/10.png)

- After finding for the differents users, I have located the first flag in the holt user :

  ![user-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/11.png)

## 🐚💻🚀 7. Privilege Escalation
- I used the command ```sudo -l``` to check the privileges of the user ```jake```

  ![sudo-l](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/12.png)

- I have used less for checking the content of the second flag in ```root/root.txt``` and I got the following output:

  ![root-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/brooklyn-ninenine/screenshots/13.png)
