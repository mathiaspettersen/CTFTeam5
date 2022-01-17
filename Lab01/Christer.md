Login time
00:08:23

In order to find the correct logintime, we had to find a login time in which the auditd.result was a sucsess. Many of the audits before the correct point of time were sucsessfully executed, by "user" as the primary actor. However, they only got acsess to "root" as the secondary actor. Therefore, by filtering the times the audit was sucsessfull, as well as investigating at which times the user "user" acted as the secondary actor consistantly, we were able to find out at which time the login was sucsessful. This first occoured at 00:08:23.248, from which point the logins further consistaltly returned the audit sucsessful with "user" as the secondary actor".



![image](https://user-images.githubusercontent.com/46780028/149744204-45c93546-667f-462f-80bd-e35e2956c9a3.png)
