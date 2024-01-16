# SAW XI EP.2 [300 pts]
> จาก Saw XI EP.1 นั้น ทำให้ไฟล์ Text ในเครื่องถูกเข้ารหัสจนไม่สามารถเปิดดูได้ ฉะนั้นเป็นหน้าที่ท่านที่ต้องหาทางถอดรหัสไฟล์ text นั้น และท่านจะพบกับคำตอบ (แต่ก่อนจะได้ถอดรหัสไฟล์นั้น ท่านต้องหาไฟล์นั้นให้เจอก่อนนะ :) ) <br>
> *** ข้อนี้ให้วิเคราะห์ไฟล์ Memory Dump ไฟล์เดียวกันกับข้อ Saw XI EP.1 ***

ข้อนี้เราจะมาดูกันที่ MFT (Master File Table) ซึ่งเป็นที่รวบรวมข้อมูลของไฟล์ทั้งหมดในไดร์ฟนั้นๆ แม้แต่ไฟล์ที่ถูกลบไปแล้ว ก็เลยจะทำการตรวจสอบตัว MFT เพื่อค้นหาไฟล์ที่อาจจะถูกลบออกไป (aka flag file) และกู้คืนกลับมา
Ref: https://www.i-secure.co.th/2019/07/forensic-analysis-mft/

เราสามารถ scan หา MTF จากไฟล์ memory โดยใช้ volatility ด้วยคำสั่ง `python vol.py -f ../SAW.mem --profile=Win7SP1x64 mftparser > ../MFT.txt` <br>
เนื่องจากไฟล์มีขนาดค่อนข้างนานก็อาจจะใช้เวลาค่อนข้างนาน ระหว่างก็ไปพักกินน้ำปัสสาวะตามอัธยาศัย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/589faaf5-58c1-489b-b512-ed61123a278c)

พอเสร็จแล้วก็ได้ไฟล์ dump MFT เข้าไปเช็คไฟล์ก็จะเป็นไฟล์ข้อมูลจาก drive ที่ dump มาจากในเครื่อง <br>
เลื่อนๆลงไปจนถึง offset 0x2e9000 เราจะเจอ flag file ที่ถูก encrypt ไว้อยู่

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/e109f05a-842b-4090-a875-c1bf9d56167f)

เราจะเห็นส่วนของ $DATA อยู่ด้วย ว่าแล้วเราก็ copy ส่วนนี้มาใส่โปรแกรม HxD แล้วเซฟเป็น flag.txt.txt.fun

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/3b92c03d-a6ca-4787-9947-b2af0eb9d2eb)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/c2ecac8b-15e9-4067-89ec-f357b30382ee)

ดูที่ชื่อไฟล์เราจะเจอว่า format และ extension ของไฟล์เป็น `.fun` มันช่างไม่คุ้นเลยก็ลองเอาไป search google ดูหา malware family <br>
https://sensorstechforum.com/fun-files-virus-jigsaw-ransomware-remove/ <br>
เราจะพบว่า encrypted file ที่ extension `.fun` มาจาก "Jigsaw ransomware" ทีนี้เราก็ไปหาวิธีมา decrypt เจ้าไฟล์ที่ถูก ransomware นี้เข้ารหัส

จนกระทั่งเจอตัวโปรแกรม Jigsaw Decryptor โหลดมา add drive เป็นที่ที่เซฟไฟล์ fun เอาไว้แล้วถอดไฟล์ได้เลย <br>
Ref: https://www.emsisoft.com/en/ransomware-decryption/jigsaw/

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/e2f7c2fe-ee42-4283-a4ee-f4bdcd5b9d9d)

เราจะได้ไฟล์ดั้งเดิมที่ไม่ถูกเข้ารหัสกลับมา เปิดดูก็จะเจอ flag เรียบร้อย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/903c9253-4262-45f2-bb6b-ad58941f550d)

ปล. เรื่องนี้สอนให้รู้ว่า ใครมันสั่งมันสอนให้หาเอา ransomware จริงๆมาออกโจทย์วะไอ้ซั้ส!!!
