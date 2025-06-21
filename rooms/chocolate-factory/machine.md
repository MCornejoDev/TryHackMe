# Writeup for Room "Chocolate Factory"

This is the writeup for the box Chocolate Factory

## 🔍 Enumeration Phase

### 1. WebSite
- The website.
- No useful information was found in `robots.txt`.
  
![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/00.png)

![Robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/01.png)

### 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files but no useful information was found.

![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/02.png)

## 📡 3. Nmap Scan
- The following information was found:
  - The port 21 is open. **FTP**
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**
  - ...

![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/03.png)

## 4. SQL Injection 

"I tried to find some kind of SQL Injection vulnerability, but found nothing."

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

The content of the file has a part that looks like a key:

![hexdump](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/09.png)

I made a copy of the file in .txt format and used the command ```cat key_rev_key.txt``` to get the first flag, which is the key

![cp-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/10.png)

![cat-content](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/chocolate-factory/screenshots/11.png)

