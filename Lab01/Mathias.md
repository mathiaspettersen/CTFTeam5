


## name

Wordpress is the underlying framwork as teh attacker uses wp-scan to bruteforce.

## Kibana

uses 7.16.2 by checking the

## More ports

Had to find the exact time fram in which the scanning occurred, which was done by looking at the table in the Discover page. There is a time fram in which stands lonely:

![bilde](https://user-images.githubusercontent.com/70077872/149670992-a3512899-d5be-4cc1-9b4a-79be78e1a922.png)

Checking the main port of nmap, which can also be verified by looking at the last response:

![bilde](https://user-images.githubusercontent.com/70077872/149671041-a3047184-b780-4c46-9809-c5e3e2d8b310.png)

Then, entering this timeframe into the Dashboard, and choosing the specified port, with filtering the unique destination ports got the result of 100 ports:

![bilde](https://user-images.githubusercontent.com/70077872/149670940-425cf730-fad2-467c-8bec-bbb71adcc62e.png)



# Extra

## Slide into Julius Caesar DM's

### Caesar cipher, put it into cyberchef and rotate the ciphers. After rotating it and trying different possibilities, the ROT number is 13, and revels sthe flag in clear text:

![bilde](https://user-images.githubusercontent.com/70077872/149499935-9c8d6648-a99a-40d1-9060-6ae23b763dd1.png)


## 200 IQ

We are given a weird looking string with different characters that have to be decoded. As it says in the text, this string is an esolanguage. After searching for esolanguages, a Wikipedia article came up about the "Esoteric programming language", where "Brainfuck" is mentioned. This language also looks like the one we are trying to decode:

![bilde](https://user-images.githubusercontent.com/70077872/149500736-b83bbf77-f15f-4ad2-8b68-0bac7f5116f0.png)

To ecode, we tried searching for a "Brainfuck decoder", where https://www.dcode.fr/brainfuck-language came up. After passing the string and decoding it, we got the flag:

![bilde](https://user-images.githubusercontent.com/70077872/149501063-59cd8a84-35a2-45bd-ae38-2577f6edafbd.png)

