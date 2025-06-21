# Writeup for Room "Gotta Catch'em All!"

This is the writeup for the box Gotta Catch'em All!

## 🔍 Enumeration Phase

### 1. WebSite
- The website is running on Apache.
- No useful information was found in `robots.txt`.

![WebSite](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/00.png)

### 2. Fuzzing
-  **Dirbuster / Gobuster** was used to find directories and files but no useful information was found.

![Fuzzing](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/01.png)

### 3. Web Console
- The web console was opened and we found the following information:

![Web Console](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/02.png)

- In addition, the following tag was found in the DOM:
  ```
  <pokemon>:
    <hack_the_pokemon><hack_the_pokemon>
  </pokemon>
  ```

## 4. Nmap Scan
- The following information was found:
  - The port 22 is open. **SSH**
  - The port 80 is open. **HTTP (Apache)**

![Nmap Scan](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/03.png)

## 5. SSH Access
- After wasting some time with the script we saw in the console, I decided to use the following credentials for try a ssh connection:
```
Username: pokemon
Password: hack_the_pokemon
```

![SSH Access](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/04.png)


- After loggin in, I did a ls -la and saw that the following files were present: ```P0kEm0n.zip```

- I extracted the zip file and saw that the following files were present: ```grass-type.txt``` and the content of the file was a hex string.

![P0kEm0n.zip](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/05.png)
  
- If I convert the hex string to ascii, I got the first flag: ```PoKeMon{Bulbasaur}```

![first-flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/gotta-catch'em-all!/screenshots/06.png)