- The machine is not a real-time machine with an IP, but rather a file that you download, and the title of the machine gives you a hint: "OhSINT".

- The file in question:

    ![file](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/00.png)

- I will start by performing steganography on the file using different tools:

- Exiftool

- If we look at the Copyright section, there is a name

    ![copyright](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/01.png)

- We searched for it on the internet

    ![search](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/02.png)

- And we found the first flag, which is the person's avatar: cat

- The second flag was found on a GitHub shared by the person in question: London

    ![github](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/03.png)

- The third flag is found using the BSSID provided by the Twitter account and using winglet.net, zooming in as much as possible, and the answer is: UnileverWifi

    ![winglet](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/04.png)

    ![zoom](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/05.png)

- The fourth flag is the person's email address: OWoodflint@gmail.com
  
- The fifth flag is the place where we found the email and where this person is located: GitHub

- The sixth flag is where this person is on vacation, which is New York, since they have a WordPress site where they say they will update the site

    ![vacation](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/06.png)

- I was able to find the password in the source code by searching the website on Google:

    ![google](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/07.png)

- And I decided to look at the source code and ta-da!!

    ![source](https://github.com/MCornejoDev/TryHackMe/blob/main/rooms/ohsint/screenshots/08.png)