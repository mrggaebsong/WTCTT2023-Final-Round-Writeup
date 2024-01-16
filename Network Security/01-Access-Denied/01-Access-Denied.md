# Access Denied [100 pts]
> เพื่อนสาวบอกฉันว่าหน่วยงานของนางถูกโจมตี เพราะนางดันตั้งรหัสผ่านของ Admin เป็น Flag ของรายการ WCTF23 นางเลยกรองข้อมูล Log เฉพาะช่วงที่โจมตีไม่สำเร็จส่งมาให้ฉันดู ฉันวิเคราะห์เบื้องต้นแล้วเจอ Flag เต็มไปหมดเลย แล้วฉันจะรู้มั้ยว่าบรรทัดไหนคือ Flag ที่ถูกต้อง

เปิดไฟล์ log ขึ้นมา เราจะเจอกับ access log ที่ทุกตัวโดน mark as "Access Denied" <br>
จุดที่น่าสนใจคือ password ที่ใช้ brute-force เข้า admin จะเป็น flag pattern md5 ทั้งหมด

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/cfbd33f2-0ffe-4f3f-88b1-70e77cdf8a41)

ทีนี้เลยลองตัดเฉพาะส่วนของ md5 ออกมาเพื่อจะเอามา crack หารหัสที่ถูกต้อง

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/a2a75518-c0cc-46f8-ae9c-9c538d497401)

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt md5_flag_out.txt --format=raw-md5
john --show md5_flag_out.txt --format=raw-md5
```

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/65342b82-9a6a-461e-b452-6a819de48965)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/db9de3f8-bbc2-49c4-b1b5-1b1709d95f31)

ตอนแรกก็พยายามหาว่า hash ตัวไหนที่ crack ไม่ออกก็น่าจะเป็นคำตอบ แต่สรุปโดนคนออกโจทย์สับขาหลอก เพราะทั้งหมดในนี้ไม่ใช่คำตอบ (เอ้า) <br>
คำตอบจริงๆก็คือ ถ้าเราสังเกตที่บรรทัดสุดท้ายของ log file รหัสที่เราถอดได้จะเป็น noemi ซึ่งถ้านี่เป็น last access denied แปลว่ารหัสหลังจากนี้จะเป็นรหัสที่ถูกต้อง (ใครจะไปรู้วะเนี่ยยยยยย) <br>
ถ้าบรรทัดสุดท้ายเป็น noemi ให้ไปดูในไฟล์ rockyou.txt ยอดฮิตของตายเวลา crack รหัส บรรทัดถัดไปจะเป็นคำว่า "luvbug" เอาไปเข้า md5 ก็จะเป็น flag ที่แท้จริง

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/3559626c-0e3d-4136-8ca6-891a6803c635)
