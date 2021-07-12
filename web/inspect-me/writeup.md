# web/inspect-me
Author: NotDeGhost

See if you can find the flag in the source code!

https://inspect-me.mc.ax/

## Solution
The description and the title of the challenge gives us a good idea of where to start.
Also this is a usual place to start in for a web challenge.

The main website is basically just there to try to crash your computer and keep you away from doing the challenge.

Picture of main website: 

<img src="https://user-images.githubusercontent.com/86171033/125229052-c990b400-e28a-11eb-80d9-06a5b21c39e5.png" alt="image" width="800"/>


Look into the source code and look for flag.

The flag format is flag{}, so use Ctrl + F, and search for flag

this appears in the comments of the html:

```<!-- flag{inspect_me_like_123} -->```

## Flag
```flag{inspect_me_like_123}```

