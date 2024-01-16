# XOR One-2-N [100 pts]
> เพื่อนสาวฉันสอนให้เข้ารหัสลับด้วยวิธีการแบบ XOR ฉันเริ่มเรียนรู้จากการเริ่มต้นนับ 1 ถึง N แต่ฉันก็ยังงงอยู่ดี ใครก็ได้ช่วยฉันที

จากโจทย์ก็ตรงตัว cipher text ที่เราได้มาถูกเข้ารหัสแบบ XOR จาก 1 to N เป็น format pattern ตามนี้
> char(N) ^ char(index[n-1])

ก็เลยทำการเขียน python script ไป reverse algorithm จาก 1-to-N เป็น N-to-1 เพื่อเอา flag ออกมา

```python
enc = open('xor_one_2_n.txt').read().strip()
enc = bytes.fromhex(enc).decode()
flag = ''

for i in range(len(enc)):
    flag += chr(ord(enc[i]) ^ (i+1))

print("The flag is " + flag)
```

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/4e58d9b1-9b4d-4ca0-a88f-e00dfd6492f7)
