**โจทย์หัวข้อ: "ระบบสั่งซื้อสินค้า"**
------
**คำอธิบาย**
ให้สร้าง API ของระบบสั่งซื้อสินค้า และทำโจทย์เสริมสำหรับปรับปรุง API ให้ทำงานได้มีประสิทธิภาพมากขึ้น โดยมีเวลาให้ทำ 3 ชั่วโมง

--------
**Technical Constraints**
- Programming Language: Go
- Architecture: Hexagonal Architecture/Clean Architecture
- Web Framework: Gin/Fiber
- Database: SQLite
----------
**รายละเอียดของ API**

- >เพิ่มสินค้า:
    >POST /products
    การทำงาน: สร้างสินค้าใหม่พร้อมกำหนดจำนวนสต็อกเริ่มต้น

- >ดูข้อมูลสินค้าทั้งหมด:
    >GET /products
    >การทำงาน: แสดงรายละเอียดสินค้าและจำนวนสต็อกที่เหลืออยู่ปัจจุบัน

- >ดูข้อมูลสินค้า
    >GET /products/:id
    >การทำงาน: แสดงรายละเอียดสินค้าและจำนวนสต็อกที่เหลืออยู่ปัจจุบัน

- >สร้างคำสั่งซื้อ:
    >POST /orders
    >การทำงาน: สร้างคำสั่งซื้อโดยการตัดสต็อกสินค้า

**Database structure**
```
    CREATE TABLE IF NOT EXISTS products (
    id INTEGER PRIMARY KEY AUTOINCREMENT, 
    name TEXT NOT NULL,
    stock INTEGER NOT NULL,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
    );

    CREATE TABLE IF NOT EXISTS orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT, 
    product_id INTEGER NOT NULL,            
    user_id TEXT NOT NULL,
    quantity INTEGER NOT NULL,
    status TEXT NOT NULL,
    idempotency_key TEXT UNIQUE,
    created_at DATETIME NOT NULL,
    FOREIGN KEY (product_id) REFERENCES products (id)
    );
```
**โจทย์เสริมมี 3 ข้อ (สามารถทำได้ทุกข้อ)**
- >โจทย์เสริมข้อ 1: ป้องกันการสั่งซื้อซ้ำซ้อน
คำสั่ง: 
เพิ่มการป้องกันการสั่งซื้อด้วยรายการเดิมซ้ำหลายรอบ
- >โจทย์เสริมข้อ 2: ป้องกันการตัดสต็อกชนกัน
คำสั่ง: 
เพิ่มการป้องกันการสั่งซื้อที่เข้ามาพร้อมกันแล้วทำให้เกิดการตัด stock ผิด
- >โจทย์เสริมข้อ 3: ระบบแจ้งเตือนผ่าน Channel (Channel-based Event System)
คำสั่ง:
หลังจากที่ Order ถูกสร้างและบันทึกลงฐานข้อมูลสำเร็จ ให้ระบบส่ง Email ยืนยันหาลูกค้า โดยที่ไม่ต้องรอการส่งเมล์เสร็จ (ให้ mock การส่ง email หาลูกค้า และให้หน่วงเวลาเพิ่มไว้ 1 วินาที)

----------
**เวลาในการทำแบบทดสอบ**
- เมื่อได้ git repository นี้แล้วถือว่าเวลาได้เริ่มแล้ว 
- มีเวลาในการทำ 3 ชั่วโมง ** ต้อง push code สุดท้ายไม่เกินเวลาที่กำหนด
------------
**ขั้นตอนของการทำแบบทดสอบ**
- เมื่อได้รับ git repository ให้สร้าง dev_{ชื่อตัวเอง} โดย copy มาจาก branch main
- ให้ทำ API ทีละเส้นให้เสร็จแล้วทำการ commit code และ push ขึ้น server
- หลังจากทำ API ของโจทย์ปกติเสร็จหมดแล้วให้ทำส่วนของโจทย์เสริมต่อได้ และยังต้องทำที่ละข้อแล้วทำการ commit และ push code ขึ้น server
