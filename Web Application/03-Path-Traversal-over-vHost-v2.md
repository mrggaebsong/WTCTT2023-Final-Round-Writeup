![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/ddab4d03-d590-479b-bdf6-ba2f0e8d7765)![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/fdac30ce-63c2-4e54-9133-c2a1304a885d)# Path Traversal over vHost v2 [200 pts]
> Find another vHost, do Server-Side Request Forgery (SSRF) attack to get the flag. <br>
> Flag Format: WTCTT2023_FINAL_WEB03{[a-z0-9]{32}} <br>
> URL: http://chall3.final.wtctt2023.p7z.pw <br>
> Hidden vHost URL Pattern: https://?????.chall3.final.wtctt2023.p7z.pw

เข้าหน้าเว็บมา เราจะเจอกับหน้าเว็บที่บอกชื่อไฟล์กับ file content ถ้าเราสังเกตที่ URL เราก็จะเจอเป็น URL parameter "page" ที่จะทำการเรียก local file ออกมาแสดงผล

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/cdb20e43-f6b8-4203-8b06-1d1099d92b04)

เห็นแบบนี้ก็เลยลองใช้ LFI (Local File Inclusion) ในการไปเรียกไฟล์อื่นๆในเครื่อง ในที่นี้เราจะลองทำการเรียก `/etc/passwd` ก่อน

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/af4354e4-78d0-4c97-b3cf-e110b23330b9)

เราจะพบว่าเราสามารถเรียกดูไฟล์ดังกล่าวได้ แต่ว่าโจทย์พูดถึง vHost เพราะฉะนั้นเราก็จะไปดูต่อกันที่ไฟล์ `/etc/hosts`

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/6f7f6089-8f52-41db-a3fc-42a7d700f8a6)

ก็จะเจอสิ่งที่น่าสนใจก็คือจะมีบรรทัดนึงเป็น virtual host ที่ domain "mon1t0r.chall3.final.wtctt2023.p7z.pw" <br>
มีเพิ่มเติมก็คือ ที่ response จะมี server banner เป็น Nginx ตรงนี้จะเป็นตัวช่วยให้เราสามารถ scope down ในการ enum config file ให้ล็อคเฉพาะของ nginx

```
HTTP/1.1 200 OK
Server: nginx
Date: Tue, 02 Jan 2024 07:06:06 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Referrer-Policy: no-referrer-when-downgrade
Content-Security-Policy: default-src * data: 'unsafe-eval' 'unsafe-inline'
Content-Length: 6362
```

ลองไปค้นอากู๋ดูก็จะเจอว่า default vhost config file location ของ nginx จะอยู่ที่ `/etc/nginx/sites-available/<domain-name>.conf` ว่าแล้วก็แทน `<domain-name>` ด้วย "mon1t0r" ของเรากันเลย

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/0f5d55b1-f5dd-489c-8afc-01f14922cc27)

ใน config file จะบอกเราว่า webroot ของ vhost mon1t0r จะอยู่ใน directory `/var/www/mon1t0r` <br>
Browse index.php จาก directory ดังกล่าว เราจะเจอ PHP code ชุดหนึ่งในหน้าเว็บ

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/e2ee2c93-8806-470a-96b7-b172a8ff5d50)

```PHP
<?php
// © All rights reserved for Siam Thanat Hack Company Limited

// Define the expected API key
$expectedApiKey = getenv('API_KEY');


// Check if the 'HTTP_X_API_KEY' is set in the $_SERVER superglobal
if (isset($_SERVER['HTTP_X_API_KEY']) ) {

    // Retrieve the API key from the header
    $apiKey = $_SERVER['HTTP_X_API_KEY'];

    // Compare it with the expected API key
    if ($apiKey === $expectedApiKey) {
        echo '<p>Good Job!! Flag 3 is: '.getenv('FLAG3').'</p>';
        // Proceed with your logic here
    } else {
        echo "<p>Invalid API key.</p>";
        // Handle the error (e.g., deny access)
    }
    exit;
} 


if(isset($_POST['submit'])) {

    $url = 'https://sth.sh/';
    if(isset($_POST['url'])){
        $url = $_POST['url'];
    }
    $url = $url.'?api_key='.$expectedApiKey;

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); 

    $response = curl_exec($ch);
    // Check if the request was successful
    if (!curl_errno($ch)) {
        $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

        if ($httpCode == 200) {
            echo 'success';
        } else {
            echo 'fail';
        }
    } else {
        // Handle cURL error
        echo 'cURL error: ' . curl_error($ch);
    }

    // Close the cURL session
    curl_close($ch);
   exit;
}

?>
```

ถึงตรงนี้จากโค้ดเราจะทราบ flow คร่าวๆก็คือ ถ้าเราส่ง request โดยแนบ header "X-API-KEY" ที่มีค่าตรงกับ API key ที่เซ็ตไว้ เราจะได้ flag กลับมา <br> 
ซึ่ง API key เราจะได้ต่อเมื่อส่ง POST request ไปหา URL ปลายทางที่รับค่าเข้ามา (ในที่นี้ initial ที่ STH web) ก็จะมีการหนีบ parameter `api_key` ต่อท้าย URL ด้วย

> Using `$_SERVER['HTTP_X_API_KEY']` is decalring the “X-API-KEY” in Swagger. <br>
> Ref: https://github.com/zircote/swagger-php/issues/549 <br>
> Thanks to แอด idealphase ที่ช่วยชี้ทางสว่างให้ ตอนวันแข่งคือตายที่ตรงนี้ อีกนิดเดียวก็ได้ละ

ถ้าเราเลื่อนหน้าโค้ดลงมาต่ออีกหน่อย เราจะเจอเป็น input form ที่รับค่า URL ซึ่งตรงกับ PHP code ที่เราเจอ

```HTML
<form method="post">
    <input type="text" name="url" placeholder="Enter URL">
    <input type="submit" name="submit" value="Submit">
</form>
```

มาถึงตรงนี้ ท่าที่เราจะใช้โจมตีจะเป็น "Server-Side Request Forgery (SSRF)" เพื่อทำการ exfiltrate ค่า api_key ผ่าน request ที่ทำการส่งออกไปหลังใส่ URL ใน input form ที่เราเจอไปหา server ของเราเองที่ตรงนี้เราจะใช้ interactsh-client ในการตั้ง DNS server ขึ้นมา <br>
พอตั้ง server เสร็จแล้ว ก็จะทำการ craft POST request ด้วยการตั้ง URL เป็น DNS server ของเราเอง อย่าลืมตั้ง Host header เป็น `mon1t0r.chall3.final.wtctt2023.p7z.pw` นะ <br>
พอเซ็ตอะไรต่างๆเสร็จแล้วก็ยิง request ได้เลย ถ้า response ตอบ success แปลว่าเราสามารถส่ง request หา server เราเองได้แล้ว

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/17d38f69-65ec-4481-8cc7-06c57e9f2520)
![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/2be71dfb-6453-4466-a69b-0d52d5b4b8e9)

มาดูที่ DNS server ของเราก็จะพบว่ามี request วิ่งมาหาเราแบบในรูปและมีหนีบ `api_key` parameter มาที่ URL ทีนี้เราก็ได้ API key มาใช้ยิง request ต่อแล้ว <br>
ส่ง request ไปที่ index.php ด้วย host mon1t0r และ "X-API-KEY: hk7yrnzb1igg32ccrvyob0iqpam3iyuf" แล้วก็ ตื้อดึง flag มาเราแบบ Good Job คมๆ

![image](https://github.com/mrggaebsong/WTCTT2023-Final-Round-Writeup/assets/22939654/a8bf6e0c-f972-459f-854a-788df20495b7)

FLAG: WTCTT2023_FINAL_WEB03{92959f78b3bc5baa6fb7781affcc8dfc}
