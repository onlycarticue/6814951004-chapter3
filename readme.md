# รายงานแบบฝึกหัด Socket API & Docker (Assignment 1 & 2)

**ข้อมูลผู้จัดทำ**
* ชื่อ-นามสกุล: [ณรงค์ฤทธิ์ เมธาวรากุล]
* รหัสนักศึกษา: [6814951004]
* กลุ่ม/เซคชัน: [3]

---

## 📋 สิ่งที่ต้องติดตั้งก่อนใช้งาน (Prerequisites)

* **Docker Desktop** (ใช้รันคอนเทนเนอร์ และมี Docker Compose มาให้ในตัว)
* **Git** (สำหรับ clone โปรเจกต์)
* ตรวจสอบการติดตั้งด้วยคำสั่ง:
```bash
docker --version
docker-compose --version
```

## 📥 วิธีดาวน์โหลดโปรเจกต์

```bash
git clone https://github.com/onlycarticue/6814951004-chapter3
cd 6814951004
```

---

## 🛠️ แบบฝึกหัดที่ 1: ระบบ Chat ด้วย Docker (2 Clients & 1 Server)

### คำอธิบายการทำงาน
ระบบแชทนี้ใช้ Socket API แบบ TCP ในการส่งข้อมูลระหว่าง Client 2 ตัวผ่าน Server กลาง
โดยรันเป็น Docker Multi-container และจัดการด้วย Docker Compose

**หลักการทำงาน:**
1. `server.py` สร้าง socket รอรับการเชื่อมต่อที่พอร์ต [ใส่พอร์ตของคุณ เช่น 5000]
2. `client.py` (2 ตัว) เชื่อมต่อไปยัง Server ผ่านชื่อ service ใน Docker network
3. เมื่อ Client ตัวหนึ่งพิมพ์ข้อความ ข้อความจะถูกส่งไปที่ Server
4. Server ส่งต่อข้อความนั้นไปยัง Client อีกตัวเพื่อแสดงผล

**ไฟล์ในโฟลเดอร์:**
| ไฟล์ | หน้าที่ |
|---|---|
| server.py | โปรแกรม Server รับ-ส่งต่อข้อความ |
| client.py | โปรแกรม Client สำหรับพิมพ์และรับข้อความ |
| Dockerfile.server | สร้าง image สำหรับ Server |
| Dockerfile.client | สร้าง image สำหรับ Client |
| docker-compose.yml | กำหนดและรัน container ทั้งหมดพร้อมกัน |

### ขั้นตอนการเริ่มทำงาน (How to run)
1. เข้าโฟลเดอร์แบบฝึกหัดที่ 1:
```bash
cd chat-app  
```
2. สั่งรันคอนเทนเนอร์ทั้งหมด:
```bash
docker-compose up --build
```
3. เปิด Terminal ใหม่ 2 หน้าต่าง เพื่อเข้าใช้งาน Client:
```bash
docker exec -it chat_client1 python client.py
```
```bash
docker exec -it chat_client2 python client.py
```
4. พิมพ์ข้อความที่ Client 1 แล้วตรวจสอบว่า Client 2 ได้รับข้อความ (และกลับกัน)

### วิธีหยุดการทำงาน
```bash
docker-compose down
```

---

## 🌐 แบบฝึกหัดที่ 2: เว็บแอปพลิเคชัน Flask และ Socket Server

### คำอธิบายการทำงาน
แอปพลิเคชันหน้าเว็บพัฒนาด้วย Flask รับข้อความผ่านฟอร์ม (POST Method)
จากนั้น Flask ทำหน้าที่เป็น Client ส่งข้อความไปยัง Socket Server
เพื่อสะท้อนข้อมูลกลับ (Echo) และแสดงผลบนหน้าเว็บ

**หลักการทำงาน:**
1. ผู้ใช้กรอกข้อความในฟอร์มบนเว็บ แล้วกดส่ง
2. `app.py` (Flask) รับข้อความ แล้วเปิด socket เชื่อมต่อไปยัง `socket_server.py`
3. `socket_server.py` รับข้อความแล้วส่งกลับ (Echo)
4. Flask นำข้อความที่ได้กลับมาแสดงผลบนหน้าเว็บ

**ไฟล์ในโฟลเดอร์:**
| ไฟล์ | หน้าที่ |
|---|---|
| app.py | เว็บแอป Flask (ทำหน้าที่เป็น Socket Client) |
| socket_server.py | Socket Server สำหรับ Echo ข้อความ |
| Dockerfile.app | สร้าง image สำหรับเว็บแอป |
| Dockerfile.socket | สร้าง image สำหรับ Socket Server |
| docker-compose.yml | กำหนดและรัน container ทั้งหมดพร้อมกัน |

### ขั้นตอนการเริ่มทำงาน (How to run)
1. เข้าโฟลเดอร์แบบฝึกหัดที่ 2:
```bash
cd socket
```
2. สั่งรัน Docker Compose:
```bash
docker-compose up --build
```
3. เปิดเว็บเบราว์เซอร์ไปที่ http://localhost:5000
4. พิมพ์ข้อความในช่อง กดส่ง แล้วตรวจสอบว่าข้อความที่ Echo กลับมาแสดงบนหน้าเว็บ

### วิธีหยุดการทำงาน
```bash
docker-compose down
```
