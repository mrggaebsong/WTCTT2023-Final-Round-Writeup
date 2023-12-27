# Hidden Message in APK v2 [100 pts]
> A malicious mobile application, SecGuy 888 v2, contains a secret flag 1 inside. Extract the apk file and find it. <br>
> Flag format: WTCTT2023_FINAL_MOB01{[a-z0-9]{32}} <br>
> File: https://pub-0ac4dc5a7c7f4ec8af9e32a5bdb28c6b.r2.dev/sh.sth.secguy888v2.apk <br>
> SHA-256 Checksum: 2f5b23f368c378932f7836b33f1cff29dfc08f0ec2a25e7dc08ca8a80f7e6c15

เปิด JADX-GUI แล้ว import file APK เข้ามา เข้าไปที่ `Resource/resources.arsc/res/values/strings.xml` เราจะเห็น FLAG1 ใน strings แต่ว่าดั๊นโดนกันซีนซะก่อนเพราะไม่ออก flag มาตรงๆ
```xml
<string name="FLAG1_IN_drawable">REMOVED_FOR_SECURITY_REASONS</string>
```
แต่ๆๆๆว่า มันมี hint ใน string คำว่า `FLAG1_IN_drawable` แปลว่าเบาะแสต่อไปของเราก็คือ `Resource/res/drawable` <br>
เข้า folder drawable เลื่อนลงไปให้สุด แล้วจะเจอไฟล์รูป "uat_leftover_flag1.jpg" และ flag ที่ตามหา

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/bd7ade2e-7950-4b3a-a846-8e2caac5e7cc)

FLAG: WTCTT2023_FINAL_MOBILE01{6979429966290e11c7401ae5b94f67d9}
