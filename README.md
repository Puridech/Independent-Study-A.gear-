# n8n AI Automation Workflows

[cite_start]โปรเจกต์นี้เป็นการพัฒนา AI Automation Workflow ด้วย n8n [cite: 4] [cite_start]ประกอบด้วย 2 workflows หลักที่ทำงานได้จริง [cite: 6] [cite_start]ได้แก่ แชทบอท AI ทั่วไปบน Telegram [cite: 7, 8] [cite_start]และระบบถาม-ตอบจากเอกสาร (Document Q&A Bot) [cite: 9, 10]

---

## 1. Workflow: Telegram Bot AI Gemini
ไฟล์: `Telegram Bot Ai Genmini.json`

[cite_start]แชทบอท AI บน Telegram ที่ทำหน้าที่ตอบคำถามทั่วไป [cite: 7, 8] สามารถพูดคุยและตอบโต้ได้อย่างเป็นธรรมชาติ

**ส่วนประกอบหลัก (Nodes):**
* **Telegram Trigger:** ทำหน้าที่ดักจับและรับข้อความเริ่มต้นที่ผู้ใช้พิมพ์ส่งเข้ามา
* **Set (Edit Fields):** คัดกรองและดึงเฉพาะข้อมูลที่จำเป็น (เช่น `ChatID` และเนื้อหาข้อความ) เพื่อส่งต่อให้ระบบทำงานง่ายขึ้น
* **AI Agent & Google Gemini Chat Model:** "สมองกล" หลักของบอท ทำหน้าที่ประมวลผลข้อความและคิดคำตอบ โดยใช้โมเดลของ Google Gemini
* **Simple Memory:** ระบบความจำระยะสั้น ช่วยให้บอทจดจำบริบทและประวัติการสนทนาก่อนหน้าได้
* **Telegram (Send Message):** ส่งข้อความผลลัพธ์จาก AI กลับไปหาผู้ใช้งานทางแชท

---

## 2. Workflow: Document Q&A Bot
ไฟล์: `Document Q&A Bot.json`

[cite_start]ระบบแชทบอทถาม-ตอบจากเอกสาร โดยผู้ใช้สามารถอัปโหลดไฟล์ แล้วให้ AI วิเคราะห์และตอบคำถามจากเนื้อหาในเอกสารนั้นๆ (ระบบ RAG) [cite: 9, 10]

**ส่วนประกอบหลัก (Nodes):**

**▶ ส่วนที่ 1: การจัดการเอกสาร (File Upload Flow)**
* **On form submission (Form Trigger):** หน้าเว็บฟอร์ม (Webhook) สำหรับรับไฟล์เอกสารที่ผู้ใช้อัปโหลด
* **HTTP Requests (Create Store / Upload / Import):** ชุดคำสั่ง API ที่ทำหน้าที่สร้างคลังเก็บข้อมูล (File Search Store), อัปโหลดไฟล์ขึ้น Google Gemini, และดึงไฟล์เข้าสู่คลังเอกสารเพื่อเตรียมให้ AI เข้าถึง

**▶ ส่วนที่ 2: ระบบถาม-ตอบ (Q&A Chat Flow)**
* **Telegram Trigger:** รับข้อความคำถามจากผู้ใช้งาน
* **AI Agent (RAG Agent):** ตัวประมวลผลที่ถูกตั้งคำสั่ง (System Prompt) ให้ทำหน้าที่ค้นหาข้อมูลจากเอกสาร และต้องระบุแหล่งที่มา/เลขหน้าทุกครั้งที่ตอบ
* **DataFile (HTTP Request Tool):** เครื่องมือพิเศษที่เชื่อมต่อกับโมเดล `gemini-2.5-flash` เพื่อทำการค้นหาข้อมูล (Search) เจาะจงเฉพาะในคลังเอกสารที่เราอัปโหลดไว้
* **Simple Memory & Gemini Chat Model:** ทำหน้าที่ประมวลผลภาษาและจดจำบริบทการสนทนา
* **Telegram (Send Message):** สรุปข้อมูลที่ได้จากเอกสาร และส่งเป็นข้อความตอบกลับหาผู้ใช้
