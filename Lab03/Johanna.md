
## 9002
When running `sudo -l` , it is discovered that the user may run `apt` as root, as also written in the description of the challenge. 

![sudo_l](https://user-images.githubusercontent.com/72946914/157649370-a624e621-e187-449b-8090-fdd071b44cbc.PNG)

The challenge title was *GTFO*; thus, searching for *GTFO apt priv esc* led us to the website [GTFOBins](https://gtfobins.github.io/), with the description " (...) a curated list of Unix binaries that can be used to bypass local security restrictions in misconfigured systems." Hence, it seemed like we were at the right place. 

Then, we searched for apt, which we knew had root privileges. 

![image](https://user-images.githubusercontent.com/72946914/157649540-edecc1b7-aafc-4e15-b143-700d1f53e688.png)

The site provides three different ways of achieving root access, in which the two first options require the machine to be connected to the Internet. As these will not work, we got along with the last one: ``sudo apt update -o APT::Update::Pre-Invoke::=/bin/sh ``. This command gave us access to the root user, and we were able to access the flag in root/root.txt. 

![image](https://user-images.githubusercontent.com/72946914/157650681-5bde2385-3901-4848-90eb-2e85b43d3810.png)

>**Flag:`IKT449{APT_APT_APT_m000re_l11ke_ATP}`**

