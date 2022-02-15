## Number

>*On which line in rockyou.txt do you find the correct password for the admin user in wordpress?*

![wpscan](https://user-images.githubusercontent.com/59768512/152354724-03a29621-df1f-438f-8fe7-a5e1ccd7a163.png)

![robots.txt](https://user-images.githubusercontent.com/59768512/152354867-6a364ba4-9370-44ee-a746-de3b79ec13ac.png)


`10.225.150.160/admin` redirect to `http://10.225.150.160/wp-login.php?redirect_to=http%3A%2F%2F10.225.150.160%2Fwp-admin%2F&reauth=1`

![admin](https://user-images.githubusercontent.com/59768512/152354961-d49d19a1-0f5e-4793-92c7-04fdc1484d28.png)

![wpbrute](https://user-images.githubusercontent.com/59768512/152354596-62286464-89d2-4210-a0bc-732890f5c6cf.png)

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

![image](https://user-images.githubusercontent.com/70077872/152762635-bb95d4b0-710a-4567-b918-d58e29b95c24.png)


![image](https://user-images.githubusercontent.com/70077872/152495938-3446f01d-5977-45d4-9c14-c104ef5c6837.png)


![image](https://user-images.githubusercontent.com/70077872/152495794-2651a028-68b2-4ed7-bf23-f2e5365c0312.png)



![image](https://user-images.githubusercontent.com/70077872/152495542-ded4206f-a13d-4897-8b72-50a2aacb24c5.png)


![image](https://user-images.githubusercontent.com/70077872/152496009-95482ebb-11eb-4878-bd5d-b28e4a5ad649.png)


