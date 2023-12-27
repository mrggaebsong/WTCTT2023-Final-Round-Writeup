# Account Takeover v2 [100 pts]
> Take over an admin account with an Insecure Direct Object Reference (IDOR) vulnerability. <br>
> Flag Format: WTCTT2023_FINAL_WEB01{[a-z0-9]{32}} <br>
> URL: https://chall1.final.wtctt2023.p7z.pw

เปิดเว็บขึ้นมา เจอหน้า Login

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/b0a6ed28-3d10-42c8-80f9-557c07cb7074)

Click "View Page Source" จะเห็น credential ใน comment เป็น `testuser01:testuser01` <br>
ว่าแล้วก็ Login ด้วย user testuser01 ได้เลย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/95f7094f-d6fc-454c-b9bf-830e227015b1)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/b5ed6edb-ee71-4c69-a411-33e7d899ca0e)

แต่ว่าตอนนี้เรายังไม่ได้เป็น admin เราก็เลยยังไม่ได้เห็น flag แต่ว่าๆๆๆ ถ้าดูที่ URL มันจะมี `user_id=1` อยู่ ก็เลยทำ IDOR เปลี่ยน `user_id` เป็นเลขอื่นไปดูข้อมูลของ user อื่นโดยการเอาเข้า Burp Intruder <br>
จากการทดลอง trial and error ระหว่างแข่ง ลอง set payload request ที่ 300

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/06cf96cb-9508-4f6e-862e-ff189f1cb715)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/8091140b-d9a1-40d8-a49b-ca355ceb7ac3)

> Tips: แนะนำให้ตั้ง Grep - Extract ให้ display username กับ message ("You do not have administrator privilege..." บลาๆๆๆ) เพราะสิ่งนี้จะช่วยให้เราหา flag ได้ง่ายและเร็ว

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/df44b865-a2af-4d50-bc24-dc96a495ce70)

รัน Intruder รอไปซักพักเราก็จะได้ flag ที่ request `user_id=256` แบบไม่ต้องเปลี่ยน session cookie ที่ request เหมือนรอบคัดเลือกแต่อย่างใด <br>
นี่แหละฮะ เหตุผลว่าทำไมเราถึงแนะนำให้ grep เอา message มันมาด้วย :)

Flag: WTCTT2023_FINAL_WEB01{6788151d0604ab9c1ac4c16c88f6e444}
