# 4005 - screw it

>*I give up, no more passwords, no more backdoor. This is just a simple app which provides your string in return. Nothing else that is strange right?*

The task grants us with two files: task5 and task5.c. task5 is a compiled C-program that returns the value in which is put in by the user, while task5.c is the non-compiled raw code of the program. Below is the source code with some additional comments to explain how the program functions:

![image](https://user-images.githubusercontent.com/70077872/160817314-686c72c2-6a3c-41f7-ba45-a89ee48c8f61.png)

As seen in the picture above, the program defined a maximum flag size of 64 characters. It also includes a function `sigsegv_handler` that prints the flag file. This is the function we want to invoke to retrieve our flag. Further down, the `main` function reads the flag file, and if the flag file is present, it invokes the `SIGSEGV
