# crypto/baby
Author: EvilMuffinHa

I want to do an RSA!

## Solution
The description and the attached file gives us a good idea of what we want to do. Simply find/write a script that can decrypt text (c) encrypted by RSA having been provided with n and e.

I found a stack overflow solution using RsaCtfTool:
https://stackoverflow.com/questions/49878381/rsa-decryption-using-only-n-e-and-c

The correct command in your linux terminal would be:
```
./RsaCtfTool.py -n 228430203128652625114739053365339856393 -e 65537 --uncipher 126721104148692049427127809839057445790
```
*Note:* Make sure the command is typed correctly. I initially didn't get the flag because of this.

## Flag
```flag{68ab82df34}```
