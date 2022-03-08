## 9001 - ALL

## 9002 - GTFO

## 9003 - Date check

## 9004 - Back me up

## 9005 - Got ID?

## 9006 - Easy Access

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



## 9007 - SCREAM

>*I might be a bit dated, but please stop screaming at me for not keeping all the applications up to date.. OK ?*

This box did not have any immediate low-hanging fruit for us to grab at, so we tried to look at which binaries we could run with root permissions. The command `find / -type f -perm -04000 -ls 2>/dev/null` is a command that searches for binaries that we can run with root permissions. The command searches from `/` (the top directory in Linux, allowing us to search for all files), `-type f` (searches for regular files), `-perm -04000` (shows the set-user-ID execute bits), and `2>/dev/null` (redirects error messages to show a cleaner output).

The command showed some different binaries. While it shows many binaries, many of them do not have SUID exploits, or do not lead to a direct privilege escalation for the regular user. The most interesting binary was the `screen` binary, which stands out from the regular binaries that usually are shown when running the `find` command. The name also looks similar to the task name, which is `scream`:

![bilde](https://user-images.githubusercontent.com/70077872/157317198-109ae957-c06a-4fb6-b346-a9950079150a.png)

Firstly, we tried to run `screen` with sudo, which did not work as user is not in the sudoers file:

![bilde](https://user-images.githubusercontent.com/70077872/157318118-7f08b927-fb5b-49d0-a971-1519b41a6eb4.png)

After taking a closer look at screen, we can try to view the version number of it to see if there are any active exploits for it. After looking at the version, it is apparent that it is `Screen version 4.05.00`:

![bilde](https://user-images.githubusercontent.com/70077872/157317818-70b52d45-5d6d-48c7-9d85-93957a493bae.png)

A simple Google search for screen with the version number got a hit in `exploit-db` which is a vulnerability database:

![bilde](https://user-images.githubusercontent.com/70077872/157318559-81569aad-ce1f-4191-85ca-aea32dc4b758.png)

The website shows a local privilege escalation exploit that the regular user runs. This script creates a shell file and removes the dependency file `/etc/ld.so.preload`, and creates its own `ld.so.preload` and inserts the shellcode into there. Then, when `screen` is run, it runs with the SUID bit set, allowing us to elevate our privileges.

![bilde](https://user-images.githubusercontent.com/70077872/157318710-1f2f9bc2-9007-463d-86c8-5bdd452a80d6.png)

Then, when the exploit is run, we get a `root` shell on the machine:

![bilde](https://user-images.githubusercontent.com/70077872/157319878-f56a2af7-b9b8-4e19-ad41-59c6962df05f.png)

This also allows us to collect the flag we need:

![bilde](https://user-images.githubusercontent.com/70077872/157320034-b7179635-618b-4660-8ce3-d2035ec3a6d7.png)

>**Flag:`IKT449{it_might_be_ugly_but_it_works}`**

