# ⚽ GlobalCup 2026 - Web3 ระบบเทรดตลาดพยากรณ์

> GlobalCup 2026 เป็น DApp ตลาดพยากรณ์แบบกระจายศูนย์ที่สร้างบน **BSC (Binance Smart Chain)** โครงการนี้ใช้ **Account Abstraction (ERC-4337) �เทคโนโลยีไร้ Gas**, ผสมผสาน **การเทรดหุ้น AMM (Shares AMM)** กับ **กลไกตัดสินใจ Multi-signature ของ DAO** เพื่อมอบประสบการณ์พยากรณ์และเทรดกีฬา Web3 ที่ราบรื่นและเป็นมืออาชีพ

---

## 🌟 คุณสมบัติ

### 1. การเทรดแบบไร้ Gas ด้วย ERC-4337 (Gasless Trading)
- ผสาน **AppKit**, **Viem** และ **Permissionless.js** พร้อม Pimlico Paymaster สนับสนุนค่า Gas
- สร้าง Safe smart account ให้ผู้ใช้โดยอัตโนมัติ ดำเนินการเทรดแบบ batch เช่น `Approve` + `Bet` ในคลิกเดียว ลดอุปสรรค Web3 อย่างมาก

### 2. การเทรดสองทิศทางแบบ AMM (Share-based AMM & Cash Out)
- ละทิ้งอัตราต่อรองคงที่แบบดั้งเดิม ใช้อัลกอริทึมกลไกราคาแบบไดนามิก
- รองรับ **ซื้อ (Buy)** และ **ขาย/ปิดสถานะ (Sell/Cash Out)** ผู้ใช้สามารถปิดสถานะก่อนกำหนดได้ตลอดเวลาตามอัตราต่อรองแบบเรียลไทม์เพื่อล็อกกำไรหรือตัดขาดทุน

### 3. การแสดงราคาอัตราต่อรองแบบเรียลไทม์ (Real-time Odds)
- ผสาน MongoDB `O(1)` atomic updates กับ WebSocket (`Socket.io`) ประเภท broadcast ระดับมิลลิวินาที
- การเดิมพันของผู้ใช้ใดๆ จะทำให้อัตราต่อรองและกราฟทั่วทั้งเครือข่าย refresh ทันที

### 4. พอร์ตโฟลิโอและรายการโปรด (Portfolio & Favorites)
- แผงควบคุม "พอร์ตโฟลิโอ" ที่ใช้งานง่าย แสดง **มูลค่าปัจจุบัน** และ **กำไร/ขาดทุนที่ยังไม่รับรู้ (Unrealized PnL)** ของหุ้นที่ถืออยู่แบบเรียลไทม์
- รองรับการเพิ่มการแข่งขันลงรายการโปรดด้วยคลิกเดียวและการเชื่อมต่อสถานะข้ามแผงควบคุม

### 5. กลไกตัดสินใจแบบ Multi-signature ของ DAO (DAO Multi-sig Resolution)
- คอนโซลตัดสินใจ `admin.html` แยกต่างหาก อิงจากการยืนยันลายเซ็น Nonce ของ Web3 wallet
- หลังจากการแข่งขันสิ้นสุด ต้องมีสมาชิก DAO สองคนลงคะแนนตรงกัน และ bot แบ็กเอนด์จะอัปโหลดผลลัพธ์ไปยัง chain โดยอัตโนมัติ ทริกเกอร์การชำระกองทุน ป้องกันพฤติกรรมไม่ซื่อสัตย์จุดเดียว

### 6. หลายภาษาอัจฉริยะ (i18n Auto-detection)
- ระบุตำแหน่งผู้ใช้อัตโนมัติผ่าน IP Geolocation และ Browser Headers
- รองรับ English, 中文 (zh), ລາ​ວ (lo), และ **ภาษาไทย (th)** โดยกำเนิด พร้อมสลับภาษาทั้ง UI และข้อมูลพื้นฐานได้อย่างราบรื่น

---

## 🔌 ภาพรวม API

แบ็กเอนด์สร้างบน **Node.js (Express) + MongoDB**

### 1. Public API
| Endpoint | Method | คำอธิบาย |
| :--- | :--- | :--- |
| `/api/events` | `GET` | ดึงข้อมูลพื้นฐานการแข่งขันทั้งหมดที่เก็บในฐานข้อมูล |
| `/api/win-rates` | `GET` | ดึงยอดกองทุนและอัตราการชนะแบบไดนามิก (odds) ของการแข่งขันทั้งหมด |
| `/api/trades/:id` | `GET` | ดึงบันทึกการเทรดทั้งหมดสำหรับการแข่งขันที่ระบุ (eventId) |
| `/api/locale` | `GET` | ตรวจจับภาษาสำรอง - ส่งคืนภาษาแนะนำโดยแปลงส่วนหัว `Accept-Language` |

### 2. Trading & User API
| Endpoint | Method | คำอธิบาย |
| :--- | :--- | :--- |
| `/api/save-order` | `POST` | รับข้อมูลเดิมพัน/ปิดสถานะจาก frontend อัปเดตกองทุนแบบ atomic (`$inc`) ทริกเกอร์ WS broadcast |
| `/api/user-portfolio` | `GET` | ส่ง `address` เพื่อดึงประวัติการเดิมพัน มูลค่าตำแหน่ง และ PnL ทั้งหมดของ smart account นั้น |
| `/api/user-payouts` | `GET` | ส่ง `address` เพื่อดึงรายการคำสั่งที่ชนะและสามารถถอนได้ |
| `/api/pimlico/56` | `POST` | ส่งต่อคำขอ Bundler/Paymaster เพื่อซ่อน API Key |

### 3. DAO Admin API
> **หมายเหตุ:** endpoints ทั้งหมดด้านล่างต้องผ่าน middleware `verifyDaoAuth` ตรวจสอบ `x-wallet-address`, `x-signature`, `x-nonce`, `x-timestamp` ในส่วนหัว

| Endpoint | Method | คำอธิบาย |
| :--- | :--- | :--- |
| `/api/admin/auth-nonce` | `GET` | endpoint pre-login สร้างข้อความลายเซ็นครั้งเดียวป้องกัน replay (Nonce) |
| `/api/admin/resolve-event`| `POST` | ส่งคะแนนการตัดสิน เมื่อสองคะแนนตรงกัน แบ็กเอนด์จะเรียก smart contract `resolveEvent` ชำระบน chain โดยอัตโนมัติ |
| `/api/admin/dao-members` | `GET` | ดึงรายชื่อสมาชิก DAO ทั้งหมดใน whitelist ปัจจุบัน |
| `/api/admin/dao-members` | `POST` | เพิ่มที่อยู่ BSC wallet ของสมาชิก DAO ใหม่ |
| `/api/admin/dao-members/:address`| `DELETE`| ลบสิทธิ์สมาชิก DAO ที่ระบุ (super admin และตัวเองไม่สามารถลบได้) |

---

## 🔄 แผนภาพ Workflow

### 1. การเทรดหลักและ Flow อัปเดตอัตราต่อรองแบบเรียลไทม์ (User Trading Flow)

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Bundler as Bundler(Pimlico)
    participant BSC_Contract
    participant Backend as Backend(Node+Mongo)
    participant All_Clients
    
    User->>Frontend: เลือกซื้อ/ขาย คลิกยืนยัน
    Frontend->>Bundler: สร้าง UserOperation (bundle Approve + Bet)
    Bundler->>BSC_Contract: สนับสนุน gas และดำเนินการ transaction บน chain
    BSC_Contract-->>Frontend: ส่งคืน txHash
    Frontend->>Backend: POST /api/save-order รายงานคำสั่ง
    Backend->>Backend: MongoDB $inc O(1) อัปเดตกองทุนแบบ atomic
    Backend->>All_Clients: WebSocket broadcast อัตราต่อรองล่าสุด (odds_updated)
    All_Clients->>All_Clients: UI hot-reload: กราฟกระเด้ง อัปเดตอัตราต่อรองบนแผง
```

### 2. DAO Multi-signature Resolution & Auto Settlement Flow (DAO Resolution Flow)

```mermaid
sequenceDiagram
    participant DAO_Admin
    participant Admin_UI
    participant Backend(Nodejs)
    participant BSC_Contract
    participant Users
    
    DAO_Admin->>Admin_UI: 3 ชั่วโมงหลังการแข่ง ล็อกอิน admin.html
    Admin_UI->>Backend(Nodejs): ขอ Auth-Nonce และลงชื่อเข้าใช้
    DAO_Admin->>Admin_UI: ส่งผลการแข่ง (เช่น ทีมเหย้าชนะ)
    Admin_UI->>Backend(Nodejs): POST /api/admin/resolve-event
    Backend(Nodejs)->>Backend(Nodejs): บันทึกคะแนน ตรวจสอบว่าถึง 2 คะแนนที่ตรงกันหรือไม่
    alt ถึงความยินยอม (2 คะแนนตรงกัน)
        Backend(Nodejs)->>BSC_Contract: Node.js machine wallet เริ่ม transaction resolveEvent()
        BSC_Contract-->>Backend(Nodejs): กองทุนบน chain เข้าสู่สถานะ Settlement
        Backend(Nodejs)->>Users: อัปเดตสถานะ พอร์ตโฟลิโอแสดงปุ่ม "ถอน"
        Users->>BSC_Contract: คลิก "ถอน" contract โอน USDC ให้ตามสัดส่วน
    else ยังไม่ถึงความยินยอม
        Backend(Nodejs)-->>Admin_UI: บันทึกสำเร็จ รอการยืนยันจาก admin อีกคน
    end
```

---

## 📖 คู่มือการใช้งาน

### 👤 คู่มือผู้ใช้ทั่วไป
1. **เชื่อมต่อ Wallet & ยืนยันตัวตน**: เปิดหน้าหลัก คลิก **[เชื่อมต่อ Web3Wallet]** มุมขวาบน หลังยืนยัน ระบบจะสร้าง smart account สำหรับเทรดแบบไร้ gas บน BSC ให้โดยอัตโนมัติ
2. **โอนเงิน (ฝาก)**: ก่อนเดิมพันครั้งแรก คลิก **[ฝากเงิน]** ที่แผง wallet มุมขวาบน ใส่จำนวนและยืนยันเพื่อโอนเงินจาก EOA wallet ส่วนตัวไปยัง smart account ของแพลตฟอร์ม
3. **การเทรดสองทิศทาง**:
   - **ซื้อ (Buy)**: เลือกทีมที่ชอบในแผงด้านขวา ใส่จำนวน USDC คลิกซื้อเพื่อรับ "หุ้น" จำนวน相应
   - **ปิดสถานะก่อนกำหนด (Cash Out)**: หากอัตราต่อรองเป็นใจ คลิก **[พอร์ตโฟลิโอ]** มุมขวาบน คลิก `[Cash Out]` ใน "ประวัติการเดิมพัน" ระบบจะซื้อคืนหุ้นของคุณในราคาตลาดแบบเรียลไทม์และส่งคืน USDC ทันที
4. **ถอนหลังการแข่ง**: หลังการแข่งสิ้นสุดและ DAO ตัดสิน หากคุณถือหุ้นฝ่ายชนะ ไปที่ **[พอร์ตโฟลิโอ]** และคลิก **[ถอน]** เงินต้นและกำไรจะโอนไปยัง smart account ของคุณโดยอัตโนมัติพร้อมเอฟเฟกต์เฉลิมฉลอง

### 🛡️ คู่มือ DAO Admin
1. **เข้าสู่ระบบ Admin**:
   - เยี่ยมชม `http://[โดเมนหรือIPของคุณ]:3010/admin.html`
   - คลิก **[เชื่อมต่อ Wallet & ยืนยันตัวตน]** และลงนามข้อความครั้งเดียว (Nonce) จากระบบใน MetaMask (ไม่มีค่า gas)
2. **การตัดสินใจ Multi-signature**:
   - หลังเข้าสู่ระบบ ในแผง **[การแข่งขันที่รอการตัดสิน]** คุณจะเห็นการแข่งขันทั้งหมดที่ **เวลาแข่งผ่านไป 3 ชั่วโมง** และยังไม่ได้ชำระ
   - ตามผลการแข่งขันจริง คลิก "ทีมเหย้าชนะ", "ทีมเยือนชนะ", หรือ "เสมอ"
   - เมื่อสมาชิก DAO สองคนลงคะแนนผลเดียวกันสำหรับการแข่งขันเดียวกัน ระบบจะทริกเกอร์ smart contract เพื่อชำระกองทุนการแข่งขันบน chain โดยอัตโนมัติ
3. **การจัดการสมาชิก DAO**:
   - สลับไปที่แท็บ **[การจัดการสมาชิก DAO]**
   - คุณสามารถใส่ที่อยู่ BSC wallet ของพันธมิตรที่เชื่อถือได้คนอื่นเพื่อเพิ่มเป็น DAO admin ที่มีสิทธิ์ลงคะแนน หรือลบได้ตลอดเวลา (super admin ไม่สามารถลบได้)
