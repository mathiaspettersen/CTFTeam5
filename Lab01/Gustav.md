## Admin Password

By searching in Elastic for `http.response.status_code:200 and response:isAdmin` we are able to see the logs where the status code for the http response is `200 OK` and where the `isAdmin()` is set.

![Admin Password](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/adminpassword.PNG)

Expanding this log we can see that the admin password is `purple1`
![Admin Password2](https://github.com/Gustav-Magnussen/CTFTeam5/blob/main/Lab01/Screenshots/adminpassword2.PNG)

## Reverse Port

Searched for destination.port:9001
