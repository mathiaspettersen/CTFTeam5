## Number
>*On which line in rockyou.txt do you find the correct password for the admin user in wordpress?*

Knowing that the server is running WordPress on port 80 the tool wpscan can be used to scan the website and look for possible vulnerabilities and interesting directories or files. Using the wpscan tool a scan of the website can be performed using the command:

![wpscan](https://user-images.githubusercontent.com/59768512/152354724-03a29621-df1f-438f-8fe7-a5e1ccd7a163.png)

The output of this command provides a lot of information about the WordPress installation running on the server. This includes information about a robots.txt file which states that an interesting entry is `/wp-admin/`. Within a WordPress installation, the `/wp-admin/` directory is most commonly used as the admin dashboard. 

![robots.txt](https://user-images.githubusercontent.com/59768512/152354867-6a364ba4-9370-44ee-a746-de3b79ec13ac.png)

Trying to access the `/wp-admin/` directory by entering the webpage `http://10.225.150.160/wp-admin` redirects to `http://10.225.150.160/wp-login.php?redirect_to=http%3A%2F%2F10.225.150.160%2Fwp-admin%2F&reauth=1` which is the admin login page.

![admin](https://user-images.githubusercontent.com/59768512/152354961-d49d19a1-0f5e-4793-92c7-04fdc1484d28.png)

To find a username and password combo that allows for access to the admin dashboard the wpscan tool is used again. This time only using the `--passwords` option which allows for brute-force attempts on the password of WordPress users. Using this option also requires a wordlist to acquire passwords to test from, here the `rockyou.txt` wordlist is chosen.

![wpbrute](https://user-images.githubusercontent.com/59768512/152354596-62286464-89d2-4210-a0bc-732890f5c6cf.png)

Running the above command the wpscan tool attempts to find valid users on the WordPress site by looking for mentions in places like created posts. Using this technique the user `admin` is quickly discovered.

![adminuser](https://user-images.githubusercontent.com/59768512/154031838-81c8435c-5538-4749-b4b6-2e8b9d4883fd.png)

Having found this user the tool starts a brute-force attack against the aforementioned admin login site using the `admin` username and passwords from the `rockyou.txt` wordlist.

![sucess](https://user-images.githubusercontent.com/59768512/154031211-98d7d02a-71f8-4d48-b693-0c3fdb49688f.png)

After a very short scan, the wpscan tool recognizes the WordPress administrator credentials as `admin:ramirez`. To find the line in `rockyou.txt` where the password `ramirez` is located the `rockyou.txt` file is opened using the text editor `sublime`. Within `sublime` the `CTRL + F` search feature can be used to search for `ramirez`.

![image](https://user-images.githubusercontent.com/70077872/152354630-5cca4df8-3ad0-4971-817d-d4cd3695bb33.png)

This search shows that the password `ramirez` is located at line `1337` in the `rockyou.txt` wordlist. 

Flag:`IKT449{1337}` 

## Ports

> *What ports are open on the host? (answer in ascending order)*

To figure out which ports were open, we chose to use `nmap`. This is a free and open security scanner which is used to gain information about hosts and running services on a network, includning information about open ports by performing port scanning. 

We used `nmap` to scan our given target IP address, along with three different switches. The perfromed scan can be viewed in the screenshot bellow. 

 `-T4` is used to specify the timing and performance. Since we wanted a specific and reliable scan, `-T4` was chosen before `-T5` as the latter tends to be too strong and fast, and can as a consequence omit important information. 

Next, to specify the service and version detection wanted, `-A` is used,  which includes an OS dectection, version detection, script scanning and traceroute. Looking at the screenshot, using `-A`is what enables the `VERSION` column at line five. In addition, all of the service protocols in the `SERVICE` column have been now verified, as opposed to if we had not used `-A`. 

Finally, since we wanted information about all ports available, `-p-` states that we want information about all ports, i.e. from 0 to 65 365. 

The `PORT` column lists all availble ports, which in this case are  22, 80 and 1337 . Thus, the flag is *IKT449{22,80,1337}*. 

![port_scan](https://user-images.githubusercontent.com/72946914/152356071-995428fe-be9c-4fe4-83ca-b7526130de09.png)

Flag: `IKT449{22,80,1337}`.


## Admin

>*Get access to the admin dashboard and provide the flag found.*

By using the login password that was obtained in the `rockyou.txt` file using WPscan, acsess to the admin user will be gained using the correct username and password. First, enter the login details: Username: `admin`. Password: `ramirez`.

![image](https://user-images.githubusercontent.com/70077872/152356337-2013f9ae-2120-476f-be58-a3a0516eb1e9.png)

Then, after successfully logging in to the admin user, the flag will be visible on the dashboard. The flag is marked as IKT449{admin_access_is_not_for_everyone} under "Your Recent Drafts".

![image](https://user-images.githubusercontent.com/70077872/152356590-f2780792-8f93-4e09-8516-cd76fb041a90.png)

Flag: `IKT449{admin_success_is_not_for_everyone}`.


## User

>*Get access to the user account, remember what you saw in the logs on how to do this. Provide the flag in user.txt*

After access has been gained to the Wordpress site, we have to do some additional steps to be able to gain terminal access to a user. As previously found in the logs, the user we want initial access to is `www-data`, which should be the default user running a webserver. This is because if `root` ran a webserver, all files would be accessible if the webserver is compromised. `www-data` is a user that has lower privileges, meaning a lower change of further damage after initial access.

As the logs also state, the attacker put a reverse shell within the `404.php` file within the `twentytwentyone` theme in Wordpress. This is a standard theme, which is also the only theme that is activated in this instance of Wordpress. The `404.php` file is a good file to replace with a reverse shell since it only shows error messages. Replacing other files like `functions.php` (which controls the whole theme) could result in the website not working properly, thus either raising an alarm towards the real owner or making it more difficult for us as attackers to utilize the reverse shell.

> Why a reverse shell? A reverse shell enables the attacker to make the victim connect to us, making it more straightforward than gaining a regular shell. Since we already have access to the Wordpress site, using a reverse shell is one of the "easiest" ways of gaining a terminal connection, and enables us to elevate our privileges at a later stage.

When access to the `404.php` file is made, one has to find a proper reverse shell script to enter. There are many different reverse shells based on language, terminal/non-terminal, and resources available. In this instance, we know the webserver runs PHP (as the file is named 404.php), making it feasible to find a reverse shell written in PHP. Doing a quick Google search for "reverse shell php" results in the Pentest Monkey's GitHub repository: https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php. This is a widely used script for PHP reverse shell, and is something we will try. The picture below showcases that the `404.php` file contents has been replaced with the PHP reverse shell, and that the IP and PORT is changed according to our IP and our desired listening port:



![image](https://user-images.githubusercontent.com/70077872/152762635-bb95d4b0-710a-4567-b918-d58e29b95c24.png)


Now that the file is updated and saved with our desired PHP reverse shell script, we have to set up a **listener** which listens on the connection coming from the victim's website server. Common binaries are **nc**, **ncat**, and **netcat**. In this instance we are using **ncat** as it comes from the same developers as **nmap**, and provides proper encryption within the reverse shell connection.

> The command for listening is: ncat -nlvp 9001 

The command has four switches; -nlvp. These are: 

* -n : Do not resolve hostnames via DNS
* -l : Bind and listen for incoming connections
* -v : Set verbosity level
* -p : Specify source port to use

Once the listener is set up, all is ready for the connection to take place. Activating the PHP reverse shell script has to be manually done by traversing into the general Wordpress directory that holds the themes and the specific file. The directory path is shown below:


![image](https://user-images.githubusercontent.com/70077872/152495938-3446f01d-5977-45d4-9c14-c104ef5c6837.png)


After the file is activated, **ncat** sees a connection coming in on port 9001, and runs a basic shell based on that connection. As seen in the picture below, running **whoami** showcases that we are `www-data` on the `victim` box. Within the user `user` folder, there are two text files; `note.txt` and `user.txt`. We do not yet have the permission to view the contents of `user.txt`, but luckily, `note.txt` contains the credentials for `user`. Once logging in with the password given, we are able to view `user.txt` and collect the final flag:


![image](https://user-images.githubusercontent.com/70077872/152495794-2651a028-68b2-4ed7-bf23-f2e5365c0312.png)



![image](https://user-images.githubusercontent.com/70077872/152495542-ded4206f-a13d-4897-8b72-50a2aacb24c5.png)


![image](https://user-images.githubusercontent.com/70077872/152496009-95482ebb-11eb-4878-bd5d-b28e4a5ad649.png)


Flag: `IKT449{great_job_root_will_come_later}`
