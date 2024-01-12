# Hidden Media [100 pts]
> เกิดมาเป็นคนสวยทั้งทีก็ต้องมีแอบเก็บของดีเอาไว้บ้างหละ แล้วไหนหละของดีที่ว่าหนะ

เปิดโจทย์มา เราจะได้รูปภาพ PNG มารูปนึง

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/b7f9e439-09b5-4134-975f-75d418e01684)

```bash
$ file hidden_media.png
hidden_media.png: PNG image data, 1280 x 720, 8-bit/color RGB, non-interlaced
```

เช็ค file signature ของรูป เราจะพบว่า trailer (16 bytes สุดท้ายของรูป) ไม่ใช่ PNG format
> PNG Magic Bytes <br>
> Header: `8950 4e47 0d0a 1a0a` <br>
> Trailer: `4945 4e44 ae42 6082`

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/1f142355-dfbd-4623-b49c-021fc86f4815)

เราก็สงสัยว่ามีไฟล์ซ้อนหรือมี garbage bytes หรือเปล่า ทีนี้เราก็เปิด Hex Fiend ขึ้นมาแล้วหาตัว PNG trailer เราจะเจอมันแต่ว่ามีอีกแสนล้าน bytes จ่อก้นอีกเป็นพรืด

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/6434b352-8a2a-4f00-b5a6-1d3a77f0c348)

ซึ่งถ้าเราสังเกตที่ด้านขวาที่เป็น decoded text เราจะเห็น header mp4 ซึ่งบ่งบอกได้ว่าไฟล์ PNG นี้มีไฟล์ MP4 ซ้อนอยู่ด้วย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/289b3fba-3b90-47ce-80eb-93d47ad6d17e)

ซึ่งตอนแรกจะลอง binwalk/foremost ดูแต่เหมือนจะโดนดักให้ไปท่านั้นไม่ได้ เลยแตกมือด้วยการลบก้อน bytes PNG ออกให้หมดแล้วเซฟใหม่เป็นไฟล์ MP4 เปิดคลิปมาเราก็จะเจอ flag ในคลิป

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/aa702a44-5091-4ec7-9307-06afd10daf31)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/d5512992-7df0-43d2-8792-769809134540)
