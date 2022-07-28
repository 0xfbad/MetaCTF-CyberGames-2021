# Other - 7 challenges
[Flag Format (50 pts)](#flag-format-50-pts)<br>
[Interception I (100 pts)](#interception-i-100-pts) *no soln*<br>
[This Ain't a Scene, It's an Encryption Race (100 pts)](#this-aint-a-scene-its-an-encryption-race-100-pts) *no soln*<br>
[Interception II (150 pts)](#interception-ii-150-pts) *no soln*<br>
[MetaCTF CyberGames 2021 Feedback (200 pts)](#metactf-cybergames-2021-feedback-200-pts) *no soln*<br>
[Interception III (275 pts)](#interception-iii-275-pts) *no soln*<br>
[Step into the NET (600 pts)](#step-into-the-net-600-pts) *no soln*<br>

## Flag Format (50 pts)
> Most of our flags are formatted like this: MetaCTF{string_separated_with_und3rscores}
>
> If the flag is not in that format, we specify what the flag format should be instead. If you solve this challenge, make sure to tell your teammates about the flag format!

Self explanatory.

<div align="center">

Flag:
```
MetaCTF{string_separated_with_und3rscores}
```
[return to top](#top)</div>


## Interception I (100 pts)
> 192.168.0.1 is periodically (once every 4 seconds) sending the flag to 192.168.0.2 over UDP port 8000. Go get it.<br>
> `ssh ctf-46ed3559da08@host.cg21.metaproblems.com -p 7000`
> 
> _If you get an SSH host key error, consider using_<br>
> `ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" ctf-46ed3559da08@host.cg21.metaproblems.com -p 7000`
> 
> Note that the connection can take a while to initialize. It will say `Granting console connection to device...` and then three dots will appear. After the third dot you should have a connection.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## This Ain't a Scene, It's an Encryption Race (100 pts)
> Ransomware attacks continue to negatively impact businesses around the world. What is the Mitre ATT&CK technique ID for the encryption of data in an environment to disrupt business operations?
> 
> The flag format will be `T####`.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Interception II (150 pts)
> Someone on this network is periodically sending the flag to ... someone else on this network, over TCP port 8000. Go get it.<br>
`ssh ctf-f36ef72cadc1@host.cg21.metaproblems.com -p 7000`

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## MetaCTF CyberGames 2021 Feedback (200 pts)
> Submit your [feedback](https://compete.metactf.com) for some points! We use your feedback to make our CTFs better in the future.
>
> Please ask everyone on your team to submit it.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Interception III (275 pts)
> 192.168.55.3 is periodically sending the flag to 172.16.0.2 over UDP port 8000. Go get it.<br>
> By the way, I've been told the admins at this organization use really shoddy passwords.<br>
> `ssh ctf-2a89377260b6@host.cg21.metaproblems.com -p 7000`<br>
> *Note:* The password for this user is the flag from Interception I. You must finish Interception I before starting this challenge. 

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Step into the NET (600 pts)
> For the grand finale, it's time to put all of your analysis skills together. You're going to face a little bit of Crypto & Reverse Engineering and a whole lot of Forensics.
> 
> We've taken the liberty of collecting [a packet capture](https://metaproblems.com/5450f756cf49545a1061e7d28ee45d1a/step_into_the_net.pcapng) and [a process dump](https://metaproblems.com/5450f756cf49545a1061e7d28ee45d1a/step_into_the_net.7z) of the beacon in question for you.
> 
> In order give our management the clarity they need, we want to know *exactly* what actions the attackers took on this high value machine (note - this is a different memory dump/pcap from any previous problem). In order to do this, you're going to want to match the information in the pcap and extract critical data you need from the memory dump to put together a picture of what happened.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>
