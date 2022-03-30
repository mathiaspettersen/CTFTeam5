## 4001 - Connection established

In order to establish a connection to the machine and the correct port, we use NetCat to scan the port. This is done by using the command `nc`, followed by the target ip. Finally, the port to be scanned is specified.

To be able to confirm that the port has been scanned and that the target machine has been accessed, we use the command `whoami` to be able to see who we are and which privileges can be used. Then `ls` is used to list the available files, including `flag.txt`.

Finally, "cat" is used on `flag.txt` fiile to open it, thereby revealing the flag.

![oppgave1-lab4](https://user-images.githubusercontent.com/46780028/160906356-d711a0ab-d396-4e43-948e-261693df9676.PNG)



## 4003 - all strings attached

