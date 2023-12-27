# Broken Firebase Database [200 pts]
> Firebase services are so popular. SaaS service like this is probably secure. Prove it wrong, in the case that it is insecurely misconfigured, like in this mobile application. <br>
> Flag format: WTCTT2023_FINAL_MOB03{[a-z0-9]{32}} <br>
> File: https://pub-0ac4dc5a7c7f4ec8af9e32a5bdb28c6b.r2.dev/sh.sth.secguy888v2.apk <br>
> SHA-256 Checksum: 2f5b23f368c378932f7836b33f1cff29dfc08f0ec2a25e7dc08ca8a80f7e6c15

ในไฟล์ "strings.xml" เจ้าเก่าเจ้าเดิมก็ได้ให้ hint เกี่ยวกับ FLAG3 มาไว้
```xml
<string name="FLAG3_HINT">https://wtctt2023-secguy888-default-rtdb.asia-southeast1.firebasedatabase.app</string>
```
ซึ่งเจ้าตัวนี้จะเป็น URL Firebase Database ที่ดูทรงแล้วอาจจะมีการ misconfig กันเกิดขึ้นที่อาจทำให้เรามี unauthorized read permission ที่จะสามารถเจข้าไปอ่านไฟล์ข้อมูลใร Firebase Database ได้ <br>
หนึ่งในวิธีการ enum ช่องโหว่นี้ก็จะเป็นการเติม `.json` เข้าไปท้าย URL ดังกล่าวเป็น
```
https://wtctt2023-secguy888-default-rtdb.asia-southeast1.firebasedatabase.app/.json
```
เราก็จะสามารถเข้าถึง Firebase DB ได้พร้อมกับ flag ที่ตามหา

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/31000f58-1e06-4234-8450-627c1782c95a)

FLAG: WTCTT2023_FINAL_MOBILE03{c26b6c66a97254d3756bced76438e571}
