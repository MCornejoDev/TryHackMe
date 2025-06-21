# Hacking your first machine

The first machine you will be attacking is a virtual machine.

The room is called [fakebank](https://tryhackme.com/room/offensivesecurityintro)

We are going to use gobuster application to find hidden directories and files in the target machine.

The command to run is:
```bash
gobuster dir -u {ip_target_machine} -w {wordlist} -t 100 -x php,txt,html
```

The command uses the following parameters:

- -u: The target machine IP
- -w: The wordlist to use
- -t: The number of threads to use
- -x: The extensions to search for

The output of the command has been the following:

```
=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://fakebank.thm/
[+] Threads      : 10
[+] Wordlist     : wordlist.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2025/06/05 08:30:18 Starting gobuster
=====================================================
/images (Status: 301)
/bank-transfer (Status: 200)
=====================================================
2025/06/05 08:30:32 Finished
=====================================================
```
We can see that the target machine has a hidden directory called /bank-transfer with a status code of 200.

If we go to the url http://fakebank.thm/bank-transfer we can see a page with a form to transfer money, if we put the following values:

```
- From: 1000
- To: 2000
- Amount: 100
```

We can see that the transfer was successful.