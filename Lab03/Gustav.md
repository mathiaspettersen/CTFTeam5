## 9001

![image](https://user-images.githubusercontent.com/59768512/155337571-062b0dec-a2ef-4300-9458-90bcd8cfc9d7.png)

![image](https://user-images.githubusercontent.com/59768512/155337634-9efdc87a-8f88-4786-a02c-7b3366bdd99e.png)

>**Flag:`IKT449{t00_much_p0wa}`**

## 9002

![image](https://user-images.githubusercontent.com/59768512/155338344-d5b9e046-c364-4bf5-a6e6-1233fc1ac327.png)

![image](https://user-images.githubusercontent.com/59768512/155338992-b9cff96c-e371-429a-afb0-937c7fc425ef.png)

![image](https://user-images.githubusercontent.com/59768512/155338250-24dc5d90-62de-4c1d-b4e6-a37bde593330.png)

>**Flag:`IKT449{APT_APT_APT_m000re_l11ke_ATP}`**

## 9005

![image](https://user-images.githubusercontent.com/59768512/155546194-4a7e181e-a8ea-4d1f-897e-293fbeae690f.png)

Run runme seems to execute the id command.

![image](https://user-images.githubusercontent.com/59768512/155546461-5acfea81-5d2c-41bc-8ab0-5a66a0eb0640.png)

Altering the path used by the `runme` executable might trick it into running a fabricated version of the id command


Running the command: `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u` finds writeable folders.
`2>/dev/null` cleans error messages
`cut -d "/" -f 2,3` 
`grep -v proc` removes results related to processes
`sort -u` sorts numerically and alphabetical

![image](https://user-images.githubusercontent.com/59768512/155546802-a04f101c-dfa8-4cac-bbca-9b85a6fa6184.png)

Adding the writeable folder `/tmp` to the path means that executable files will use programs present in that folder

Adding a altered `id` file and make it execute `/bin/bash` means that when the `runme` executable file is executed it will run the fabricated id file and spawn a sudo bash shell.
`echo "/bin/bash" > id`
`chmod 777 id`

![image](https://user-images.githubusercontent.com/59768512/155548678-670ba5ce-fce1-4dc3-92f9-dc9e5c154a2c.png))

Running the `runme` executable, grants the shell sudo rights.

![image](https://user-images.githubusercontent.com/59768512/155549244-e5176232-28d0-4393-b12d-6a351186ea6a.png)

>**Flag:`IKT449{show_me_your_id_please}`**


## 9007

The task text mentioned outdated applications. In order to get an idea of possible package that can be execute the command `find / -type f -perm -04000 -ls 2>/dev/null` was executed:

![image](https://user-images.githubusercontent.com/59768512/155718341-ae8009ee-23d8-4ca4-afc0-551d261a86d3.png)

The application `screen` is o present, this might be the vulnerable application


![image](https://user-images.githubusercontent.com/59768512/155718037-f2b0c826-cf0f-45db-83f7-be8389c1baf5.png)


>**Flag:`IKT449{it_might_be_ugly_but_it_works}`**


