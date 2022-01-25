# CTFTeam5

## Introduction

*The attack occured from Jan 8, 2022 @ 23:55:00.000 to Jan 9, 2022 @ 00:10:00.000... [truncated]*

To confirm to the rules and regulations to perform the tasks, we replied `IKT449{i_will_behave}`.

## Attacker 

### Attacker

*From which IP does the attack originate?*

From the task given, it was apparent that the victim IP address was `10.13.37.4`. To find the attacker IP address, we filtered for `source.ip` within Elastic, where it showcased two IPs. Since `.4` was the victim, the `10.13.37.5` is the attacker since it has the most requests:

![bilde](https://user-images.githubusercontent.com/70077872/150950686-69d9b703-fb9a-4ab6-921d-56da8ecf9b63.png)


### Search
*The attackers performed a few searches on the site, what was the last thing he searched for?*

Most often, searches are viewed as `/?s=` in the URL. By entering this as a filter, in addtion to the attackers IP address, we were left with two hits. 

![bilde](https://github.com/Gustav-Magnussen/CTFTeam5/blob/fed34ccb99994864921f08254e002888e51cef5b/Lab01/Screenshots/search_filter_results.PNG)

Checking the search result in the newest hit, it was discovered that the search was `/?s=hello+this+is+a+test`. Thus, the answer to the challenge was *hello this is a test*.

![bilde](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/search_answer.PNG)

## Bruteforce

### Bruteforce Activity V

### Bruteforce Tool V

### Bruteforce Success

*What is the content of the _id field where the correct password was guessed from bruteforcing?*

We previously found out that the password for the admin user was "purple1". To find the "id" field in which the correct username was found, we searched for "purple1" and looked at the packet. This was done since the system most likely accepts the correct password and gives the successful message back in the same packet:

![bilde](https://user-images.githubusercontent.com/70077872/149674201-88ff335d-3b7f-4bf3-892c-d52e6e6fc5cd.png)

This can be verified later in the packet since the variable "isAdmin" returns a boolean value of "1", meaning the Admin password was correct:

![bilde](https://user-images.githubusercontent.com/70077872/149674301-1cf71a85-5fea-4175-a3c6-8e0c84467359.png)

## Files

### File Read 1 V

### File Read 2 V

### Strange File V

## Firefox V

## Framework 

### name

*What framework is used to host the site running on port 80 on the victim? (all lowercase)*

As found in Bruteforce Tool, the tool to perform the scanning was `wp-scan`. After researching WP-scan, it was apparent that it is used for bruteforcing the Wordpress framework's login page. As such, the underlying framework running on port 80 is Wordpress.

### Themes
*The attacker replaced the 404.php page of one of the themes with a reverse shell. Which theme was compromised? (all lowercase, one word)*

It was quickly discovered that the page in question was in fact a Wordpress site.  By entering "theme and 404" as a search word, we were left with 16 hits. For the attacker to fetch the  desired theme, an http `GET` request was required. By filtering on just GET requests, there were 5 hits left. 

![bilde](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/compromised_theme.PNG)

By expanding one of the five this, it was discovered that the compromised theme was *twentytwentyone*.

## Kibana V

## Logs

### Log Count V

### Log Sources V

## Port

### Got Port? V

### Reverse Port V

### More Ports

*It seems to have occured a portscan at the start of the attack, how many ports where tested?*

Had to find the exact time fram in which the scanning occurred, which was done by looking at the table in the Discover page. There is a time fram in which stands lonely:

![bilde](https://user-images.githubusercontent.com/70077872/149670992-a3512899-d5be-4cc1-9b4a-79be78e1a922.png)

Checking the main port of nmap, which can also be verified by looking at the last response:

![bilde](https://user-images.githubusercontent.com/70077872/149671041-a3047184-b780-4c46-9809-c5e3e2d8b310.png)

Then, entering this timeframe into the Dashboard, and choosing the specified port, with filtering the unique destination ports got the result of 100 ports:

![bilde](https://user-images.githubusercontent.com/70077872/149670940-425cf730-fad2-467c-8bec-bbb71adcc62e.png)

## Users

### Admin Password V

### User what now? V

### Login Time V
