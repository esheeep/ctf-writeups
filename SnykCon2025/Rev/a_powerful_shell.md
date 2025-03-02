# A powerful shell
![CleanShot 2025-03-02 at 23 39 59@2x](https://github.com/user-attachments/assets/bf625b3a-face-41c7-9afb-e3c626e7192e)
Attachment: [challenge.ps1](https://github.com/esheeep/ctf-writeups/blob/main/SnykCon2025/Attachments/challenge.ps1)
# Writeup
The given file has a base64 string. Using cyberchef the string can be decoded to:
```
$decoded = [System.Convert]::FromBase64String('ZmxhZ3s0NWQyM2MxZjY3ODliYWRjMTIzNDU2Nzg5MDEyMzQ1Nn0=')
$flag = [System.Text.Encoding]::UTF8.GetString($decoded)

# Only show flag if specific environment variable is set
if ($env:MAGIC_KEY -eq 'Sup3rS3cr3t!') {
    Write-Output $flag
} else {
    Write-Output "Nice try! But you need the magic key!"
}
```

In the output there is another base64 string. `ZmxhZ3s0NWQyM2MxZjY3ODliYWRjMTIzNDU2Nzg5MDEyMzQ1Nn0=`
Decoding that gives the flag. <br>
![CleanShot 2025-03-02 at 23 50 40@2x](https://github.com/user-attachments/assets/475fca5f-7ba6-45a9-94b5-dcb43ab4597e) <br>
Flag: flag{45d23c1f6789badc1234567890123456}
