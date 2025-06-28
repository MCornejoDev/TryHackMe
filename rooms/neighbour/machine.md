- Let’s view the room’s webpage and we see the following:

    ![web-page](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/00.png)

- We open the robots.txt and find:

    ![robots.txt](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/01.png)

- If we run a dirbuster or gobuster, we see the following:

    ![dirbuster](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/02.png)

- I also used dirsearch but got the same results.

    ![dirsearch](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/03.png)

- Note: there is a db.php

- Could it be a command execution (cmd)?

    ![db.php](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/04.png)

- We use nmap and get the following open ports:

    ![nmap](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/05.png)

- It was much easier than expected.

    ![flag](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/neighbour/screenshots/06.png)
