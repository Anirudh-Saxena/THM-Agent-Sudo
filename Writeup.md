# Task 1
Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!


Lets deploy the rooms and move forward with the questions..

# Task 2

**Q. How many open ports?**

For this we can use the nmap scan


        nmap -sV -sC <targetip>

![nampsc](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/8f2c8957-7bcf-4bfc-86cc-b0eb4b03208c)

So in total `3` ports are open

**Q. 
How you redirect yourself to a secret page?**

Ans: After visitng the webiste, and reading the text the answer was quite simple 

` Use your own codename as user-agent to access the site. `

`user_agent` is used to access the secret page..


**Q.What is the agent name?**

For this what i did was i tried spoofing with different agents name, like on the website the message was written by `Agent R`

So in total there are 26 alphabets, R being removed remains 25, lets do the hit and trial method and see if any agent has a 
has valid text in it or not, or any agent name that we can see

THe command that we will be using is:

          curl -A "alphabets" -L <targetip>

![Screenshot_2023-08-14_05_04_32](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/3c2287ee-a2af-4dcb-b0bd-2c4bd66ea9cb)



Noice agent C name is `chris`


This solves are 3 question **What is the agent name?**

# Task 3

Lets move forward now..

**Hash cracking and brute-force**


**Q. FTP password**

FOr this we can use the hydra, the command is as follows:

      hydra -l chris -P /usr/share/wordlists/rockyou.txt <targetip> ftp

![hailhydra](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/6af27055-9cf9-4774-ae76-85584874e2e5)


SO the password of agentC is `crystal` bruh!!

Now lets try to login with the FTP as we have the name as well as the password of the agent


      ftp chris@<Targetip>

![ftpconn](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/d6f24403-3350-44d3-8d1a-c9e57ccd5785)

I was trying the wrong command here, after searching for a bit i found out that in order to download the files from the ftp server we use the 
`mget` command

SO lets donwload and see what the files have hiddne for us..

![Screenshot_2023-08-14_05_05_09](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/915f7f27-aae8-4e0f-8fec-589b4390cd8a)


NOw lets read the files first and see what it has to offer..


![agentJ](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/8fc9db82-8be9-4ca6-ad30-a8fadab46822)


Okieee so we have to try setgo also in this chal intresting..

Now lets check which file is legit by using the tools such as binwalk and exiftool, the command is quite simple for this


      binwalk <fnmae>

And in order to extract the data if that file has something hidden in it is

        binwalk -e <fname>

![Screenshot_2023-08-14_03_28_07](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/74e5265e-c9bf-4d1a-a613-c2f93ae1010a)

As we can see that the cuite.png has some hidden data in it lets see what it is any how can we unlock it..

For this we will using the john to crack the password of the zip as there are no clues..

![Screenshot_2023-08-14_03_34_06](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/3351e370-0070-4691-8e1c-0ff80de2be29)

After getting the password lets move forward and unzip the `8702.zip`, you can use gzip or 7z or any other tool that you are familiar with..

![Screenshot_2023-08-14_03_41_30](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/6d4a3900-58d9-42fe-add9-2b604853c0fc)


NOw lets read the To_agentR file.

    cat To_agentR.txt 
    Agent C,
    We need to send the picture to 'QXJlYTUx' as soon as possible!
    By,
    Agent R

Hmm instresting, lets find out what does it means `QXJlYTUx`
  

https://gchq.github.io/CyberChef/ using this website the text appear to be `Area51` noice we got the password for the other image now lets 
try to extract the data from the other file


Lets see wether it has any hidden data in it using `steghide` tool..


![Screenshot_2023-08-14_03_54_54](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/e5ffc441-c24c-49ab-a3d1-cd2255620122)

# Task 4

Noiceee, lets go we got the pass and its time to go for the ssh connection....

After succesfully connecting with the server lets see what are the files in it..

![image](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/b0d0020e-5f01-474a-bf53-75848fc3b5f4)

Now lets see the content of the txt file

![image](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/1c0e403d-d2f0-48e4-a839-1dae7a8b4a3a)



**Q. What is the incident of the photo called?**

For this we will have to donwload the photo the command will be as follows:


        scp james@'machine-ip':/home/james/Alien_autospy.jpg /home/kali/Downloads/

After doing a rev search through google image , the incident appears to be as follows: roswell alien autopsy



Now the final task..


# Task 5

**Enough with the extraordinary stuff? Time to get real.**

**CVE number for the escalation** 

I tried the commands `sudo -l` and `sudo -V`  in order to find something useful to exploit..

https://www.exploit-db.com/exploits/47502

Make sure to visit the page and see how can we exlpoit this CVE and gain root access to the server..

After that you just have to navigate to the `/root` directory and check the file to gain the answer of the final step.


![Screenshot_2023-08-14_05_06_47](https://github.com/Anirudh-Saxena/THM-Agent-Sudo/assets/73027020/c9c1f25e-a6b1-4572-b48e-9bc34adbc0e3)





        
