## 9001
>*I made a simple machine for ALL my users, this is a secure setup right? Port: 9001*

![image](https://user-images.githubusercontent.com/59768512/155337571-062b0dec-a2ef-4300-9458-90bcd8cfc9d7.png)

![image](https://user-images.githubusercontent.com/59768512/155337634-9efdc87a-8f88-4786-a02c-7b3366bdd99e.png)

>**Flag:`IKT449{t00_much_p0wa}`**

## 9002
>*I got tired of telling people to GTFO when they asked for sudo permissions to install tools on their Linux machines. So I allowed APT to run as root on this machine. Should be fine right.. ? Port: 9002*

![image](https://user-images.githubusercontent.com/59768512/155338344-d5b9e046-c364-4bf5-a6e6-1233fc1ac327.png)

![image](https://user-images.githubusercontent.com/59768512/155338992-b9cff96c-e371-429a-afb0-937c7fc425ef.png)

![image](https://user-images.githubusercontent.com/59768512/155338250-24dc5d90-62de-4c1d-b4e6-a37bde593330.png)

>**Flag:`IKT449{APT_APT_APT_m000re_l11ke_ATP}`**

## 9003
>*I made myself a nice little task which provides a fresh timestamp for me every minute. Why? No clue. Port: 9003*

The task text mentiones a task that: "provides a fresh timestamp every minute". When scheduling task in a Linux enviroment a common method is using cronjobs. Looking into the crontab file `/etc/crontab` does not reveal any special tasks that are executed. 

![image](https://user-images.githubusercontent.com/59768512/155780901-9614fd96-3e83-4518-b7b3-91bedd21e6b4.png)

Looking through the different crontab folders (`cron.d`, `cron.daily`, `cron.hourly`, `cron.monthly`, `cron.yearly` and `cron.weekly`) shows that within the folder `/etc/cron.d` a interesting file called `autorun` is present. This file seems to be the one the task text referce to. Looking at the file it seem to be a cronjob that is executed every minute, where a file called `/home/user/autorun.sh` is executed. This file is also executed with sudo privileges as the owner is `root` meaning that any commands written in this file is executed will administrative privileges, which could be used to escalate privilege.

![image](https://user-images.githubusercontent.com/59768512/155783149-d7031e90-6c20-4e16-b296-0b16727913ae.png)

The `autorun.sh` file seems to be a script file which includes a not so interesting script that essential does what the task mentiones, however looking at the file permissions the owner of the file is the user `user` which means that we are able to rewrite the script being executed. 

![image](https://user-images.githubusercontent.com/59768512/155784259-e2d48e2d-c6dd-4ed8-8e73-81d51f68da70.png)

A simple way to escalate the privilege of the user account is simply adding the account to the sudoers file. Given that the user account can edit the `autorun.sh` file it can simply be done by running a couple of commands. Since there is no text editor on this machine, the echo command can be used instead using the commands `echo '#!/bin/bash' > autorun.sh` and `"echo 'user ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers"  >> autorun.sh`. The first command using the single greater than sign overwrites the file with the given string `#!/bin/bash` while the second command with two greater than signs appends ` user ALL=(ALL) NOPASSWD:ALL` to the end of the file. 

![image](https://user-images.githubusercontent.com/59768512/155784510-28f32f1f-9306-4f04-8533-8c151310140f.png)

This means that the next time the cronjob is executed, which at the latest happens one minute after editing the file, the user account will be granted sudoer rights on the machine. After waiting a little while the user can simply change privilege to root using the command `sudo su`.

![image](https://user-images.githubusercontent.com/59768512/155784713-cc22895e-0ef8-4ab7-bd92-9ee03c898504.png)

>**Flag:`IKT449{cron_what?_more_like_root_on_demand}`**

## 9004
>*Having a good backup of the home folder is quite important. Why would you not want all the files included? Port: 9004*

As with task 9003 the task text seems to point to cronjob being executed to backup files and as with the last task the `/etc/cron.d/` folder also contains a interesting file this time called `backup`.

![image](https://user-images.githubusercontent.com/59768512/155786815-cb1ed51c-af20-4c99-8e9b-09ed2158dc47.png)

Looking at this interesting file it seems to be running every half a minute and creating the `/tmp/files.bak` file and backup all the files within `/home/user` into to it.

![image](https://user-images.githubusercontent.com/59768512/155786966-e2061d6a-55f2-49de-84df-f631a1afc1a2.png)

Looking at the `/tmp/files.bak` confirms this theory.

![image](https://user-images.githubusercontent.com/59768512/155789582-93873aff-7aea-41ff-a459-bc58bcbfd02f.png)

Given that the cronjob will save files located in the `/home/user` directory it might be possible to perform a cron tar wildcard injection. A quick google search reveals that this is probably possible. 

![image](https://user-images.githubusercontent.com/59768512/157516165-0f7c34a4-7f8a-4f84-a94d-2e2ffde5f862.png)

Looking into this GitHub page the privelege escalation process seems fairly straight forward. One thing to note however is that a lot of the code that is being injected in this example is unessecary for a simple escalation to root and therefore only the line containing `echo 'echo "user ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers' > test.sh`, the two checkpoint lines and the create file for the archive.tar is necessary.

![image](https://user-images.githubusercontent.com/59768512/155793728-686e59fc-131f-4e65-8c5a-cdf8ecd484ec.png)

Adding the same lines to a file called test.sh, and running the checkpoint commands:

![image](https://user-images.githubusercontent.com/59768512/155794107-17a340fd-11af-46b5-9a63-5b899b2a945a.png)

And running the command `tar cf /tmp/archive.tar *`, means that the next time the `backup` cronjob is executed the user account will have root access.

![image](https://user-images.githubusercontent.com/59768512/155795186-f65967d2-b239-483b-9592-62ab64b50f9b.png)

While there is an error saying that file cannot be created because "permission is denied", the privlege escalation still works.

>**Flag:`IKT449{and_the_crowd_goes_WILD}`**

## 9005
>*I was told to run the file in my home directory, but it does not seem to do anything.. Do we have to provide some sort of ID or what? Port: 9005*

Looking into the files contained in the `/home/user` directory, the described called `runme` seems to be compiled C file from the `runme.c` file.

![image](https://user-images.githubusercontent.com/59768512/155546194-4a7e181e-a8ea-4d1f-897e-293fbeae690f.png)

Running the `runme` file seems to produce output that is similar to that off the `id` executable executed by a root user. This means that file is executed with `root` privileges and that either the `runme` file is a copy of the `id` executable or it calls the `id` executable within the code. If the latter is the case altering the path used by the shell could possible make it use a fabricated version of the `id` executable. 

![image](https://user-images.githubusercontent.com/59768512/155546461-5acfea81-5d2c-41bc-8ab0-5a66a0eb0640.png)

Before being able to create a "fake" `id` command a suitable writeable folder needs to be found where it can be created.

Running the command: `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u` finds writeable folders.

In this command the part `2>/dev/null` cleans error messages by putting errors into /dev/null which discards it. Secondly the `cut -d "/" -f 2,3` command cuts the the output using the delimiter `/` and keeps the fields 2 and 3. The `grep -v proc` part removes results related to processes that are uninteressting for this privelege escalation technique. Finally the `sort -u` part sorts the output numerically and alphabetical making it easier to look for suitible folders.

![image](https://user-images.githubusercontent.com/59768512/155546802-a04f101c-dfa8-4cac-bbca-9b85a6fa6184.png)

Looking at the output the `/tmp` folder seems like a perfect directory to create the fabricated file. In order for the shell to use the `/tmp` instead of the original location of the `id` executable the `/tmp` directory needs to be put at the start of the path. This can be done using the command `export PATH=/tmp:$PATH` which adds the `/tmp` folder to the start of the path and therefore when the id command is called by the user or by a program, in this scenario `runeme`, uses the first path rather than the using the actual `id` executable within the `/usr/bin` folder. 

![image](https://user-images.githubusercontent.com/59768512/157209027-939f5a47-8f51-4dec-98ef-1c2e3b9828ae.png)

In order to actually make the `runme` executable call a fabricated `id` executable a file called `id` needs to be created in the `/tmp` directory. Creatin the `id` file and make it execute `/bin/bash` means that when the `runme` executable file is executed it will run the fabricated id file and spawn a sudo bash shell as the `runme` command calls the `id` executable with sudoer privileges. Making the `id` file execute `/bin/bash` can be done by running the command `echo "/bin/bash" > id` and making it executable with the command `chmod 777 id`.

![image](https://user-images.githubusercontent.com/59768512/155548678-670ba5ce-fce1-4dc3-92f9-dc9e5c154a2c.png))

Running the `runme` executable, after having done the above steps grants the shell sudo rights.

![image](https://user-images.githubusercontent.com/59768512/155549244-e5176232-28d0-4393-b12d-6a351186ea6a.png)

>**Flag:`IKT449{show_me_your_id_please}`**


## 9007
>*I might be a bit dated, but please stop screaming at me for not keeping all the applications up to date.. OK ?*

The task text mentioned outdated applications. In order to get an idea of possible package that can be execute the command `find / -type f -perm -04000 -ls 2>/dev/null` was executed:

![image](https://user-images.githubusercontent.com/59768512/155718341-ae8009ee-23d8-4ca4-afc0-551d261a86d3.png)

The application `screen` is present, this might be the outdated application. Running the command `screen --version` shows that the application is running Screen GNU 4.05.00.

![image](https://user-images.githubusercontent.com/59768512/155718660-48dafc26-750a-4f6d-99ff-f4a8de03495d.png)

Looking online there seems to be a possible vulnerability in Screen 4.5.0. Especially the github page seems interesting.

![image](https://user-images.githubusercontent.com/59768512/155718913-95c099cd-ba1b-4276-9d86-f7359eaef656.png)

Looking into the github page a shellscript exploit is available and that infact Screen version 4.05.00 (GNU) is also vulnerable.

![image](https://user-images.githubusercontent.com/59768512/155719086-44a3b8b6-b36d-4b45-afdf-93bd5a9cb9cb.png)

On the machine the nano application is also available meaning that a copy of the `screen2root.sh` file can be created on the machine.

![image](https://user-images.githubusercontent.com/59768512/155719344-b99aee4e-47e3-45b6-9362-cfeb1eaa4726.png)

Making the shell script executable and executing it increases the privilege to root.

![image](https://user-images.githubusercontent.com/59768512/155719472-e658dbe6-a4df-4955-b833-96daf1dabe4b.png)


>**Flag:`IKT449{it_might_be_ugly_but_it_works}`**


