# picklerick

Hello folks, I’m Yousef Elnagar AKA (MrDurdenEG)
a cybersecurity enthusiast and beginner CTF player
in this write-up I'll walk you through my journey solving the 'pickle rick' room
explaining my thought process and how I got the final ingredients

![alt text](image.png)

first i open the web application and i got this
![alt text](<Screenshot 2025-12-07 154401.png>)

i viewed the source-page
![alt text](<Screenshot 2025-12-07 154449.png>)

i got the username
![alt text](<Screenshot 2025-12-07 154455.png>)

Username: R1ckRul3s

i noticed that there a path named 'assets'
![alt text](image-1.png)
but i got nothing some images and scripts

try to open '/robots.txt' path
![alt text](<Screenshot 2025-12-07 155316.png>)

i got something not sure what's this but kept in my mind

Wubbalubbadubdub

then i try to fuzzing the URL

ffuf -u http://10.82.168.227/FUZZ -w /root/Desktop/Tools/wordlists/SecLists/Discovery/Web-Content/common.txt -e .php,.html,.txt -mc 200

-e .php,.html,.txt : to find the paths with those extension
-mc 200 : to filter the paths with response status = 200 (succeeded)
![alt text](<Screenshot 2025-12-07 143812.png>)

got those files : index.html , login.php , robots.txt

we already saw /robots.txt

index.html is the main page

when i access : http://10.82.168.227/login.php it shows this page
![alt text](<Screenshot 2025-12-07 155355-1.png>)

i tried the username i got
Username: R1ckRul3s
password : Wubbalubbadubdub
this password worked

![alt text](image-2.png)

command panel

i tried some commands

ls -la
![alt text](image-3.png)

got some interesting files

Sup3rS3cretPickl3Ingred.txt
clue.txt

when i try to 'cat' those files and it was disabled
![alt text](<Screenshot 2025-12-07 155510.png>)

i accessed the files by put it as a path
http://10.82.168.227/Sup3rS3cretPickl3Ingred.txt
![alt text](image-4.png)
got first ingredient

http://10.82.168.227/clue.txt
![alt text](image-5.png)
nothing important

i showed the source-page i got something
![alt text](image-6.png)

<!-- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== -->

base64 decode
i decoded this string 6 times
![alt text](<Screenshot 2025-12-07 144505.png>)
finally i got something : rabbit hole
i can't use it anywhere

so i keep use command panel

i try to show working dir 'pwd'
![alt text](image-7.png)

i got back to the root dir 'cd /../../ && ls -la' to list the dir
![alt text](image-8.png)

important directories (home, root)
lets see 'cd /home && ls -la'
![alt text](image-9.png)

rick dir lets go through it 'cd /home/rick && ls -la'
![alt text](image-10.png)
second ingredients hhhmmmmmmmm
but i cant use 'cat'
i try (cat,head ,tail,more) but all were disabled
but 'less' workeddd

less '/home/rick/second ingredients'
![alt text](image-11.png)

got the second ingredient : 1 jerry tear

lets now try the '/root' dir
i tried to list the dir but i cant , i noticed "drwx------ 4 root root 4096 Jul 11 2024 root"
the root is the only one who has permission lets try 'sudo'
'sudo ls -la /root'
![alt text](image-12.png)
3rd.txt????? hhmmmm
'less /root/3rd.txt'
![alt text](image-13.png)
3rd ingredients: fleeb juice
