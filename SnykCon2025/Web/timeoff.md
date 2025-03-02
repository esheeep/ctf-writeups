# TimeOff
![CleanShot 2025-03-02 at 14 47 48@2x](https://github.com/user-attachments/assets/5c2a2de5-7835-4b73-be03-0b901023f5a6)

Attachment: [timeoff.zip](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/timeoff.zip)

## Writeup
The web app allows users and admin to request time off. When creating a "New Time Off Request" a file can be uploaded.
There are 2 store values when uploading the file to take note, the "Name" and the "Stored Name". 
The name is the actual name of the file that being uploaded and the Stored name is what the user input for the name of the file. <br>
![CleanShot 2025-03-02 at 14 55 09](https://github.com/user-attachments/assets/08b688d3-5629-420e-ac1f-2e121640f6e0)<br>

When trying to download the upload file there is an error that shows the path of where the document is stored. <br>
![CleanShot 2025-03-02 at 14 57 28](https://github.com/user-attachments/assets/c97ec07e-63e4-44c5-8353-7b4e626652e9)

The file is stored as the Name and the link to download the file is using the Stored name.
Naming the Stored name to a path traversal can potentially get access to the flag.

Uploading a text file with path traversal payload: <br>
![CleanShot 2025-03-02 at 15 07 09](https://github.com/user-attachments/assets/9a7118ab-0e3c-4ac6-a667-4334153126b5) <br>

Downloading the document returns a flag: <br>
![CleanShot 2025-03-02 at 15 18 50](https://github.com/user-attachments/assets/249b2821-e7aa-4860-b9fd-acaa1f2afa1a) <br>

Flag: flag{52948d88ee74b9bdab130c35c88bd406}
