# TOTP Token Generator v2 [100 pts]
> SMS OTP is not recommended. So, SecGuy 888 v2’s admin secretly uses TOTP. However, a TOTP secret is leaked, and you need to find a way to utilize it in order to get the flag 2. <br>
> Flag format: WTCTT2023_FINAL_MOB02{[a-z0-9]{32}} <br>
> File: https://pub-0ac4dc5a7c7f4ec8af9e32a5bdb28c6b.r2.dev/sh.sth.secguy888v2.apk <br>
> SHA-256 Checksum: 2f5b23f368c378932f7836b33f1cff29dfc08f0ec2a25e7dc08ca8a80f7e6c15 <br>

ต่อจากข้อ 1 เราจะได้เห็น strings บรรทัดถัดมาที่บอกใบ้เราเกี่ยวกับ FLAG2
```xml
<string name="FLAG2_HINT2">TRY_TO_GET_OTP_AND_USE_API</string>
<string name="FLAG2_HINT3">Authorization: Basic YWRtaW46MTIzNDU2</string>
<string name="FLAG2_SEE_CODE">REMOVED_FOR_SECURITY_REASONS</string>
```
ท่าในการแก้โจทย์ข้อนี้จะคล้ายๆกับรอบคัดเลือก แต่ครั้งนี้เราจะไม่ได้ค่า TOTP seed มาโต้งๆแล้ว เราก็ต้องไปหาเองจากการอ่านโค้ด <br>
ส่วนค่า Basic Authorization ตรงนี้เราไป decode Base64 จะออกมาได้ `admin:123456`

ทีนี้ก็มาดูโค้ด เราจะเห็นได้ว่าในไฟล์ "GameActivity" จะมี API 2 เส้นที่จะพาเราไปหา flag
```java
private void generateOtpSeed() {
	new OkHttpHelper().getJsonRequest("https://api.mobile.final.wtctt2023.p7z.pw/initial_otp");
}

private void verifyOtp() {
	new OkHttpHelper().getJsonRequest("https://api.mobile.final.wtctt2023.p7z.pw/verify_otp?otp=0000677131352");
}
```
ครั้งนี้จะมี method `generateOtpSeed()` เข้ามาด้วย แปลว่าเราต้องทำการ craft API เพื่อยิงเส้น `initial_otp` ไปเอาค่า TOTP Seed ก่อนแล้วค่อยมา generate OTP ไปยิงเส้น `verify_otp` <br>
ก็จัดสิคะรออะไร craft API ใน Postman ให้ยิงไปที่เส้น `initial_otp` อย่าลืมตั้ง Basic Auth header ด้วยนะ

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/c6c7025f-8776-4f74-8541-b283eae53fc9)

เราก็จะได้ค่า TOTP seed เป็น "LESSONLEARNT" ทีนี้ก็เอามาใส่ TOTP Generator (https://totp.danhersam.com/) และเนื่องจากในเส้น `verify_otp` จะเห็นได้ว่าเลข OTP มี 13 หลักก็เซ็ตค่า OTP ให้ออกมา 13 หลักด้วย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/2f2fe510-b056-4deb-a531-dafab7e9f97d)

พอได้เลข OTP เอาไปใส่ request เส้น `verify_otp` ยิงไปปุ๊บ ได้ flag กลับมาปั๊บ

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/2d16dc55-98ca-42e1-9464-e311689b71f9)

FLAG: WTCTT2023_FINAL_MOBILE02{86b99ce7952b823fa1dcfe45c72fb88f}
