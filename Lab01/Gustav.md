## Admin Password
>*What is the correct password for the admin account?*

By searching in Elastic for `http.response.status_code:200 and response:isAdmin` we are able to see the entries where the status code for the http response is `200 OK` and where the `isAdmin()` is set.

![Admin Password](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/adminpassword.PNG)

Expanding this entry we are able to see that the admin password is `purple1`.

![Admin Password2](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/adminpassword2.PNG)


## Strange File
*The attacker created a strange file one access to the machine was gained, can you find the file? Submit only the filname once you find it.*

Since we know that the attacker gained access to the `www-data` user first on the machine we can search for entries in the log where the user name is `www-data`, this can be done using the filter `user.name IS www-data`. 

![S1](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/strangefile1.PNG)

As this is still providing a lot of entries in the logs and expanding the entries shows that mostly all of them have `/usr/sbin/apache2` in the `process.executable` field. These are entries that account for traffic the webserver itself conducts and using a filter like `process.executable IS NOT /usr/sbin/apache2` reduces the matches down from 363 hits to 21 where one of the entries include the creation of a strange file called `IKT449{this_is_a_perfectly_benign_file}`

![S2](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/strangefile.PNG)

## Bruteforce Activity
*How many unique passwords was tried during the bruteforce?*

In order to find the total amount of unique passwords tried during the bruteforce attack the total amount of unsuccessfull and the successfull attempt have to be added. Filtering the logs by setting the `request.keyword` to `Incorrect` only shows entries where there was a unsuccessfull login attempt. Knowing that there were `670` unsuccessfull login attempts and one successfull one the total amount of tried passwords are `671`.

![B1](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/Bruteforce1.PNG)

## Bruteforce Tool
*From the logs one can conlude that a bruteforce has taken place. What tool has been used to perform this attack? (all lowercase)*

In order to find out what type of tool was used to perform the bruteforce attack the phrase `password` was searched in Elastic. This resulted in a number of entries where the phrase `Incorrect username or password` were present. Expanding one of these entries and looking at the `user-agent.original` field shows that a tool called `wpscan` is used, which is a Wordpress bruteforcing tool.

![B2](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/incorrect.PNG)

## User what now?
*What was the first user they got access to ? (on the machine itself)*

As is found earlier the framework `wordpress` is running on port 80 on the machine, given that the attacker gained access to the webserver framework first it would make sense that landing the victim machine the attacker would initially have access to user running the webserver. Using best practice when setting up a webserver on a Ubuntu platform is to use the `www-data` user. Filtering the logs using `user.name:www-data`shows that a number of commands have been executed on the machine including commands that setup a more user-friendly enviroment for further escalation. Therefore the first user the attacker got access to on the machine is `www-data`.

![U1](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/www-data.PNG)

## Log count
*How many logs are found in the interesting timeframe?*

Looking at the discovery section of Elastic it can be seen that a total of `6788` entries are present when not filtering for anything.

![LC](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/totalLogs.PNG)

## Log sources
*What data shippers from Elastic have been used to log all the different logs found in Kibana? Sort the values in alphabetical order, sepereated with a comma, all lowercase. Ex: IKT449{blue,red,zebra}*

Expanding the `agent.type` field within Elastic show that in total there are three different data shippers that have been used meaning that the correct answer is `IKT449(auditbeat,filebeat,packetbeat)`.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/agenttype.PNG)

## Got Port?
*What port is the most commonly requested one in the logs?*

Expanding the `destination.port` field within Elastic shows that the most commonly requested port is port `80`.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/dport.PNG)


## Reverse Port
*The attacked managed to get a reverse shell from the machine, on which port did the attacker recieve this shell?*

Filtering the logs for the phrase `reverse shell` displayes a number of entries where a pentestmonkey reverse shell is mentioned.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/reverseshell.PNG)

Expanding the `http.request.body.content` field of one of this entries shows a lot of text where the port `9001` is mentioned showing that this is the port the attacker used.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/portreverse.PNG)

## File Read 1
*What file was read by the attackers initial user in /home/user ?*

Knowing that the attacker initially gained access to the user `www-data` and that the file that is read is in the `/home/user` directory the logs can be filtered using `user.name:www-data and process.working_directory:/home/user`. This gives a total of three entries and looking at one of theses shows that the attacker used the executable `/usr/bin/cat` on a file called `note.txt`.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/userread1.PNG)

## File Read 2
*What file did the attacker read once logged in as 'user' ?*

Knowing that the attacker used the executable `/usr/bin/cat` to read file the logs can be filtered using `user.name:user and process.executable:/usr/bin/cat`. This provides a total of 13 entries where only one is using the `/usr/bin/cat` executable. Expanding this entry shows that the user `user` used the executable `/usr/bin/cat` on a file called `user.txt`.

![LS](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/useruser.PNG)
