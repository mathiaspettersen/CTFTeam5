
# 4005 - screw it

>*I give up, no more passwords, no more backdoor. This is just a simple app which provides your string in return. Nothing else that is strange right?*

The task grants us with two files: task5 and task5.c. task5 is a compiled C-program that returns the value in which is put in by the user, while task5.c is the non-compiled raw code of the program. Below is the source code with some additional comments to explain how the program functions:

![image](https://user-images.githubusercontent.com/70077872/160817314-686c72c2-6a3c-41f7-ba45-a89ee48c8f61.png)

As seen in the picture above, the program defined a maximum flag size of 64 characters. It also includes a function `sigsegv_handler` that prints the flag file. This is the function we want to invoke to retrieve our flag. Further down, the `main` function reads the flag file, and if the flag file is present, it invokes the `SIGSEGV` which runs the `sigsegv_handler` function.  Lastly, the program defines a buffer size of 128 characters, but the problem is that is also outputs up to 256 characters of data. This flaw contributes to a buffer overflow vulnerability. 

In our case, to invoke the `sigsegv_handler` functions that prints the flag, we have to overflow the input with characters. Since the buffer is 128 characters, and the max size of the flag is 64 characters, we use Python to give us 200 characters to be safe:

![image](https://user-images.githubusercontent.com/70077872/160821819-43981c87-14ef-4384-9cfa-a52477e2346f.png)

Then, after getting our 200 characters, the program is run and the characters are put in. The program then returns our string along with the flag which is read from out-of-bounds memory:

![image](https://user-images.githubusercontent.com/70077872/160821983-f3dcea7d-da8f-4e1e-9774-6c055e4fce5f.png)


>**Flag:`IKT449{B0000000000000000000_big_00000000000000000000F}`**
