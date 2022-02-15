## Number

>*On which line in rockyou.txt do you find the correct password for the admin user in wordpress?*

![wpscan](https://user-images.githubusercontent.com/59768512/152354724-03a29621-df1f-438f-8fe7-a5e1ccd7a163.png)

![robots.txt](https://user-images.githubusercontent.com/59768512/152354867-6a364ba4-9370-44ee-a746-de3b79ec13ac.png)


`10.225.150.160/admin` redirect to `http://10.225.150.160/wp-login.php?redirect_to=http%3A%2F%2F10.225.150.160%2Fwp-admin%2F&reauth=1`

![admin](https://user-images.githubusercontent.com/59768512/152354961-d49d19a1-0f5e-4793-92c7-04fdc1484d28.png)

![wpbrute](https://user-images.githubusercontent.com/59768512/152354596-62286464-89d2-4210-a0bc-732890f5c6cf.png)

![adminuser](https://user-images.githubusercontent.com/59768512/154031838-81c8435c-5538-4749-b4b6-2e8b9d4883fd.png)


![sucess](https://user-images.githubusercontent.com/59768512/154031211-98d7d02a-71f8-4d48-b693-0c3fdb49688f.png)


Use `sublime` with the `CTRL + F` search feature, and search for `ramirez` since its the password WPscan showed us: 



![image](https://user-images.githubusercontent.com/70077872/152354630-5cca4df8-3ad0-4971-817d-d4cd3695bb33.png)

## Ports

>*What ports are open on the host? (answer in ascending order)*

![image](https://user-images.githubusercontent.com/72946914/152356071-995428fe-be9c-4fe4-83ca-b7526130de09.png)


## Admin

>*Get access to the admin dashboard and provide the flag found.*

First, enter the admin details: `admin`:`ramirez`:

![image](https://user-images.githubusercontent.com/70077872/152356337-2013f9ae-2120-476f-be58-a3a0516eb1e9.png)

Then, the flag is in the dashboard:

![image](https://user-images.githubusercontent.com/70077872/152356590-f2780792-8f93-4e09-8516-cd76fb041a90.png)


## User

>*Get access to the user account, remember what you saw in the logs on how to do this. Provide the flag in user.txt*

After access has been gained to the Wordpress site, we have to do some additional steps to be able to gain terminal access to a user. As previously found in the logs, the user we want initial access to is `www-data`, which should be the default user running a webserver. This is because if `root` ran a webserver, all files would be accessible if the webserver is compromised. `www-data` is a user that has lower privileges, meaning a lower change of further damage after initial access.

As the logs also state, the attacker put a reverse shell within the `404.php` file within the `twentytwentyone` theme in Wordpress. This is a standard theme, which is also the only theme that is activated in this instance of Wordpress. The `404.php` file is a good file to replace with a reverse shell since it only shows error messages. Replacing other files like `functions.php` (which controls the whole theme) could result in the website not working properly, thus either raising an alarm towards the real owner or making it more difficult for us as attackers to utilize the reverse shell.

> Why a reverse shell? A reverse shell enables the attacker to make the victim connect to us, making it more straightforward than gaining a regular shell. Since we already have access to the Wordpress site, using a reverse shell is one of the "easiest" ways of gaining a terminal connection, and enables us to elevate our privileges at a later stage.

When access to the `404.php` file is made, one has to find a proper reverse shell script to enter. There are many different reverse shells based on language, terminal/non-terminal, and resources available. In this instance, we know the webserver runs PHP (as the file is named 404.php), making it feasible to find a reverse shell written in PHP. Doing a quick Google search for "reverse shell php" results in the Pentest Monkey's GitHub repository: https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php. This is a widely used script for PHP reverse shell, and is something we will try. The picture below showcases that the `404.php` file contents has been replaced with the PHP reverse shell, and that the IP and PORT is changed according to our IP and our desired listening port:

![image](https://user-images.githubusercontent.com/70077872/152495938-3446f01d-5977-45d4-9c14-c104ef5c6837.png)

![image](https://user-images.githubusercontent.com/70077872/152762635-bb95d4b0-710a-4567-b918-d58e29b95c24.png)





![image](https://user-images.githubusercontent.com/70077872/152495794-2651a028-68b2-4ed7-bf23-f2e5365c0312.png)



![image](https://user-images.githubusercontent.com/70077872/152495542-ded4206f-a13d-4897-8b72-50a2aacb24c5.png)


![image](https://user-images.githubusercontent.com/70077872/152496009-95482ebb-11eb-4878-bd5d-b28e4a5ad649.png)


