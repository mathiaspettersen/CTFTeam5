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

The task text mentiones a "which provides a fresh timestamp for me every minute" this sounds like a cronjob. Looking into the crontab file `/etc/crontab` does not reveal any special tasks that are executed. 

![image](https://user-images.githubusercontent.com/59768512/155780901-9614fd96-3e83-4518-b7b3-91bedd21e6b4.png)

Looking through the different crontab folders (`cron.d`, `cron.daily`, `cron.hourly`, `cron.monthly`, `cron.yearly` and `cron.weekly`) shows that within the folder `/etc/cron.d` a interesting file called `autorun` is present. This file seems to be the one the task text referce to. Looking at the file it seem to be a cronjob that is executed every minute, where a file called `/home/user/autorun.sh` is executed, this file is also executed with sudo privileges as the owner is `root`.

![image](https://user-images.githubusercontent.com/59768512/155783149-d7031e90-6c20-4e16-b296-0b16727913ae.png)

The `autorun.sh` file seems to be a script file which includes a not so interesting script, however looking at the file permissions the owner of the file is the user `user` which means that we are able to rewrite the script being executed. 

![image](https://user-images.githubusercontent.com/59768512/155784259-e2d48e2d-c6dd-4ed8-8e73-81d51f68da70.png)

A simple way to escalate the privilege of the user account is simply adding the account to the sudoers file. Given that the user account can edit the `autorun.sh` file it can simply be done by executing the commands:

![image](https://user-images.githubusercontent.com/59768512/155784510-28f32f1f-9306-4f04-8533-8c151310140f.png)

This means that the next time the cronjob is executed, which at the latest happens one minute after editing the file, the user account will be granted sudoer rights on the machine. After waiting a little while the user can simply change privilege to root using the command `sudo su`.

![image](https://user-images.githubusercontent.com/59768512/155784713-cc22895e-0ef8-4ab7-bd92-9ee03c898504.png)

>**Flag:`IKT449{cron_what?_more_like_root_on_demand}`**

## 9004
>*Having a good backup of the home folder is quite important. Why would you not want all the files included? Port: 9004*

As with task 9003 the task text seems to point to cronjob being executed to backup files. As with the last task the `/etc/cron.d/` folder contains a interesting file called `backup`.

![image](https://user-images.githubusercontent.com/59768512/155786815-cb1ed51c-af20-4c99-8e9b-09ed2158dc47.png)

Looking at this interesting file it seems to be running every half a minute and creating the `/tmp/files.bak` file and all the files within`/home/user` backup'ed into to it.

![image](https://user-images.githubusercontent.com/59768512/155786966-e2061d6a-55f2-49de-84df-f631a1afc1a2.png)

Looking at the `/tmp/files.bak` confirms this theory.

![image](https://user-images.githubusercontent.com/59768512/155789582-93873aff-7aea-41ff-a459-bc58bcbfd02f.png)

Given that the cronjob will save files located in the `/home/user` directory it might be possible to perform a cronob tar wildcard injection. A quick google search reveals that this is probably possible. 

![image](https://user-images.githubusercontent.com/59768512/155792282-d9c5f99f-8c07-478b-8dca-3bbf6e9b53dc.png)

Looking into this GitHub page the privelege escalation process seems fairly straight forward. One thing to note however is that a lot of the code that is being injected in this example is unessecary for a simple escalation to root and therefore only the line containing `echo 'echo "user ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers' > test.sh`, the two checkpoint lines and the create file for the archive.tar is necessary.

![image](https://user-images.githubusercontent.com/59768512/155793728-686e59fc-131f-4e65-8c5a-cdf8ecd484ec.png)

Simply adding the same lines to a file called test.sh:

![image](https://user-images.githubusercontent.com/59768512/155794107-17a340fd-11af-46b5-9a63-5b899b2a945a.png)

And running the command `tar cf /tmp/archive.tar *`, means that the next time the `backup` cronjob is executed the user account will have root access.

![image](https://user-images.githubusercontent.com/59768512/155795186-f65967d2-b239-483b-9592-62ab64b50f9b.png)

>**Flag:`IKT449{and_the_crowd_goes_WILD}`**

## 9005
>*I was told to run the file in my home directory, but it does not seem to do anything.. Do we have to provide some sort of ID or what? Port: 9005*

![image](https://user-images.githubusercontent.com/59768512/155546194-4a7e181e-a8ea-4d1f-897e-293fbeae690f.png)

Run runme seems to execute the id command.

![image](https://user-images.githubusercontent.com/59768512/155546461-5acfea81-5d2c-41bc-8ab0-5a66a0eb0640.png)

Altering the path used by the `runme` executable might trick it into running a fabricated version of the id command


Running the command: `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u` finds writeable folders.
`2>/dev/null` cleans error messages
`cut -d "/" -f 2,3` 
`grep -v proc` removes results related to processes
`sort -u` sorts numerically and alphabetical

![image](https://user-images.githubusercontent.com/59768512/155546802-a04f101c-dfa8-4cac-bbca-9b85a6fa6184.png)

Adding the writeable folder `/tmp` to the path means that executable files will use programs present in that folder using the command export PATH=/tmp:$PATH



Adding a altered `id` file and make it execute `/bin/bash` means that when the `runme` executable file is executed it will run the fabricated id file and spawn a sudo bash shell.
`echo "/bin/bash" > id`
`chmod 777 id`

![image](https://user-images.githubusercontent.com/59768512/155548678-670ba5ce-fce1-4dc3-92f9-dc9e5c154a2c.png))

Running the `runme` executable, grants the shell sudo rights.

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


