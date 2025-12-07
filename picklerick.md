Hello folks, I’m Yousef Elnagar AKA (MrDurdenEG)
# Pickle Rick CTF Write-up

A cybersecurity enthusiast and beginner CTF player. In this write-up, I'll walk you through my journey solving the **Pickle Rick** room, explaining my thought process and how I got the final ingredients.

![Room Banner](image.png)


First, I opened the web application:

![Homepage](Screenshot%202025-12-07%20154401.png)


I viewed the page source:

![Source Page](Screenshot%202025-12-07%20154449.png)

Found a username in the HTML comments:

![Username Found](Screenshot%202025-12-07%20154455.png)

**Username:** `R1ckRul3s`


I noticed a path named `/assets`:

![Assets Directory](image-1.png)

Nothing useful here - just images and scripts.


I tried opening `/robots.txt`:

![Robots.txt](Screenshot%202025-12-07%20155316.png)

Found an interesting string (possibly a password):

**Wubbalubbadubdub**


I used `ffuf` to enumerate directories:

```bash
ffuf -u http://10.82.168.227/FUZZ -w /root/Desktop/Tools/wordlists/SecLists/Discovery/Web-Content/common.txt -e .php,.html,.txt -mc 200
```

- `-e .php,.html,.txt` : Find paths with these extensions
- `-mc 200` : Filter paths with HTTP 200 status

![Fuzzing Results](Screenshot%202025-12-07%20143812.png)

**Discovered files:**
- `index.html`
- `login.php`
- `robots.txt`

## Login Panel

Accessing `http://10.82.168.227/login.php` revealed a login page:

![Login Page](Screenshot%202025-12-07%20155355-1.png)

Tried the credentials:
- **Username:** `R1ckRul3s`
- **Password:** `Wubbalubbadubdub`

Success! Gained access to a command panel:

![Command Panel](image-2.png)

## Command 

### Listing Files

I ran `ls -la`:

![Directory Listing](image-3.png)

**Interesting files found:**
- `Sup3rS3cretPickl3Ingred.txt`
- `clue.txt`

### Bypassing Command Restrictions

The `cat` command was disabled:

![Cat Disabled](Screenshot%202025-12-07%20155510.png)

**Solution:** Access files directly via URL

### First Ingredient 

`http://10.82.168.227/Sup3rS3cretPickl3Ingred.txt`

![First Ingredient](image-4.png)

**First Ingredient Found!**

### Clue File

`http://10.82.168.227/clue.txt`

![Clue](image-5.png)

Nothing particularly useful.

### Base64 Rabbit Hole

Found a Base64 string in the page source:

![Source Comment](image-6.png)

```html
<!-- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== -->
```

After decoding 6 times:

![Decode Result](Screenshot%202025-12-07%20144505.png)

Result: **"rabbit hole"** - A dead end!

## System Exploration

### Current Directory

Checked working directory with `pwd`:

![PWD](image-7.png)

### Root Directory

Navigated to root and listed contents: `cd /../../ && ls -la`

![Root Directory](image-8.png)

**Important directories:** `/home`, `/root`

### Home Directory

Explored home: `cd /home && ls -la`

![Home Directory](image-9.png)

Found `rick` directory!

### Rick's Directory

`cd /home/rick && ls -la`

![Rick's Directory](image-10.png)

Found: **"second ingredients"** file

### Second Ingredient 

Since `cat`, `head`, `tail`, and `more` were disabled, I tried `less`:

```bash
less '/home/rick/second ingredients'
```

![Second Ingredient](image-11.png)

**Second Ingredient:** `1 jerry tear`

## Root Directory Access

### Permission Issues

Tried listing `/root` but noticed permissions: `drwx------ 4 root root`

Only root has access!

### Using Sudo

`sudo ls -la /root`

![Root Contents](image-12.png)

Found: `3rd.txt`

### Third Ingredient

```bash
less /root/3rd.txt
```

![Third Ingredient](image-13.png)

**Third Ingredient:** `fleeb juice`

## Summary

**All Three Ingredients Collected:**
1.  `mr. meeseek hair` (from `Sup3rS3cretPickl3Ingred.txt`)
2.  `1 jerry tear` (from `/home/rick/second ingredients`)
3.  `fleeb juice` (from `/root/3rd.txt`)



