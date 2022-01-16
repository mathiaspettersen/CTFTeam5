## Theme

*The attacker replaced the 404.php page of one of the themes with a reverse shell. Which theme was compromised? (all lowercase, one word)*

It was quickly discovered that the page in question was in fact a Wordpress site.  By entering "theme and 404" as a search word, we were left with 16 hits. For the attacker to fetch the  desired theme, an http GET request was required. By filtering on just GET requests, there were 5 hits left. 

By expanding one of the five this, it was discovered that the compromised theme was *twentytwentyone*.


