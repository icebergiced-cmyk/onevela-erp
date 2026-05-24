# วันเวลา ERP — Demo P0

ระบบจัดซื้อ-คลัง-ต้นทุนต่อหลัง สำหรับโครงการ **วันเวลา (One Vela — Amata Chonburi)** บริษัท ทู บิลด์ ดีเวลลอปเมนท์ จำกัด

แทนระบบ Ecount เดิม ด้วยระบบที่ออกแบบมาเพื่อบ้านจัดสรร 483 แปลงโดยตรง

## 🚀 เปิด Demo

👉 **[เปิด Hub รวมทั้ง 9 หน้า](https://icebergiced-cmyk.github.io/wanwela-erp/00-hub-demo.html)**

## 📦 หน้าจอทั้งหมด (10 ไฟล์)

| # | หน้า | บทบาท | Device |
|---|---|---|---|
| 00 | [Hub รวมหน้า](00-hub-demo.html) | ทุกคน | 💻📱 |
| 01 | [PR Create — สร้างใบขอซื้อ](01-pr-create-demo.html) | โฟร์แมน | 📱 |
| 02 | [PR Inbox](02-pr-inbox-demo.html) | แอดมิน A | 💻 |
| 03 | [RFQ — ใบขอราคา](03-rfq-demo.html) | แอดมิน A | 💻 |
| 04 | [PO Create + เทียบราคา](04-po-create-demo.html) | แอดมิน A | 💻 |
| 05 | [MD อนุมัติ PO](05-po-approve-demo.html) | MD | 📱 |
| 06 | [Goods Receipt — รับของ](06-gr-demo.html) | แอดมิน A | 📱 |
| 07 | [Stock FIFO — คลัง](07-stock-demo.html) | ทุกคน | 💻 |
| 08 | [Issue — ขายลงแปลง](08-issue-demo.html) | แอดมิน B | 💻 |
| 09 | [Dashboard ต้นทุน](09-dashboard-demo.html) | MD | 💻 |

## 🧠 หลักการ

- **FIFO Cost Tracking** — ทุก lot รับเข้าคลัง คำนวณต้นทุนเข้าก่อนออกก่อน
- **ผูกแปลงตอน Issue** (ไม่ใช่ตอน PR) — traceability ที่ถูกต้องตามจริงของ procurement
- **ราคาขายลงแปลง = ราคาทุน FIFO เป๊ะ** — ไม่บวก margin
- **คลังกลาง "ร้านวันเวลาค้าวัสดุ"** — ไม่ใช่นิติบุคคลแยก
- **เลิกใช้ Ecount** — Export Excel ไประบบทำต้นทุนบ้านรายหลัง

## 👥 บทบาท

| คน | บทบาท |
|---|---|
| 👷 สมชาย เกตุแก้ว / ใหญ่ พุ่มวารี | โฟร์แมน (สร้าง PR) |
| 📊 เจนจิรา | แอดมิน A — จัดซื้อ + รับของ |
| 📤 คุณนิด | แอดมิน B — ขายลงแปลง |
| 🎯 คุณไอซ์ | MD — อนุมัติ + ดู Dashboard |

## ⚙️ Tech Stack

**Demo P0 (ปัจจุบัน):**
- Standalone HTML/CSS/JS — ไม่ต้อง build
- Sarabun font จาก Google Fonts
- html2pdf.js สำหรับ PDF generation
- SheetJS สำหรับ Excel export
- Plot data จาก Apps Script API จริง (One Vela Sales DB) — 483 แปลง

**Production P1+ (กำลังจะทำ):**
- Apps Script endpoints: `savePR`, `saveRFQ`, `savePO`, `saveGR`, `saveIssue`, `getStock`, `getDashboard`
- LINE Notify webhook integration
- Drive folder auto-organize: `One Vela Sales Files/เอกสารจัดซื้อ/{PR,RFQ,PO,GR,ISS}/`
- Export Excel ไประบบ "ทำต้นทุนบ้านรายหลัง" (เว็บเดิม)

## 🎬 Flow ทำงานทั้งระบบ

```
👷 PR (โฟร์แมน) → 📥 Inbox (Admin A) → 📨 RFQ → 📋 PO → ✅ Approve (MD)
                                                              ↓
📊 Dashboard ← 📤 Issue (Admin B) ← 🏪 Stock (FIFO) ← 📦 GR (รับของ)
```

## 📁 โครงสร้างไฟล์

```
wanwela-erp/
├── 00-hub-demo.html        # หน้ารวม + iframe preview
├── 01-pr-create-demo.html  # ฟอร์มสร้าง PR (โฟร์แมน)
├── 02-pr-inbox-demo.html   # Inbox จัดการ PR
├── 03-rfq-demo.html        # ขอราคา + กรอกราคา
├── 04-po-create-demo.html  # เทียบราคา + ออก PO
├── 05-po-approve-demo.html # MD อนุมัติ
├── 06-gr-demo.html         # รับของ + ถ่ายรูป + FIFO lot
├── 07-stock-demo.html      # คลัง FIFO view
├── 08-issue-demo.html      # ขายลงแปลง + FIFO calc + Excel export
├── 09-dashboard-demo.html  # KPI + ต้นทุนแปลง + charts
└── README.md
```

## 🛠️ Local Development

```bash
# Clone
git clone https://github.com/icebergiced-cmyk/wanwela-erp.git
cd wanwela-erp

# เปิดด้วย browser หรือ live server
open 00-hub-demo.html
# หรือ
python3 -m http.server 8000
# → http://localhost:8000/00-hub-demo.html
```

---

**สถานะ:** P0 Demo เสร็จสิ้น (25 พ.ค. 2569) · รอ wire backend P1+
**ผู้พัฒนา:** ระบบในเครือ Two Build Development
**License:** Proprietary — ใช้ภายในบริษัทเท่านั้น
