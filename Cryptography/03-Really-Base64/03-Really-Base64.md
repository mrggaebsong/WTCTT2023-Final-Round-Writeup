## Really Base64 [200 pts]
> รหัสลับที่เห็นนั้นเข้ารหัสด้วยอัลกอริทึม Base64 จริงๆ นะไม่ได้โกหก

ในข้อนี้เราจะได้ cipher text ที่ถูกเข้ารหัสด้วย base64 แต่เป็น base64 ที่ออกมาไท้ยไทย
> ขจผ๗4ลฉงส๐นรฅมพงยสฝ๓ขสฝ๓ข๕ปงตมบว๒ยบ๓ขวผ๘ฉมฟท4๖ฉฆพคถฅน๓ญ๘ฮวญฤฝ๗ฎ๗ฑ๘ญฤฝ๑บฤขรฎ๗ญ๔ฏคยลฎดพฤฎคฒวญฤก๖ฎดฎลฏธ๕มฉณฅย==

และเราก็ได้ python script มาชุดหนึ่งที่อ่านแล้วจะเป็น script ใช้เข้ารหัสดังกล่าวนั่นเอง

```python
def char_gen():
    char = [chr(i) for i in range(ord('ก'), ord('ก') + 47)]
    char += [chr(i) for i in range(ord('๐'), ord('๐') + 10)]
    char += [chr(i) for i in range(ord('0'), ord('0') + 10)]
    char = char[:64]
    return char

def char_to_hex(msg):
    hex_data = ''
    for c in msg.encode():
        hex_data += hex(c)[2:]
    return hex_data

def base64_encode(msg):
    char = char_gen()
    msg = char_to_hex(msg)
    msg_num = eval('0x' + msg)
    base64 = ''
    while msg_num > 0:
        base64 = char[msg_num % 64] + base64
        msg_num = msg_num // 64
    
    base64 += '=='
    return base64

msg = input('Message: ')
base64 = base64_encode(msg)
open('enc.txt', 'wb').write(base64.encode('utf-16'))
```

เราก็เลย reverse script ให้กลับไป decode ข้อความดังกล่าวเพื่อถอด flag ออกมา จบ

```python
def char_gen():
    char = [chr(i) for i in range(ord('ก'), ord('ก') + 47)]
    char += [chr(i) for i in range(ord('๐'), ord('๐') + 10)]
    char += [chr(i) for i in range(ord('0'), ord('0') + 10)]
    char = char[:64]
    return char

def hex_to_char(hex_str):
    return bytes.fromhex(hex_str).decode('utf-8')

def base64_decode(base64_str):
    char = char_gen()
    base64_str = base64_str.rstrip('=')
    msg_num = 0
    for c in base64_str:
        msg_num = msg_num * 64 + char.index(c)
    hex_data = hex(msg_num)[2:]
    if len(hex_data) % 2 != 0:
        hex_data = '0' + hex_data
    return hex_to_char(hex_data)

base64_str = open('enc.txt', 'rb').read().decode('utf-16')
decoded_msg = base64_decode(base64_str)
print('Decoded Message:', decoded_msg)
```

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/e38db2e6-3f9a-44d4-927e-749a3f6061b7)
