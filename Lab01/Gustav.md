## Admin Password
*What is the correct password for the admin account?*

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



## Reverse Port

Searched for destination.port:9001
