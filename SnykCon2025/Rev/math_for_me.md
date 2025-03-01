# Math for me
![CleanShot 2025-03-01 at 23 14 44@2x](https://github.com/user-attachments/assets/6f345fd3-47a2-42d4-9836-6bd7b79a8abe)
Attachment: [math4me](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/math4me)

## Writeups
The binary expects a special number: <br>
![CleanShot 2025-03-01 at 23 16 50](https://github.com/user-attachments/assets/391e62fc-a90c-4754-a4bc-20b9395aca03)

Using `ltrace` to see what the program is doing:
![CleanShot 2025-03-01 at 23 19 53](https://github.com/user-attachments/assets/52b78911-1818-4898-9175-3980a45d55ec)

`ltrace` is not giving much clue. Instead use ghidra to get the main function.
![CleanShot 2025-03-01 at 23 31 11](https://github.com/user-attachments/assets/3254a08d-28d5-4eaf-806d-8292792dd93b)

In the main function there is a function `check_number()` that is used to validate the user input. 
![CleanShot 2025-03-01 at 23 32 56](https://github.com/user-attachments/assets/1071f805-596b-407b-b6b0-a61b710ff5fb)

This function takes an integer, calculate it and checks if it equals `0x34` which is hexadecimal for `52`. 
If two side are equal it will return 1, making `iVarl` = 1 which will execute the `else` in the main `if` function that generate the flag.
Solving the formula  `(param_1 * 5 + 4) / 2 == 52` -> (`param_1 = (52 * 2 - 4)/5 = 20`). Secret number is 20.
![CleanShot 2025-03-01 at 23 42 38](https://github.com/user-attachments/assets/29b8f2ce-0a5e-4815-ad83-aa29842f1a1d)

Flag: flag{h556cdd`=ag.c53664:45569368391gc}



