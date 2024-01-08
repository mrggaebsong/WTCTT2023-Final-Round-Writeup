# Dark Cloud [200 pts]
> ฉันไม่เคยเปิดไฟล์ที่เก็บอยู่ใน Dark Cloud ได้เลยแม้แต่ครั้งเดียว มันเกิดอะไรขึ้น <br>
> http://drive.qrsskjtihiuyypxksogu5mvojeydcdbfhjtq533wqjrpc3clpzahfbid.onion/download-page/6584657ae47fbe76ba587620/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2NTcyZjRlZWU0N2ZiZTc2YmE1ODYwMTciLCJpYXQiOjE3MDMxNzU1Nzl9.xK9NiqL-xbWEjWQ1eRmnwJfVtxiMbg76F5M937aYusA

ลองดูที่ URL ที่ได้มา สังเกต domain ที่มีคำว่า .onion ซึ่งจะสื่อถึง Onion service ที่มีใน Tor Browser <br>
เพราะฉะนั้น เราต้องเข้า link นี้ใน Tor Browser ถึงจะเข้าได้

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/fb028ea9-9e16-445c-b430-a39cda7e0464)

พอเข้า link มาแล้ว ก็จะได้ไฟล์ Powerpoint มาไฟล์นึง download ลงมาแล้วเปิดไฟล์ ก็จะเจอ "WCTF23{fake_flag}" เข้าเต็มหน้าสไลด์

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/9b743a52-e7a2-41a6-a7b7-66bc34e2acc6)

ซึ่งลองดูตามโน้ตหรือเปิดสไลด์โชว์ ดูทุกซอกทุกมุมแล้วก็ไม่เจอ clue ที่ไปหา flag ได้เลย ก็เหลือทางสุดท้ายคือ แปลงไฟล์จาก PPT เป็นไฟล์ zip แล้วแตกไฟล์ออกมา

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/ab22a527-a331-4f73-bf74-ee4026b8b550)

พอแตกไฟล์ออกมาก็จะเจอไฟล์ที่ซ่อนอยู่ข้างในจำนวนนึง เข้าไปดูข้างในเราก็จะเจอ flag ในรูปในโฟลเดอร์ "/ppt/media/image4.png"

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/7b2df8da-f9b6-49c1-ae5f-59ca8aec921d)

FLAG: WCTF23{0f7c01fcae806128494cf799171683c4}
