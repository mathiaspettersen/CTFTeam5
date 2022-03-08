# Box 6

>*I was tired of every user complaining about not being able to log in to X and Y account, 
>so I made the choice to let any user change any password. How? Well, that's something they will have to find out for themselves.*

The task given says it has "made the choice to let any user change any password". The Linux password system if built up of two files: 

/etc/passwd file is writeable:

![image](https://user-images.githubusercontent.com/70077872/155609450-a1a6e110-9dbd-42fe-9c00-4e16b2e3348f.png)

/etc/passwd:

![image](https://user-images.githubusercontent.com/70077872/155610010-8fbb5c67-2455-41d7-a85a-f7d659898691.png)

Add root2 with the same credentials as root within /etc/passwd:

![image](https://user-images.githubusercontent.com/70077872/155610378-ee58dba4-91d4-407c-87b3-8652cc020b80.png)

Switch to root2 and get the flag:

![image](https://user-images.githubusercontent.com/70077872/155610553-17134f5c-afa8-48e8-a401-4a9ed9fc6c94.png)






# Box 7

>*I might be a bit dated, but please stop screaming at me for not keeping all the applications up to date.. OK ?*

sudo version may be vulnerable. ran linpeas.sh:

![image](https://user-images.githubusercontent.com/70077872/155531464-cdabc693-da0b-4488-89bb-f611a8c50a43.png)
