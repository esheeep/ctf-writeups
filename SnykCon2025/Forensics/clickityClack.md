# ClickityClack
![CleanShot 2025-03-02 at 23 08 12@2x](https://github.com/user-attachments/assets/f7ff451b-fc5b-4c10-8cdb-b5e25d926aaf)
Attachment: [click.pcapng](https://github.com/esheeep/ctf-writeups/blob/main/SnykCon2025/Attachments/click.pcapng)

## Writeup
The files contain packets capture from a USB transfer. There are packets with HID data which are bytes from a device interacting with the computer. <br>
![CleanShot 2025-03-02 at 23 13 27](https://github.com/user-attachments/assets/afa21a23-3464-4ac3-875f-0d6dda97d713)
<br>
HID can be extracted using tshark.
```
tshark -r click.pcapng -Y "usbhid.data" -T fields -e usbhid.data > hid_data.txt
```
This gives a file of bytes. Based on the description these bytes are from a keyboard.

Using the a python code the bytes can converted into readable text:
```python
HID_KEYBOARD_MAPPING = {
    0x04: 'a', 0x05: 'b', 0x06: 'c', 0x07: 'd', 0x08: 'e', 0x09: 'f', 0x0A: 'g',
    0x0B: 'h', 0x0C: 'i', 0x0D: 'j', 0x0E: 'k', 0x0F: 'l', 0x10: 'm', 0x11: 'n',
    0x12: 'o', 0x13: 'p', 0x14: 'q', 0x15: 'r', 0x16: 's', 0x17: 't', 0x18: 'u',
    0x19: 'v', 0x1A: 'w', 0x1B: 'x', 0x1C: 'y', 0x1D: 'z', 0x1E: '1', 0x1F: '2',
    0x20: '3', 0x21: '4', 0x22: '5', 0x23: '6', 0x24: '7', 0x25: '8', 0x26: '9',
    0x27: '0', 0x28: '\n', 0x2C: ' ', 0x2D: '-', 0x2E: '=', 0x2F: '[', 0x30: ']',
    0x31: '\\', 0x33: ';', 0x34: "'", 0x35: '`', 0x36: ',', 0x37: '.', 0x38: '/'
}

# Mapping for shifted characters
HID_SHIFTED_KEYBOARD_MAPPING = {
    0x1E: '!', 0x1F: '@', 0x20: '#', 0x21: '$', 0x22: '%', 0x23: '^', 0x24: '&',
    0x25: '*', 0x26: '(', 0x27: ')', 0x2D: '_', 0x2E: '+', 0x2F: '{', 0x30: '}',
    0x31: '|', 0x33: ':', 0x34: '"', 0x35: '~', 0x36: '<', 0x37: '>', 0x38: '?'
}

def decode_hid_report(hid_report):
    bytes_data = bytes.fromhex(hid_report.strip())
    modifier_byte = bytes_data[0]
    key_code = bytes_data[2]

    # Check if the Shift key is pressed (bit 0x02 in the modifier byte)
    if modifier_byte & 0x02:
        return HID_SHIFTED_KEYBOARD_MAPPING.get(key_code, '')
    else:
        return HID_KEYBOARD_MAPPING.get(key_code, '')

def process_hid_file(file_path):
    decoded_string = ""
    with open(file_path, "r") as file:
        for line in file:
            decoded_string += decode_hid_report(line)
    
    print(decoded_string)

process_hid_file("hid_data.txt")
```
![CleanShot 2025-03-02 at 23 25 59](https://github.com/user-attachments/assets/afddb6c2-ad50-48a4-8041-43ea334a43a2) <br>
Flag: flag{a3ce310e9a0dc53bc030847192e2f585}
