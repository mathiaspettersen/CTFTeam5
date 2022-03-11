## 9001 - ALL
I made a simple machine for ALL my users, this is a secure setup right? Port: 9001

![lab5 1](https://user-images.githubusercontent.com/46780028/157767743-969b7fc9-eafb-44c4-bd2f-e1a524853bdc.png)

Firstly, to find out what kind of priveleges that currently exist on user "user". To do this, we run the command `sudo -1` and then enter the password for the user. Then, it lists that user "user" has acsess to all possiblle commands. This is proved by the output  `(ALL : ALL) ALL` as a response to the given command.

![lab 5 2](https://user-images.githubusercontent.com/46780028/157767781-90d5a581-5131-4efd-baf5-b866bc06246d.png)

This enables user "user" to switch user to "root", a higher priveleged user which is the system administrator. To do this, we run the command `sudo su`. To confirm that the "root" user has been acsessed, we can run the `ls` command and see which files that can now be acsessed after the switch. The flag can be found within the `root.txt` file. 

Flag:IKT449{t00_much_p0wa}**

## 9005 - Got ID?

## 9003 - Date check

>*I made myself a nice little task which provides a fresh timestamp for me every minute. Why? No clue. Port: 9003*

The task text mentions a task that: "provides a fresh timestamp every minute". When scheduling tasks in a Linux environment a common method is using cronjobs. Looking into the crontab file `/etc/crontab` does not reveal any special tasks that are executed. 

![image](https://user-images.githubusercontent.com/59768512/155780901-9614fd96-3e83-4518-b7b3-91bedd21e6b4.png)

Looking through the different crontab folders (`cron.d`, `cron.daily`, `cron.hourly`, `cron.monthly`, `cron.yearly` and `cron.weekly`) shows that within the folder `/etc/cron.d` a interesting file called `autorun` is present. This file seems to be the one the task text refers to. Looking at the file it seems to be a cronjob that is executed every minute, where a file called `/home/user/autorun.sh` is executed. This file is also executed with sudo privileges as the owner is `root` meaning that any commands written in this file are executed with administrative privileges, which could be used to escalate privilege.

![image](https://user-images.githubusercontent.com/59768512/155783149-d7031e90-6c20-4e16-b296-0b16727913ae.png)

The `autorun.sh` file seems to be a script file that includes a not so interesting script that essentially does what the task mentioned, however looking at the file permissions the owner of the file is the user `user` which means that we can rewrite the script being executed. 

![image](https://user-images.githubusercontent.com/59768512/155784259-e2d48e2d-c6dd-4ed8-8e73-81d51f68da70.png)

A simple way to escalate the privilege of the user account is simply adding the account to the sudoers file. Given that the user account can edit the `autorun.sh` file it can simply be done by running a couple of commands. Since there is no text editor on this machine, the echo command can be used instead using the commands `echo '#!/bin/bash' > autorun.sh` and `"echo 'user ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers"  >> autorun.sh`. The first command using the single greater than sign overwrites the file with the given string `#!/bin/bash` while the second command with two greater than signs appends ` user ALL=(ALL) NOPASSWD:ALL` to the end of the file. 

![image](https://user-images.githubusercontent.com/59768512/155784510-28f32f1f-9306-4f04-8533-8c151310140f.png)

This means that the next time the cronjob is executed, which at the latest happens one minute after editing the file, the user account will be granted sudoer rights on the machine. After waiting for a little while the user can simply change privilege to root using the command `sudo su`.

![image](https://user-images.githubusercontent.com/59768512/155784713-cc22895e-0ef8-4ab7-bd92-9ee03c898504.png)

>**Flag:`IKT449{cron_what?_more_like_root_on_demand}`**

## 9004 - Back me up
>*Having a good backup of the home folder is quite important. Why would you not want all the files included? Port: 9004*

As with task 9003, the task text seems to point to cronjob being executed to backup files and as with the last task the `/etc/cron.d/` folder also contains an interesting file this time called `backup`.

![image](https://user-images.githubusercontent.com/59768512/155786815-cb1ed51c-af20-4c99-8e9b-09ed2158dc47.png)

Looking at this interesting file it seems to be running every half a minute and creating the `/tmp/files.bak` file and backup all the files within `/home/user` into it.

![image](https://user-images.githubusercontent.com/59768512/155786966-e2061d6a-55f2-49de-84df-f631a1afc1a2.png)

Looking at the `/tmp/files.bak` confirms this theory.

![image](https://user-images.githubusercontent.com/59768512/155789582-93873aff-7aea-41ff-a459-bc58bcbfd02f.png)

Given that the cronjob will save files located in the `/home/user` directory it might be possible to perform a cron tar wildcard injection. A quick google search reveals that this is probably possible. 

![image](https://user-images.githubusercontent.com/59768512/157516165-0f7c34a4-7f8a-4f84-a94d-2e2ffde5f862.png)

Looking into this GitHub page the privilege escalation process seems fairly straightforward. One thing to note however is that a lot of the code that is being injected in this example is unnecessary for a simple escalation to root and therefore only the line containing `echo 'echo "user ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers' > test.sh`, the two checkpoint lines and the created file for the archive.tar is necessary.

![image](https://user-images.githubusercontent.com/59768512/155793728-686e59fc-131f-4e65-8c5a-cdf8ecd484ec.png)

Adding the same lines to a file called test.sh, and running the checkpoint commands:

![image](https://user-images.githubusercontent.com/59768512/155794107-17a340fd-11af-46b5-9a63-5b899b2a945a.png)

And running the command `tar cf /tmp/archive.tar *`, means that the next time the `backup` cronjob is executed the user account will have root access.

![image](https://user-images.githubusercontent.com/59768512/155795186-f65967d2-b239-483b-9592-62ab64b50f9b.png)

While there is an error saying that the file cannot be created because "permission is denied", the privilege escalation still works.

>**Flag:`IKT449{and_the_crowd_goes_WILD}`**

## 9005 - Got ID?
>*I was told to run the file in my home directory, but it does not seem to do anything.. Do we have to provide some sort of ID or what? Port: 9005*

Looking into the files contained in the `/home/user` directory, the described called `runme` seems to be compiled C file from the `runme.c` file.

![image](https://user-images.githubusercontent.com/59768512/155546194-4a7e181e-a8ea-4d1f-897e-293fbeae690f.png)

Running the `runme` file seems to produce output that is similar to that of the `id` executable executed by a root user. This means that the file is executed with `root` privileges and that either the `runme` file is a copy of the `id` executable or it calls the `id` executable within the code. If the latter is the case altering the path used by the shell could possibly make it use a fabricated version of the `id` executable. 

![image](https://user-images.githubusercontent.com/59768512/155546461-5acfea81-5d2c-41bc-8ab0-5a66a0eb0640.png)

Before being able to create a "fake" `id` command a suitable writeable folder needs to be found where it can be created.

Running the command: `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u` finds writeable folders.

In this command, the part `2>/dev/null` cleans error messages by putting errors into /dev/null which discards it. Secondly, the `cut -d "/" -f 2,3` command cuts the output using the delimiter `/` and keeps the fields 2 and 3. The `grep -v proc` part removes results related to processes that are uninteresting for this privilege escalation technique. Finally the `sort -u` part sorts the output numerically and alphabetically making it easier to look for suitable folders.

![image](https://user-images.githubusercontent.com/59768512/155546802-a04f101c-dfa8-4cac-bbca-9b85a6fa6184.png)

Looking at the output the `/tmp` folder seems like a perfect directory to create the fabricated file. For the shell to use the `/tmp` instead of the original location of the `id` executable the `/tmp` directory needs to be put at the start of the path. This can be done using the command `export PATH=/tmp:$PATH` which adds the `/tmp` folder to the start of the path and therefore when the id command is called by the user or by a program, in this scenario `runeme`, uses the first path rather than the using the actual `id` executable within the `/usr/bin` folder. 

![image](https://user-images.githubusercontent.com/59768512/157209027-939f5a47-8f51-4dec-98ef-1c2e3b9828ae.png)

To actually make the `runme` executable call a fabricated `id` executable, a file called `id` needs to be created in the `/tmp` directory. Creatin the `id` file and making it execute `/bin/bash` means that when the `runme` executable file is executed it will run the fabricated id file and spawn a sudo bash shell as the `runme` command calls the `id` executable with sudoer privileges. Making the `id` file execute `/bin/bash` can be done by running the command `echo "/bin/bash" > id` and make it executable with the command `chmod 777 id`.

![image](https://user-images.githubusercontent.com/59768512/155548678-670ba5ce-fce1-4dc3-92f9-dc9e5c154a2c.png))

Running the `runme` executable, after having done the above steps grants the shell sudo rights.

![image](https://user-images.githubusercontent.com/59768512/155549244-e5176232-28d0-4393-b12d-6a351186ea6a.png)

>**Flag:`IKT449{show_me_your_id_please}`**

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

The command showed some different binaries. While it shows many binaries, many of them do not have SUID exploits, or do not lead to a direct privilege escalation for the regular user. The most interesting binary was the `screen` binary, which stands out from the regular binaries that usually are shown when running the `find` command. The name also looks similar to the task name, which is `SCREAM`:

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

