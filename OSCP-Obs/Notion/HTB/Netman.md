  

it is a CMS, so, study its conf

## **Prog**ram **direc**tory

**B**y **defa**ult, **th**e **PR**TG **set**up **prog**ram **sto**res **th**e **co**re **instal**lation **i**n **on**e **o**f **th**e **follo**wing **direct**ories:

```Plain
%programfiles%\PRTG Network Monitor
```

**o**r

```Plain
%programfiles(x86)%\PRTG Network Monitor
```

  

**A**s **yo**u **ca**n **se**e **th**e **pass**word **cont**ains `2018` **an**d **al**so **b**y **th**e **da**te **o**f **th**e **fi**le `07–14–18 03:13AM 1153755 PRTG Configuration.old.bak`

**Le**ts **tr**y **th**is **pass**word **wi**th **20**19 **inst**ead!

  

~ searchsploit PRTG

---

Exploit Title | Path

---

PRTG Network Monitor 18.2.38 - (Authenticated) Remote Code Execution | windows/webapps/46527.sh  
PRTG Network Monitor 20.4.63.1412 - 'maps' Stored XSS | windows/webapps/49156.txt  
PRTG Network Monitor < 18.1.39.1648 - Stack Overflow (Denial of Service) | windows_x86/dos/44500.py  
PRTG Traffic Grapher 6.2.1 - 'url' Cross-Site Scripting | java/webapps/34108.txt

---

  

  

The `smbmap` command you're using seems to have an issue with how the password is being handled, particularly with the `dquote>` prompt indicating an unfinished or improperly closed string.

When using special characters or spaces in a password, it’s a good practice to escape them or enclose the password in single quotes. Here’s how you can correctly format the command:

run the script to create a new user 'pentest' in the administrators group with password 'P3nT3st!'

```Shell
smbmap -H 10.10.10.152 -u pentest -p 'P3nT3st!'

```

Alternatively, you can escape the special characters within the double quotes:

```Shell
bash複製程式碼
smbmap -H 10.10.10.152 -u pentest -p "P3nT3st\!"

```