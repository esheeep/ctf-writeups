# Weblog
![CleanShot 2025-03-03 at 00 52 20@2x](https://github.com/user-attachments/assets/c9337df4-127e-43a9-b842-ebe119b00171)

Attachment: [weblog.zip](https://github.com/esheeep/ctf-writeups/blob/main/SnykCon2025/Attachments/webloag.zip)

## Writeup
Challenge is a blog and you need to register an account to login to view blogs. <br>
![CleanShot 2025-03-03 at 00 00 04](https://github.com/user-attachments/assets/661bddc6-85fc-40b8-9f45-3657dc70a701)

There are two things that pop out that may be vulnerable the search function and the create post function.
Source code shows that the search function is vulnerable to SQL injection because it uses user-controlled input directly in the SQL statement to search for blogs. <br>
![CleanShot 2025-03-03 at 00 03 48](https://github.com/user-attachments/assets/9a095a83-a2ea-4750-8273-38c1f8bffe3c)
<br>
Testing for SQL injection using payload `' OR 1=1 `: <br>
![CleanShot 2025-03-03 at 00 07 11](https://github.com/user-attachments/assets/ead7d7c5-db7b-460a-aa8f-3f7ab9ae6c3b)

It is indeed vulnerable to SQL injection because of the error thrown showing the server trying to process the injected SQL.
This search function only query the blog_posts table. In the source code, file `models.py` shows there is another users table that contain username and password.
Getting access to this information can possibly allow previlage escalation to admin user. The information in models.py can be used to construct a UNION payload. <br>
![CleanShot 2025-03-03 at 00 14 38](https://github.com/user-attachments/assets/ade7d7ef-efb0-45dc-bfa2-9b417b9ff041) <br>

Payload: `' UNION SELECT id, username, password, email, role FROM users --  ` <br>

![CleanShot 2025-03-03 at 00 20 16](https://github.com/user-attachments/assets/f9474df6-22f9-4018-b983-47722ce724fa) <br>

The SQL injection returns the admin credentials with password being `c1b8b03c5a1b6d4dcec9a852f85cac59`.
Base on the source code we know that the password are hashed with md5. <br>
![CleanShot 2025-03-03 at 00 24 15](https://github.com/user-attachments/assets/7a0a719c-ee64-4efc-a114-b703f52a2008) <br>

Cracking the password using hashcat.
```
hashcat -m 0 -a 0 c1b8b03c5a1b6d4dcec9a852f85cac59 /wordlist/rockyou.txt
```
* -m (hash mode): 0 is md5
* -a (attack mode): 0 is dictionary attack

![CleanShot 2025-03-03 at 00 46 27](https://github.com/user-attachments/assets/4430b82d-f44a-4fb9-bfc5-a6c55bc3a478) <br>
Logging into admin and accessing admin panel there is a Rebuild Database Command that takes user input and execute the command. 
This part is vulnerable to command injection, injecting a simple payload like cat flag.txt is not possible because the server expects the default command first. <br>
![CleanShot 2025-03-03 at 00 49 53](https://github.com/user-attachments/assets/17d560df-aa3b-47e2-974b-f0fa66570aae)

In that case the payload should just append the existing command to get the flag. 
```
echo 'Rebuilding database...' && /entrypoint.sh
```

![CleanShot 2025-03-03 at 00 51 45](https://github.com/user-attachments/assets/c764f38d-f7b9-4f40-8b2a-4078834f6ee9) <br>

Flag: flag{b06fbe98752ab13d0fb8414fb55940f3}


