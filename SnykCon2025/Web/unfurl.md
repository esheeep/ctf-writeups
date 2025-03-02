# Unfurl
![CleanShot 2025-03-02 at 14 02 48@2x](https://github.com/user-attachments/assets/3b387793-c253-4f56-9e84-19f7a66d43f5)

Attachment: [unfurl.zip](https://github.com/esheeep/ctf-writeups/blob/main/SnykCon2025/Attachments/unfurl.zip)

## Writeup
Unfurl is a web app that takes a url and returns the metadata for that url.
![CleanShot 2025-03-02 at 14 05 32](https://github.com/user-attachments/assets/08b708ee-ce9a-4cc1-a05e-2364ea238334)

The feature unfurl takes a URL as an user input this can open up to ssrf vulnerability. 
The challenge provides the source code which can be used to see how the app process the URL.
The URL is sent as a post request to the route `/unfurl`: <br>
![CleanShot 2025-03-02 at 14 11 09](https://github.com/user-attachments/assets/8051e912-d7c0-4657-992e-a0f3df22f076) <br>
In the file `publicRoutes.js` the routes takes the user input url as `const { url } = req.body;` then `await axios.get(url);` without validating or filtering the url meaning that it may be exploitable. <br>
![CleanShot 2025-03-02 at 14 16 23](https://github.com/user-attachments/assets/e8634ee3-775e-4767-bbbf-9a60cbe4f2c6) <br>

In the `admin.js` indicates that there is an admin page that can be acessiable internally with `http://128.0.0.1:${adminPort}`. 
The port is generated randomly using `Math.random()` between numbers 1024 and 4999.
Using intruder to find the admin on the vulnerable unfurl input field. 

Request for intruder: <br>
![CleanShot 2025-03-02 at 14 26 12](https://github.com/user-attachments/assets/76fb75b7-1bd1-4656-9905-49dd90704b05) <br>
Payload: <br>
![CleanShot 2025-03-02 at 14 27 13](https://github.com/user-attachments/assets/0006ed9e-e34a-475f-a0d3-ddb15425f1ff) <br>

Admin portal: <br>
![CleanShot 2025-03-02 at 14 34 39](https://github.com/user-attachments/assets/5edbf7c9-bc6b-4cb4-a3f1-18f741d193a5) <br>

In `adminRoutes.js` file theres a route that gives command execution: <br>
![CleanShot 2025-03-02 at 14 35 55](https://github.com/user-attachments/assets/8333f806-e30c-44ac-ba5f-419284be0a2b) <br>

Payload: `http://127.0.0.1:1122?/execute?cmd=cat%20flag.txt` <br>
![CleanShot 2025-03-02 at 14 45 32](https://github.com/user-attachments/assets/a58dcc12-9ae4-4c39-ae4c-1b362ebc5343) <br>
Flag: flag{e1c96ccca8777b15bd0b0c7795d018ed}

