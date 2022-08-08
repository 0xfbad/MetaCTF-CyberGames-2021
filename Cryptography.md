# Cryptography - 10 challenges
[A to Z (100 pts)](#a-to-z-100-pts)<br>
[Thnks fr th Pwds (100 pts)](#thnks-fr-th-pwds-100-pts)<br>
[Wrong Way on a One Way Street (100 pts)](#wrong-way-on-a-one-way-street-100-pts)<br>
[Unbreakable Encryption (150 pts)](#unbreakable-encryption-150-pts)<br>
[Size Matters (175 pts)](#size-matters-175-pts)<br>
[Company Picnic (225 pts)](#company-picnic-225-pts) *no soln*<br>
[Ransomware Patch (250 pts)](#ransomware-patch-250-pts) *no soln*<br>
[Sugar, Spice, and Sweet1986Spice (425 pts)](#sugar-spice-and-sweet1986spice-425-pts) *no soln*<br>
[Two Key Crypto (525 pts)](#two-key-crypto-525-pts) *no soln*<br>
[Hide and Seek the Flag (700 pts)](#hide-and-seek-the-flag-700-pts) *no soln*<br>

## A to Z (100 pts)
> This encrypted flag will only require a simple substitution cipher to solve. Rearrange the letters from A to Z.
> 
> `yzhsufo_rh_nb_uze_wdziu`

Dump it into Cyberchef and add a substitution cipher operation, substituting `A-Z` with 'Z-A':

![Cyberchef](https://i.imgur.com/FdQo4m1.png)

Or alternatively you can use an Atbash cipher:

![Cyberchef](https://i.imgur.com/LjVZsr8.png)

<div align="center">

Flag:
```
bashful_is_my_fav_dwarf
```
[return to top](#top)</div>


## Thnks fr th Pwds (100 pts)
> On a red team engagement, you discover a text file on an administrator’s desktop with all of their passwords - you now have the keys to the kingdom!
> 
> During the engagement debrief, you explain what you found and how you were able to access so many systems. The administrator says that's impossible, because they encrypted all of the passwords in the file.
> 
> Here’s an example of one of their “encrypted” passwords: `TWV0YUNURntlbmNvZGluZ19pc19OMFRfdGhlX3NhbWVfYXNfZW5jcnlwdGlvbiEhfQ==`
> 
> See if you’re able to recover the Administrator's password.

The `==` hints at base64 padding, lets dump it into Cyberchef and add a base64 decode operation:

![Cyberchef](https://i.imgur.com/KeCotWB.png)

<div align="center">

Flag:
```
MetaCTF{encoding_is_N0T_the_same_as_encryption!!}
```
[return to top](#top)</div>


## Wrong Way on a One Way Street (100 pts)
> Hashing is a system by which information is encrypted such that it can never be decrypted... theoretically. Websites will often hash passwords so that if their passwords are ever leaked, bad actors won't actually learn the user's password; they'll just get an encrypted form of it. However, the same password will always hash to the same ciphertext, so if the attacker can guess your password, they can figure out the hash. Can you guess the password for this hash? `cb78e77e659c1648416cf5ac43fca4b65eeaefe1`

Since hashes are one way, you can either brute force them (hashing random strings and comparing them to the known hash) or you can search online databases for the hash. We'll do the latter:

Submitting the hash, we get:

![Hash](https://i.imgur.com/AeegSlW.png)

<div align="center">

Flag:
```
babyloka13
```
[return to top](#top)</div>


## Unbreakable Encryption (150 pts)
> There is a form of truly unbreakable encryption: the one time pad. Nobody, not Russia, not China, and not even Steve, who lives in his mom's basement and hacks governments for fun, can decrypt anything using this cipher... as long as it's used correctly. In this scheme, a truly random string as long as the plaintext is chosen, and the ciphertext is computed as the bitwise XOR of the plaintext and the key. However, if the key is reused even once, it can be cracked. We've intercepted some messages between some criminals, and we're hoping you could crack the one time pad they used. We're pretty sure they reused it, so you should be able to crack it...
> 
> Ciphertext 1: `4fd098298db95b7f1bc205b0a6d8ac15f1f821d72fbfa979d1c2148a24feaafdee8d3108e8ce29c3ce1291`
> 
> Plaintext 1: `hey let's rob the bank at midnight tonight!`
> 
> Ciphertext 2: `41d9806ec1b55c78258703be87ac9e06edb7369133b1d67ac0960d8632cfb7f2e7974e0ff3c536c1871b`

We can write a python script to crack the one time pad:

```py
# Initial problem
cipher1 = "4fd098298db95b7f1bc205b0a6d8ac15f1f821d72fbfa979d1c2148a24feaafdee8d3108e8ce29c3ce1291"
plaintext1 = "hey let's rob the bank at midnight tonight!"

cipher2 = "41d9806ec1b55c78258703be87ac9e06edb7369133b1d67ac0960d8632cfb7f2e7974e0ff3c536c1871b"
plaintext2 = ""


# Get bytes from cipher
cipher1 = [int(cipher1[i:i+2], 16) for i in range(0, len(cipher1), 2)]
cipher2 = [int(cipher2[i:i+2], 16) for i in range(0, len(cipher2), 2)]

# Convert plaintext to unicode
plaintext1 = [ord(char) for char in plaintext1]


# Get key from cipher1
key = [cipher1[byte] ^ plaintext1[byte] for byte in range(len(cipher1))] # xor cipher1 with plaintext1

# Using key, get plaintext2
plaintext2 = [key[byte] ^ cipher2[byte] for byte in range(len(cipher2))] # xor cipher2 with key



print("".join([chr(char) for char in plaintext2]))
```

Lets run it:

```
$ python solver.py
flag is MetaCTF{you're_better_than_steve!}
```

<div align="center">

Flag:
```
MetaCTF{you're_better_than_steve!}
```
[return to top](#top)</div>


## Size Matters (175 pts)
> RSA is a public key cryptosystem, where one can encrypt a message with one key and decrypt it with another. We've intercepted a secure message encrypted with RSA, as well as the key used to encrypt it. Since RSA keys have to be pretty big in order to be secure, we're pretty sure you can break this one. Give it a shot!
> 
> Ciphertext: `0x2526512a4abf23fca755defc497b9ab`
> e: `257`
> n: `0x592f144c0aeac50bdf57cf6a6a6e135`

There's a few ways to approach this problem, the first is just using an online tool like [dcode.fr](https://www.dcode.fr/rsa-cipher):

![dcode](https://i.imgur.com/TDEXtQ6.png)

Or we can make a quick python script to decode the message:

```py
from Cryptodome.Util.number import long_to_bytes, inverse

# Initial problem
cipherMessage = 0x2526512a4abf23fca755defc497b9ab
e = 257
n = 0x592f144c0aeac50bdf57cf6a6a6e135


# Get factors of n (used factordb.com because factorization at this scale takes forever)
p = 430535396861370041
q = 17209058493553260637

# Get phi, the totient of n (basically the number of numbers coprime to n)
phi = (p - 1) * (q - 1)

# Get d, the private key (inverse of e mod phi)
privateKey = inverse(e, phi)

# Get plaintext (creates long int from ciphertext)
plaintext = pow(cipherMessage, privateKey, n)

# Convert long int to byte string
plaintext = long_to_bytes(plaintext)


print(plaintext.decode('utf-8'))
```

Lets run it:

```
$ python solver.py
you_broke_rsa!
```

<div align="center">

Flag:
```
you_broke_rsa!
```
[return to top](#top)</div>


## Company Picnic (225 pts)
> My boss asked me to prepare asymmetric keys for each of our employees to communicate securely about our top-secret company picnic. I used a very secure and random process to generate these keys, so I'm sure it's safe to publish these public keys. Take a look!
> 
> Files: [public_keys.txt](https://metaproblems.com/2c4d19e43f1a8c225fcd413bdebeaea2/public_keys.txt)
> 
> Convert any private keys you can find to ascii, then concatenate them

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Ransomware Patch (250 pts)
> You've captured a communication containing a patch for the source code of a well-known ransomware program. It contains an update for a library the program uses, as well as an interesting file named `key`. Can you crack [this ZIP](https://metaproblems.com/f807f1b6beeecc351ab76d1353e403e8/ransomware-final.zip) and figure out the contents of `key`?
> 
> **made with 7ZIP deflate on "Normal" settings*

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Sugar, Spice, and Sweet1986Spice (425 pts)
> Most passwords never make it into a rainbow table, and on top of that, secure applications "salt" their hashes; that is, the password is combined with some random string before it's hashed, so even if the password is in a rainbow table, the salted hash won't be. However, rainbow tables aren't the only attack on hashed passwords; attackers can make custom word lists to try cracking passwords themselves
> 
> We've recovered the hashed password of a criminal that we know really like cats, cupcakes, and DC comics. She was born in 1978, and her favorite color is yellow.
> 
> Password Hash: `595577b8e8dce491e9b0680f66aa6e065f3df086`
> 
> Hint: DC comics is a dead end... take a closer look at the problem title: `Sugar, Spice, and Sweet1986Spice`

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Two Key Crypto (525 pts)
> I've created a crypto service that requires two keys to encrypt or decrypt any message (fancy, right?). I'm pretty sure that regardless of what key one person gives it, without the other, no message can be decrypted. To demonstrate this, I've set up a server that will encrypt a secret message with a server key and a client key you can specify. I bet you can't decrypt the message!
> 
> Connect with `nc host.cg21.metaproblems.com 3370`
> 
> [Downloads](https://metaproblems.com/cc98d47dedf2dc7e4c7384f40e978151/two_key.zip)
> 
> Hint: you're not going to solve this by breaking the crypto itself (I mean, you might, but it'd be very hard); instead, try thinking outside of the box and coming up with other ways you can attack this.

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>


## Hide and Seek the Flag (700 pts)
> Capture the Flag competitions are fun and all, but some people prefer the much more laid back "Hide and Seek the Flag." We managed to acquire a pcap of two people playing a round of Hide and Seek the Flag over a custom encryption system, both available [here](https://metaproblems.com/97841bc03dd32d67a61f958d8c09b023/hide_and_seek_the_flag.zip). Unfortunately, we only managed to record the traffic on one end, and we don't have the decryption keys for the traffic they were sending.
> 
> As a Capture the Flag expert, surely you have the skills needed to figure out what they were saying!

<div align="center">

Flag:
```
NOT SOLVED YET
```
[return to top](#top)</div>
