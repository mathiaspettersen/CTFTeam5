I made a simple machine for ALL my users, this is a secure setup right? Port: 9001
image

image

Firstly, to find out what kind of priveleges that currently exist on user "user". To do this, we run the command sudo -1 and then enter the password for the user. Then, it lists that user "user" has acsess to all possiblle commands. This is proved by the system outputting (ALL : ALL) ALL as a respons to the given command.

This enables user "user" to switch user to "root", a higher priveleged user which is the system administrator. To do this, we run the command sudo su. To confirm that the "root" user has been acsessed, we can run the ls command and see which files that can now be acsessed after the switch. The flag can be found within the root file. 

Flag:IKT449{t00_much_p0wa}**
