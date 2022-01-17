## Theme

*The attacker replaced the 404.php page of one of the themes with a reverse shell. Which theme was compromised? (all lowercase, one word)*

It was quickly discovered that the page in question was in fact a Wordpress site.  By entering "theme and 404" as a search word, we were left with 16 hits. For the attacker to fetch the  desired theme, an http `GET` request was required. By filtering on just GET requests, there were 5 hits left. 

By expanding one of the five this, it was discovered that the compromised theme was *twentytwentyone*.


## Search

*The attackers performed a few searches on the site, what was the last thing he searched for?*

Most often, searches are viewed as `/?s=` in the URL. By entering this as a filter, in addtion to the attackers IP address, we were left with two hits. 

![bilde](https://github.com/Gustav-Magnussen/CTFTeam5/blob/fed34ccb99994864921f08254e002888e51cef5b/Lab01/Screenshots/search_filter_results.PNG)

Checking the search result in the newest hit, it was discovered that the search was `/?s=hello+this+is+a+test`. Thus, the answer to the challenge was *hello this is a test*.

![bilde](https://github.com/Gustav-Magnussen/CTFTeam5/blob/fed34ccb99994864921f08254e002888e51cef5b/Lab01/Screenshots/search_answer.PNG)
