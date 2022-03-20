## 4003 - all strings attached

![image](https://user-images.githubusercontent.com/59768512/158986258-1995798f-095e-423f-a5c5-549c66ef5105.png)


`userInput`,`password`,`first`,`second`.

![image](https://user-images.githubusercontent.com/59768512/158986037-cd3533e6-8681-4164-a385-2e727ce7423a.png)

first + reverse second + &second

![image](https://user-images.githubusercontent.com/59768512/158986543-d830b1b6-8799-40a4-baa5-5ce90c1846bb.png)



## 4004 - INTelligence


Running the attached file seems to ask for a PIN code and returning "no" if the PIN code is wrong

![image](https://user-images.githubusercontent.com/59768512/158981968-c814230a-f6c9-4b50-b2a9-14f72a1b6ee6.png)


Open file in IDA Freeware and generating pseudocode for the main functions gives a bit of understanding into what actually happens in when runing the binary.

![image](https://user-images.githubusercontent.com/59768512/158981863-611d4047-8441-4754-8e95-3e1637705216.png)

The `fgets(s, 30, stdin9;` seems to take the user input and puts it into the variable `s`, while the variable `v5` seems to be what it is compared to. Renaming the `s` variable to `userInput`  and the `v5` to `password` will help with readability. Looking at line 16 and 17, the main function calls the library function `strcpy` and adding the integere values to the `password` and `src` variables. This means that the `password=123454652311` and `src=22`. 

IDA Freeware also seems to think that the `password` is the same as `*(_QWORD *)&password[13]`.

![image](https://user-images.githubusercontent.com/59768512/158984054-94d38097-f91a-4ddf-8e00-e37ae5ebd863.png)

Looking at lines 20 and 21 the libarary function `strcat` gets called and the adds the string of the `src` variable to the `password` variable and later the string of the `password` variable as well. Meaning that the final password variable contains the string `22123454652311` which should be the pin


![image](https://user-images.githubusercontent.com/59768512/158984306-202f5e3a-22e2-4a8c-be52-188914d4742a.png)

Using this pin confirms the theory. 

## 4005 - screw it

210

![image](https://user-images.githubusercontent.com/59768512/159141285-91a3e03a-2321-4c94-92b1-1dd764d771ea.png)

## PY - 01

String to decimal to hexadecimal, add `secret_number` to decimal value.

![image](https://user-images.githubusercontent.com/59768512/159143200-5955d8aa-e21c-42d6-810d-35c8518c852f.png)


Starts with "I"

Unicode character of "I"

![image](https://user-images.githubusercontent.com/59768512/159143239-1d9948de-fbcd-4bfe-aa71-c9e252878720.png)

First encrypted is `0xb4`

![image](https://user-images.githubusercontent.com/59768512/159143158-20607207-df29-423e-a6bf-a285ce99a949.png)

180-73= 107

`secret_number=107`

code 

![image](https://user-images.githubusercontent.com/59768512/159143186-f7f26125-4620-4193-9d12-1c1558f269b8.png)


