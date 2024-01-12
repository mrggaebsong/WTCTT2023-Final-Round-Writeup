# Uptown Chunk [100 pts]
> สาวๆ จ๋า ในไฟล์รูปภาพดังกล่าว มีคำตอบซ่อนอยู่ หาซิ !!!?

เปิดรูปมา ไม่มีอะไร นอกจากป๋า Bruno Mars ในเอ็มวี Uptown Chunk

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/bfa3b031-3d42-4d17-a9be-a75e63218cad)

พอดูที่ชื่อโจทย์ที่แปลงจาก Uptown Funk เป็น Uptown Chunk เราก็อนุมานได้ว่าเราต้องไปเล่นที่ PNG Chunk เริ่มด้วยตรวจสอบ hex ที่ต้นไฟล์รูปก่อน

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/3af61812-a7d8-4091-879f-7153f609e3b0)

พอดูแล้วเอามาเปรียบเทียบกับ original PNG chunk เราจะพบว่าใน IHDR chunk จะมี bytes ขนาดภาพ (height x width) แตกต่างจาก original

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/58a07f8f-2d96-4bb3-9204-7816956a8473)

มาถึงตรงนี้เราสันนิษฐานแล้วว่าเราจะต้องไปแก้ byte height เพราะคาดว่า flag อาจจะซ่อนตามซอกหลืบตามขอบรูปใดๆ <br>
แต่ก่อนอื่น เราทำการแปลง hex ของความสูงภาพให้ออกมาเป็นขนาดจริงๆก่อน

```python
print(int("01f4", base=16))
# 500
```

เราจะรู้ได้ว่าขนาดรูปคือ 500 px เราก็ลองปรับความสูงเป็น 550 px แล้วแปลงกลับเป็น hex

```python
hex(550)
# '0x226'
```

ทีนี้ก็เปิดโปรแกรม HxD ใน Windows (ต้อง HxD Windows ด้วยนะ ทำ OS อื่นแล้วพัง) แล้วแก้ค่า height bytes เป็น 0226 เซฟไฟล์แล้วเราจะเจอ flag อยู่ที่ขอบรูปล่าง

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/98351269-7fe3-4f20-a271-68bd91252b82)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/78940b34-c5c7-495a-81a6-985ac16255c8)
