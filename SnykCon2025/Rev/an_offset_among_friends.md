# An Offset Amongst Friends
![CleanShot 2025-03-01 at 22 05 33](https://github.com/user-attachments/assets/cd716b40-d48f-4fa1-8b7f-b3194bab872f)

Attachment: [an-offset](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/an-offset)

## Writeup
Executing the binary, it asks for the password: <br>
![CleanShot 2025-03-01 at 22 19 34](https://github.com/user-attachments/assets/93b83774-6f7a-4ed8-953d-c33612492ea6)

Running `ltrace` on the binary to break it down: <br>
![CleanShot 2025-03-01 at 22 23 09](https://github.com/user-attachments/assets/7c3cef14-89ee-4f06-bff4-8c53813d8cac)

It shows that incorrect password is compared to `unlock_me_123`. This may be the password. <br>

Entering `unlock_me_123` as a password: <br>
![CleanShot 2025-03-01 at 22 23 09](https://github.com/user-attachments/assets/bf3e540c-1da1-4d0e-818a-434211c24eea)

Flag: flag{c54315482531c11a76aeaa828e43807c}
