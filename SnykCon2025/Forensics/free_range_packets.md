# Free range packets
![CleanShot 2025-03-02 at 22 06 20@2x](https://github.com/user-attachments/assets/5dc8fc2e-78f4-40d9-9546-d77904de00d2)

Attachment: [freeRangePackets.pcapng](https://github.com/esheeep/ctf-writeups/edit/main/SnykCon2025/Attachments/freeRangePackets.pcapng)

# Writeup
There are bluetooth packets sending and receiving. Looking at Bluetooth L2CAP Protocol in wireshark it has a hexadecimal payload.
Converting the hexadecimal to ASCII there is only one printable character and that is at position 7 and 8. <br>
![CleanShot 2025-03-02 at 22 37 58](https://github.com/user-attachments/assets/18a3ef78-7cf9-4e78-90af-8830f47d15f3)
<br>
Can get the flag by using tshark to extract the `btl2cap.payload`, remove non printable hex with cut and convert to ASCII using `xxd`.
```bash
tshark -r freeRangePackets.pcapng -Y "btl2cap.payload" -T fields -e btl2cap.payload | cut -c7-8 | xxd -r -p
```
![CleanShot 2025-03-02 at 23 04 29](https://github.com/user-attachments/assets/050544d1-36dc-45dd-b2bb-532140644d98)

Flag: flag{b5be72ab7e0254c056ffb57a0db124ce}
