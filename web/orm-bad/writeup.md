# web/orm-bad
Author: ra

I just learned about orms today! They seem kinda difficult to implement though... Guess I'll stick to good old raw sql statements!

## Solution
The description right away tells us that this will be an sql injection challenge.

The main website is just a normal login screen.

Picture of main website: 




Let's try some simple sql injection payloads that are usually used to check sql injection everytime.

The payload that we are going to try is: admin' --

Then, a flag appears on the top of our screen!

## Flag
```flag{sqli_overused_again_0b4f6}```
