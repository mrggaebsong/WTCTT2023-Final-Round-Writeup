# SQLi with no HMAC [100 pts]
> Vanilla SQL Injection without protection, with no other issues. <br>
> Flag Format: WTCTT2023_FINAL_WEB02{[a-z0-9]{32}} <br>
> URL: https://chall2.final.wtctt2023.p7z.pw

เข้ามาหน้า login ลอง login ด้วย user testuser01 จากข้อแรกแต่เข้าไม่ได้ ติด error invalid credential

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/965c4538-a1d6-4134-bb0e-d14b7bd59f2c)

ทีนี้ก็มาดูที่ request เราจะเห็นสิ่งที่น่าสนใจก็คือ parameter `app_session` ตรง Cookie header เป็น `app_session:guest%3Ainvalid` (aka guest:invalid) ที่ดูแล้วน่าจะลองเล่นท่า SQL injection ได้ <br>
(FYI: SQLi ทั่วไปที่เราเจอมักจะพบที่ URL or body parameter แต่จริงๆที่ cookie หรือ header อื่นๆก็สามารถโจมตีได้เหมือนกัน ควรทำการ enum ให้ละเอียดเรียบร้อยเพื่อให้แน่ใจก่อนโจมตี)

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/1a814cd1-7309-49e9-a70d-d87db35102a4)

ทีนี้ก็ลองสัก 1 payload ตัวอย่างดู error message ว่าจะมี SQL error มั้ย ถ้ามีก็นั่นแหละฮะ สามผ่าน

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/3d03220b-82a9-4278-9fd9-faec14168ba5)

เราจะเห็นว่ามี error message ที่ debug comment ว่า "SQLSTATE[HY000]: General error: 1 no such table: information_schema" <br>
เอาละ ทีนี้ก็การันตีได้แล้วว่าเราสามารถโจมตี SQLi ที่เว็บนี้และสามารถดึงข้อมูลจาก database ได้ แต่แค่ตอนนี้เรายังดึงไม่ถูก table เลยขึ้นมาแบบนี้

เอา error message ไปค้นอากู๋ก็จะเจอว่า database นี้เป็น "SQLite" ก็จะสามารถจำกัด payload scope ได้แล้ว <br>
Ref: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md <br>
step ต่อมาก็ลองหา table ใน database

```SQL
' UNION SELECT NULL,group_concat(tbl_name),NULL FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'--
```

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/cc2b8c6c-32d4-4264-9e3f-77870a8a74bc)

เราจะพบว่าใน database นี้มี 2 tables คือ members กับ flag ก็ไปดูที่ table flag ได้เลยจ้า

```SQL
' UNION ALL SELECT NULL,flag,NULL from flag--
```

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/51378af9-9c59-4990-9a51-9cf2493d4bd0)

FLAG: WTCTT2023_FINAL_WEB02{b99591f5cc683568119c2fd54429bdc1}

------

ปล. ถ้าเรามาสำรวจที่ table members เราจะเจอ username & password เป็น "testuser_008c5926ca861023c1d2a36653fd88e2" ซึ่ง hash นี้ถ้าเราเอาไป crack เราจะได้ password ของ testuser คือ "whatever"

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/24829194-17fa-4d6e-85cc-09be6a49e059)

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/91c23387-026e-4a11-997c-b5ac8b198422)

Login เข้าไปก็จะเจอหน้า index.php ที่บอกว่า flag in database ก็ต้องไป extract ต่อตามข้างบน จบ

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/c7d9ab85-18c8-48eb-b590-9f0f3d5ae429)

