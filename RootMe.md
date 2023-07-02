# Root Me
### Hello Hackers!
Today we will be completing a CTF from Tryhackme titled  <a href="https://tryhackme.com/room/rrootme">Root Me</a>. The CTF itself says that it is a beginner level CTF. So Let's get started.

* ### RECONNAISSANCE
  Firstly we will start with a `Nmap` scan on the target machine. <br>
`sudo nmap -sC -sV -T4 -p- <Machine_IP> -v`<br>
Now let's breakdown this command <br>
1. -sC : running default script scan.
2. -sV : will scan and display the versions of programs on the target machines.
3. -T4 : increase the speed of scan and makes it more agressive.
4. -p- : it is for scanning all the ports.
5. -v  : it is for making the scan verbose. <br> <br>
Here is the output of the scan : <br>
<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/3cf3135f-c026-41c3-9333-5d45226bbc6c"/> <br> <br>
This is enough to answer first 2 questions and move forward. From nmap results we can see that TCP port 80 is open . Let is visit it in the browser and explore. <br><br>
We are greeted by a landing page <br>
   
<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/e45a5149-f8e5-40be-828a-b461ea04e47a"/> <br>
On inspecting its source code we couldn't find any thing interested. Now let's move to next phase of enumeration and do directory busting on the target. We
will use `GoBuster` for it.<br>
   `gobuster -w /usr/share/wordlists/Discovery/Web-content/common.txt -u http://<Machine_IP>` <br>
Let's breakdown this simple command<br>

   1. -w : Path to your wordlist.
   2. -u : URL of the target<br><br>
   Here is the output of the scan: <br>
   <img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/fda54b8c-96a7-4d29-a3f3-b75298b1a06f"/> <br><br>
   Now we have the list of all the directories of the machine.<br>
   Now let's visit these directories one by one.
On visiting '/panel' we get a form . We might try to upload a file to get a reverse shell here.<br>
<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/f403fd84-de1b-4aca-a7cb-f228ffd80928"/> <br><br>
I have download a PHP reverse shell from Pentest Monkey (<a href="https://github.com/pentestmonkey/php-reverse-shell">Here.</a>) and made necessary changes to it.<br>
Upon uploading it we get a error which indicates that we cannot upload a file with `.php` extension.

<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/86279891-e82a-4d86-9d11-1793c5b3d361"/> <br><br>
Now let's try by changing the extension from `.php` to `.phtml`. <br>
<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/5f7c6372-3c89-4da1-8f0e-49ec76cbae7e"/> <br><br>
And VOILA! We are successful. <br>
From gobuster scan we also learned that there is a directory '/uploads' from where we might run the shell. So start a `netcad` listener .<br>
`nc -nvlp <port_no>`<br>
Now let's break this down: <br>
1. -n : not to perform name resolution.
2. -v : verbose
3. -l : listen for incoming network.
4. -p : port number to listen on.<br>
Now let's run the shell and try to get a reverse shell. <br>
<img height="500px" width="500px" src="https://github.com/adityakhattri21/Maverick-Tryhackme/assets/101019545/00a3eb5e-2c6a-481d-b135-d2373cf387d9"/> <br><br>


   
