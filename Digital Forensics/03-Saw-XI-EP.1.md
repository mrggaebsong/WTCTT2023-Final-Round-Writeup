# Saw XI EP.1 [200 pts]
> ไฟล์ Memory Dump ไฟล์นี้ ได้ถูก Dump มาจากคอมพิวเตอร์เครื่องหนึ่งที่ถูก Ransomware เล่นงานทำให้ไม่สามารถใช้ทำงานต่อได้ แต่ว่าเจ้าของเครื่องได้ทำการเก็บข้อความที่เข้ารหัสลับไว้ในที่ๆ หนึ่งที่คล้าย “ลูกรูบิคสีฟ้าที่ยังประกอบไม่เสร็จ” อยากให้ท่านช่วยไปหาออกมาหน่อย :D <br>
> *** โปรดทำโจทย์ข้อนี้อย่างระมัดระวังเนื่องจากได้ dump ออกมาจากเครื่องที่ถูก Ransomware จริงๆ แนะนำให้ท่านแก้โจทย์ผ่าน Sandbox, VMware หรือ Virtual Machine เพื่อความปลอดภัย *** <br>
> Download file : https://drive.google.com/file/d/19QOlTOqfQZGy8kxQfPaU1W9GQy0zZcba/view <br>
> Achieved file password : infected

เราจะได้ memory file มา ซึ่งน่ากลัวมากเพราะ dump มาจาก ransomware infected machine จริงๆ ซึ่งตรงนี้เราควรทำ self-quarantine vm ให้เรียบร้อยก่อนเริ่มต้น malware analyse <br>
อ่านโจทย์ถึงคำใบ้ "รูบิคสีฟ้าที่ประกอบไม่เสร็จ" ซึ่งตรงนี้ก็หมายถึง Registry Editor แปลว่าเราต้องไปวิเคราะห์ memory dump เพื่อหารหัสลับใน registry <br>

ก่อนอื่นก็ใช้ volatility ไปดูข้อมูล memory ก่อน <br>
`python vol.py -f ../SAW.mem imageinfo`
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/8499ea3b-2996-40db-ac1b-f182d61dcdc0)

มานั่งดูผล เราจะพบว่า suggest profile ของ memory นี้มาจาก `Win7SP1x64` ทีนี้เราลอง printkey ที่เก็บใน memory ออกมา <br>
`python vol.py -f ../SAW.mem --profile=Win7SP1x64 printkey`
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/feb3df53-98c4-4d38-9722-2c25e717bc16)

เลื่อนลงมาเรื่อยๆจนมาเจอที่ไฟล์ `\??\Users\Mirthz\ntuser.dat` เราจะเจอ flag subkey "FLAG_HERE"
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/cc4cd1b0-820e-4f43-8b00-a44296805497)

ต่อไปเราต้อง dump hive เพื่อหา offset ของไฟล์ใน registry ด้วยคำสั่ง <br>
`python vol.py -f ../SAW.mem --profile=Win7SP1x64 hivelist`
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/a873bf6c-4bf3-4b91-8a8b-e40593debaf8)

พอเจอ offset ของไฟล์ ntuser.dat ที่ต้องการแล้ว นำ offset ไปค้นหา flag จาก subkey ในไฟล์ ntuser.dat จะได้เป็นข้อความ base64 <br>
`python vol.py -f ../SAW.mem --profile=Win7SP1x64 printkey -o 0xfffff8a0018fa010 -K "FLAG_HERE"`
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/56ad9a1c-3a10-4762-b4b3-671b85c776bf)

เอามา decode กลับก็จะได้ plain text flag ออกมา
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/68328c2e-5d90-49b1-bc70-4ae6e4c2d895)
