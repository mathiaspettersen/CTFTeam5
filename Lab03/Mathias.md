# Box 6

>*I was tired of every user complaining about not being able to log in to X and Y account, so I made the choice to let any user change any password. How? Well, that's something they will have to find out for themselves.*

The task given says it has "made the choice to let any user change any password". The Linux password system is built up of two files: `passwd` and `shadow`. The `passwd` file contains the users, their groups, their home directory, and which shell they are using. However, it does not contain the hash of the password to any of the users. The `shadow` file contains the same information as `passwd`, in addition to the hashed passwords of the users that are able to create a password. Generally, `passwd` is viewable, `shadow` is not viewable, and both are not writeable. If any of those are writeable, any user can add users and change groups of any user.

When further examining the files, it is apparent that `/etc/passwd` is writeable. This means we can add another root user to try to gain root privileges with another account:

![image](https://user-images.githubusercontent.com/70077872/155609450-a1a6e110-9dbd-42fe-9c00-4e16b2e3348f.png)

As also seen below, the root user is part of the `root` group which grants the user with superuser privileges. This means it has full controll over the machine and can perform any command it wants:

![image](https://user-images.githubusercontent.com/70077872/155610010-8fbb5c67-2455-41d7-a85a-f7d659898691.png)

To add another user, we firstly used the text editor `nano` to open the file, and editing it to include a `root2` user which has the exact same privileges as `root`, just with no passwd to begin with (hence there is no 'x' where other users have it):

![image](https://user-images.githubusercontent.com/70077872/155610378-ee58dba4-91d4-407c-87b3-8652cc020b80.png)

By saving the file and running `su root2`, we are prompted with the root account, and can collect the flag for this task:

![image](https://user-images.githubusercontent.com/70077872/155610553-17134f5c-afa8-48e8-a401-4a9ed9fc6c94.png)


>**Flag:`IKT449{permissions_are_hard}`** 



# Box 7

>*I might be a bit dated, but please stop screaming at me for not keeping all the applications up to date.. OK ?*

This box did not have any immediate low-hanging fruit for us to grab at, so we tried to look at which binaries we could run with root permissions. The command `find / -type f -perm -04000 -ls 2>/dev/null` is a command that searches for binaries that we can run with root permissions. The command searches from `/` (the top directory in Linux, allowing us to search for all files), `-type f` (searches for regular files), `-perm -04000` (shows the set-user-ID execute bits), and `2>/dev/null` (redirects error messages to show a cleaner output):

![bilde](https://user-images.githubusercontent.com/70077872/157316359-76747594-0248-464c-9fe5-1329456d1557.png)

