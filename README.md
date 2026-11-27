# Trade Stack

เว็บแอปบันทึกการเทรด XAUUSD / GBPUSD (MT5 · XM) โทนมืด ใช้ Google Sheet เป็นฐานข้อมูล
มีเช็กลิสต์ A+ ที่ตัดเกรดให้อัตโนมัติ, Risk Engine, คำนวณ Lot, แนบรูปกราฟ, สถิติ

## ไฟล์
- `index.html`: ตัวแอปทั้งหมด (ไม่ต้อง build)
- `manifest.json`, `sw.js`, `icons/`: ทำให้ติดตั้งเป็นแอปบนมือถือได้ (PWA) และเปิดได้แม้เน็ตไม่ดี
- `apps-script/Code.gs`: หลังบ้านที่เชื่อมกับ Google Sheet (ชีต Trades + Cashflow) และ Google Drive (เก็บรูป)

## ติดตั้ง Google Sheet (ครั้งเดียว)
1. สร้าง Google Sheet ใหม่ ตั้งชื่อ "Trade Stack"
2. ไปที่ Extensions › Apps Script แล้ววางโค้ดจาก `apps-script/Code.gs` ทั้งไฟล์ จากนั้นกด Save
3. เลือกฟังก์ชัน `setup` แล้วกด Run พร้อมกดอนุญาตสิทธิ์ เปิด Execution log แล้วคัดลอก **API_TOKEN** ไว้
4. กด Deploy › New deployment › เลือกประเภท **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - จากนั้นกด Deploy แล้วคัดลอก URL ที่ลงท้ายด้วย `/exec`
5. ถ้าแก้ Code.gs ภายหลัง ให้ไปที่ Deploy › Manage deployments › Edit แล้วเลือก Version ใหม่ (URL จะไม่เปลี่ยน)

## ขึ้นเว็บด้วย GitHub Pages
1. สร้าง repo ใหม่ เช่น `trade-stack` (ต้องเป็น Public ถ้าใช้ GitHub แบบฟรี)
2. อัปโหลดทุกไฟล์ (`index.html`, `manifest.json`, `sw.js` และโฟลเดอร์ `icons`) ไว้ที่ root ของ repo
3. ไปที่ Settings › Pages › Source เลือก "Deploy from a branch" › เลือก `main` / root แล้วกด Save
4. เปิด `https://<username>.github.io/trade-stack/` บนมือถือ
   - iPhone (Safari): กดปุ่มแชร์ › "เพิ่มไปยังหน้าจอโฮม"
   - Android (Chrome): กดเมนู ⋮ › "ติดตั้งแอป" 
5. ในแอป ไปที่ ตั้งค่า แล้วใส่ URL `/exec` และ API Token → กด "ทดสอบการเชื่อมต่อ" → กด "ส่งขึ้น Sheet"
6. เครื่องอื่น/มือถือใหม่: ใส่ URL + Token เดิม แล้วแอปจะดึงข้อมูลจาก Sheet ลงมาให้เองทุกครั้งที่เปิด

> ถ้าเคยติดตั้ง Code.gs เวอร์ชันก่อนแล้ว ให้วางโค้ดใหม่ทับ รัน `setup` อีกครั้ง (จะเพิ่มคอลัมน์และชีต Cashflow) แล้ว Deploy เป็น Version ใหม่

> Token ไม่ได้อยู่ในโค้ดบน GitHub คุณกรอกเองในแอป และแอปเก็บไว้ในเครื่องนั้นเท่านั้น
> รูปกราฟถูกเก็บในโฟลเดอร์ Drive `TradeStack_Screenshots` แบบส่วนตัว ในชีตจะมีลิงก์ไปยังรูป

## ก่อนใช้จริง
- ตรวจ **Contract size** ใน MT5 (คลิกขวาที่ Symbol › Specification) แล้วใส่ในหน้า ตั้งค่า ให้ตรงกับประเภทบัญชี XM ที่ใช้
- ใส่ทุนตั้งต้นจริง แล้วกด "ล้างข้อมูลทั้งหมด" เพื่อลบข้อมูลตัวอย่าง
- ฝากหรือถอนเงินทุกครั้ง ให้บันทึกที่ ตั้งค่า › ฝาก / ถอนเงิน ไม่อย่างนั้นยอดทุน, % risk และ DD จะเพี้ยน
- Entry model A ที่ตั้ง limit ที่ B: เลือก "Limit ที่ B" แล้วกลับมากด "ได้ fill แล้ว" หรือ "ยกเลิก" ในหน้าแรก
