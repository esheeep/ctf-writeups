# Zero Ex Six One
![CleanShot 2025-03-02 at 21 48 00@2x](https://github.com/user-attachments/assets/783c54a2-0646-4abe-bfb1-2f396a479e9f)
Attachment: [flag.txt.encry](https://github.com/esheeep/ctf-writeups/blob/main/SnykCon2025/Attachments/flag.txt.encry)

# Writeup
Challenge gives the file with hex string:
```
0x070x0d0x000x060x1a0x020x540x510x050x590x530x020x510x000x530x540x070x520x040x570x550x550x050x510x560x510x530x030x550x500x050x030x050x510x590x540x000x1c
```
The challenge description suggest that this is encrypted by XOR. XOR is a sysmmetric encryption that uses the same key to encrypt and decrypt.
The title of the challenge hints that the key maybe `0x61`. 

Using cyberchef first the hex string need to be decode to bytes because XOR can only decrypts bytes.
Then do XOR decryption with key `0x61`. <br>
![CleanShot 2025-03-02 at 22 02 20@2x](https://github.com/user-attachments/assets/686bdfc8-0d94-4b17-a23a-467b9ddaac5a)

Flag: flag{c50d82c0a25f3e644d0702b41dbd085a}

