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

![image](https://user-images.githubusercontent.com/59768512/152512225-2c75232a-08ae-4c57-b456-090709dcdf46.png) 

![image](https://user-images.githubusercontent.com/59768512/152512225-2c75232a-08ae-4c57-b456-090709dcdf46.png)
