# Plantly
![CleanShot 2025-03-02 at 15 20 06@2x](https://github.com/user-attachments/assets/47805e8b-9659-46a4-83ea-43c686b5352c)
Attachment: [plantly.zip](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/plantly.zip)

## Writeup
Plantly is a shop with features like add to cart, subcription, sign in and etc. What stand out is a feature to "Add Custom Order to Cart":
![CleanShot 2025-03-02 at 15 29 21](https://github.com/user-attachments/assets/75770518-5e0f-46dd-a573-6d5cccde83a9) <br>
Going through the process of adding a custom order and placing the order, the app generates a recipt at `/recipt` that is reflects the name of the Custom order in the receipt.

![CleanShot 2025-03-02 at 15 32 25](https://github.com/user-attachments/assets/dc342d35-d11e-4dd6-9922-9e013c1c8669) <br>

Looking at the source code, file `store.py` , `receipt()` function, it shows that the app is vulnerable to SSTI, because the user-controlled input `purchase.custom_request` is use directly in the function `render_template_string()`. <br>
![CleanShot 2025-03-02 at 16 16 18](https://github.com/user-attachments/assets/908580fc-cc2f-4918-a8ff-0091c47ef430) <br>

Testing for SSTI with `{{ 2*3 }}`: <br>
![CleanShot 2025-03-02 at 15 42 42](https://github.com/user-attachments/assets/6f94f69b-d0dc-4101-ba0d-61f158ce4fb8) <br>
In the receipt is shows that the app calculated the Custom Request meaning it is indeed vulnerable to SSTI.

Wappalyzer tells that the app is built with Flask which means it is using Jinga2. If the template have access to Python's built-in functions this payload `{{ __import__('os').system('ls') }}` can be use to get RCE.
Injecting the payload returns an internal error because `os.system()` doesn't return the output to terminal instead of python. 
Trying this payload `{{ request.application.__globals__.__builtins__.__import__('os').popen('ls').read() }}` <br>
![CleanShot 2025-03-02 at 16 09 22](https://github.com/user-attachments/assets/e852ee4c-f5af-4794-898f-7779ad267b53) <br>

Was able to get RCE and in the list files there is the flag.txt. Simply run the payload again with `cat flag.txt`. `{{ request.application.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}` <br>
![CleanShot 2025-03-02 at 16 15 12](https://github.com/user-attachments/assets/b36ed059-f8ca-461b-8107-5a90ad1b5da2) <br>

Flag: flag{982e3b7286ee603d8539f987b65b90d4}



