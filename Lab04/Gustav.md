##

![image](https://user-images.githubusercontent.com/59768512/158986258-1995798f-095e-423f-a5c5-549c66ef5105.png)


`userInput`,`password`,`first`,`second`.

![image](https://user-images.githubusercontent.com/59768512/158986037-cd3533e6-8681-4164-a385-2e727ce7423a.png)

first + reverse second + &second

![image](https://user-images.githubusercontent.com/59768512/158986543-d830b1b6-8799-40a4-baa5-5ce90c1846bb.png)



## 4003 - all strings attached


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
