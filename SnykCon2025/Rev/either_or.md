# Either Or
![CleanShot 2025-03-01 at 22 42 01@2x](https://github.com/user-attachments/assets/57f2e8cf-2812-4010-bcbe-e60e452d5323)

[either-or](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/either-or)

## Writeup
Executing the binary it's expecting a secret word: <br>
![CleanShot 2025-03-01 at 22 43 59](https://github.com/user-attachments/assets/c9b55f5d-5646-4cb9-9f5f-6c29a403c54c)

Using `ltrace` to break down the binary: <br>
![CleanShot 2025-03-01 at 22 47 17](https://github.com/user-attachments/assets/b78d2870-a6bc-4fb9-b91a-68b5b33d1365)

`ltrace` shows that the program is using the function `strcmp()` to compare two values. If the two strings equal it returns a 0.
`abc` is encoded to `nop` and is compared to `frperg_cnffjbeq`.

What it looks like is characters are converted into another character `a` -> `n`, `b` -> `o`, `c` -> `p`. 

Running `ltrace` again but this time entering the secret code as the alphabet:
![CleanShot 2025-03-01 at 22 55 35](https://github.com/user-attachments/assets/adb561e7-4935-40ae-8b78-8871acd0b2bd)

This gives:
`abcdefghijklmnopqrstuvwxyz` -> `nopqrstuvwxyzabcdefghijklm`

Python code to convert the letters.
```python
unknown = "frperg_cnffjbeq"

MAPPING = {
    'a': 'n', 'b': 'o', 'c': 'p', 'd': 'q', 'e': 'r', 
    'f': 's', 'g': 't', 'h': 'u', 'i': 'v', 'j': 'w', 
    'k': 'x', 'l': 'y', 'm': 'z', 'n': 'a', 'o': 'b', 
    'p': 'c', 'q': 'd', 'r': 'e', 's': 'f', 't': 'g', 
    'u': 'h', 'v': 'i', 'w': 'j', 'x': 'k', 'y': 'l', 
    'z': 'm'
}


def decode(word):
    secret=""
    for c in word:
        secret += MAPPING.get(c, c)
    return secret

print(decode(unknown))
```
Decoding `frperg_cnffjbeq` returns `secret_password`.

Entering `secret_password` returns flag:
![CleanShot 2025-03-01 at 23 11 07](https://github.com/user-attachments/assets/d92ca44d-3da7-48c5-a534-375eaeb2af93) <br>

Flag: flag{f074d38932164b278a508df11b5eff89}



