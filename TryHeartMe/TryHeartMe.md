I used nmap to scan the target and found 2 services are running
- 22 - TCP
- 5000 - HTTP

Starting Nmap 7.99 ( https://nmap.org ) at 2026-07-14 05:23 -0400
Nmap scan report for 10.48.152.97
Host is up (0.078s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 aa:64:ea:9d:42:14:36:6f:66:ee:e8:ae:9f:f3:92:fd (ECDSA)
|_  256 18:84:c2:eb:fb:5c:a2:64:92:03:f0:1c:18:23:08:96 (ED25519)
5000/tcp open  http    Werkzeug httpd 3.0.1 (Python 3.12.3)
|_http-title: TryHeartMe \xE2\x80\x94 Shop
|_http-server-header: Werkzeug/3.0.1 Python/3.12.3
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Port 5000 (http)

When I visit the website there is an option to login and register..when I create an account then I am assigned with an jwt token.

When I decode the jwt cookie on online website (jwt.io) there is an field named as role and another field named as credit ( it is used to buy gifts from the website)

![[Pasted image 20260714060338.png]]

I changed the role from user to admin and changed the credit score from 0 to 5,000

After changing this things I go to dev tool and replaced the cookie with the other jwt cookie which I crafted..

![[Pasted image 20260714060626.png]]

And now I am able to see the hidden gift and when I click on it, BOOM !! Flag appears on my screen

![[Pasted image 20260714060756.png]]


**FLAG:** **THM{v4l3nt1n3_jwt_c00k13_t4mp3r_4dm1n_sh0p}**

Bug I exploited in this challenge: JWT Token Manipulation

> The application used a JSON Web Token (JWT) for authorization. After decoding the token, I modified the `role` claim from `user` to `admin` and re-encoded it. Since the server failed to properly validate the JWT signature, it accepted the forged token, resulting in an authorization bypass and granting administrative privileges.

**Vulnerability:** JWT Authorization Bypass due to Improper Signature Verification.

**Tools used in this challenge:**
- Nmap
- JWT token decoder
