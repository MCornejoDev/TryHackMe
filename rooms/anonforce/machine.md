# Writeup for Room "Anonforce"

This is the writeup for the box Anonforce

## 🌐🖥️ 1. WebSite
- The website.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/00.png)

## 🧪🔍⚙️ 2. Fuzzing
- There’s no Apache server, so directory listing is excluded.

## 📡 3. Nmap Scan
- The following information was found:
  - The port 21 is open. **FTP**
  - The port 22 is open. **SSH**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/01.png)

- In addition, I have used nmap with ```--script=vuln``` to check for vulnerabilities.

    ![nmap-vuln](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/02.png)

## 💉🛢️ 4. FTP Access

- I tried to use the following credentials to access the FTP:
  ```
  Username: anonymous
  Password: 
  ```

  ![ftp-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/03.png)

- I found the first flag in the melodias directory, I downloaded it and I used ```cat user.txt``` to get the flag.

  ![user.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/04.png)

  ![cat-user.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/05.png)

- I also found the following files inside the notread directory and I downloaded them:

    ![notread](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/06.png)

## 🔓🗝️💻 5. Password Cracking

- I attempted to import the ```private.asc``` key, but it prompted for a password.

    ![private.asc](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/07.png)

- Then I decided to crack the password for ```private.asc```, and we succeeded using the following commands:

    ```gpg2john private.asc > hash.txt```
    ```john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt ```

    ![crack-password](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/08.png)

- I also decrypted the ```backup.gpg``` file using the following command and the password I found in the previous step:

    ```gpg --output backup.txt --decrypt backup.gpg```

    ![decrypt-backup](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/09.png)

- I got ```backup.txt``` and if I look the contents of the file:

    ![backup.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/10.png)

    ![backup.txt-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/11.png)

- I ran john against ```backup.txt``` and successfully cracked the root password.

    ![root-password](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/12.png)

## 🔑 6. SSH Access.

- If I made an SSH connection using the decrypted root password, and we were able to log in, then I have executed a ```ls``` and I have seen the flag ```root.txt```.
  
    ![root.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/anonforce/screenshots/13.png)

