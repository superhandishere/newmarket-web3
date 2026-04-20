## 🏇 Pari-mutuel（互相下注/彩池制）

传统赛马或早期博彩常用这种模式：赢家按下注比例瓜分输家的钱。

> ⚠️ **致命缺点**：Web3 环境下用户**无法锁定赔率**。昨天买的赔率很好，今天比赛前大户突然重注，你的利润瞬间被稀释。交易员讨厌这种"盲盒"体验。

---

### 🤖 我们的方案：AMM + CTF 恒定乘积模型

**核心法则：1 Share = 1 USDC**

当比赛分出胜负时：
- 🟢 赢方 → Share 价值 = **$1**
- 🔴 输方 → Share 价值 = **$0**

---

### 🔒 实时锁价，拒绝稀释

假设主队当前价格 = `$0.50`

用户投入 `10 USDC` → 系统按当前价格发放 `20 Shares`

这 `20 Shares` **彻底锁定**：
- 不管后面多少人把主队价格买到 `$0.99`
- 只要主队最终赢了 → 拿着 20 Shares 换 `20 USDC`

---

### 🛡️ 资金池绝对安全，不会穿仓

复杂数学公式（牛顿迭代法）保证 AMM 永远有足够资金赔付：

> 用户买入"胜"份额时 → AMM 用这笔钱铸造完整的 `[胜, 负, 平]` 套装 → "胜"给用户，"负"+"平"留在底池作为对冲储备

---

## ⚡ 传统 Web3 vs 混合架构

| | 传统纯 Web3 | GlobalCup（混合架构）|
|---|---|---|
| **订单/算价/结算** | 写在以太坊智能合约 | 放在**后端** |
| **用户提现** | 自己发起链上交易 → 调用 `claim` | **一键请求后端** |
| **等待时间** | 等以太坊出块 | **1秒内到账** |
| **Gas 费用** | 用户支付 `claim` 交易费 | **零 Gas** |

> 📌 `PredictPro.sol` 智能合约只充当**资金托管金库**，核心算价和订单记录都在后端。

---

### ✨ 混合架构优势

1. **极速体验**：点击"提现" → 无钱包签名 → 无需等出块 → **1秒内撒花特效**
2. **极度省钱**：不再为毫无意义的 `claim` 交易支付 Pimlico 赞助的 Gas
3. **绝对安全**：后端**先锁死数据库** → 再用金库私钥链上发钱 → 从根本上杜绝前端并发盗刷

---

> 💡 这就是 Web3 行业常说的 **"Web2.5 终极形态"** — 拥有 Web3 的资产透明度 + Web2 的极致丝滑体验

---

## English Version

### 🏇 Pari-mutuel (Mutual Betting / Pool System)

Commonly used in traditional horse racing and early gambling: winners split the losers' money proportionally.

> ⚠️ **Fatal flaw in Web3**: Users **cannot lock in odds**. A large investor placing a huge bet right before the event starts can instantly dilute your profits. Traders hate this "blind box" experience.

---

### 🤖 Our Solution: AMM + CTF Constant Product Model

**Core Rule: 1 Share = 1 USDC**

When the event concludes:
- 🟢 Win → Share value = **$1**
- 🔴 Lose → Share value = **$0**

---

### 🔒 Real-time Price Locking, No Dilution

Assume current home team price = `$0.50`

User deposits `10 USDC` → System issues `20 Shares` at current price

These `20 Shares` are **completely locked**:
- No matter how many people push the home team price to `$0.99`
- As long as the home team wins → redeem `20 Shares` for `20 USDC`

---

### 🛡️ Funding Pool Fully Secure, No Insolvency

Sophisticated mathematics (Newton-Raphson method) ensures AMM always has sufficient funds:

> When a user buys "win" Shares → AMM uses the payment to mint a full `[Win, Lose, Draw]` set → "Win" goes to the user, "Lose" + "Draw" stay in the reserve pool as hedging

---

## ⚡ Traditional Web3 vs Hybrid Architecture

| | Traditional Pure Web3 | GlobalCup (Hybrid) |
|---|---|---|
| **Orders/Pricing/Settlement** | On Ethereum smart contracts | On **backend** |
| **User Withdrawal** | Initiates on-chain transaction → calls `claim` | **One-click backend request** |
| **Wait Time** | Waits for Ethereum blocks | **Instant (1 sec)** |
| **Gas Fees** | User pays `claim` transaction fee | **Zero Gas** |

> 📌 `PredictPro.sol` smart contract only acts as a **fund custody vault**; core pricing and order records are on the backend.

---

### ✨ Hybrid Architecture Advantages

1. **Ultra-smooth Experience**: Click "withdraw" → no wallet signature → no block waiting → **confetti animation in 1 second**
2. **Extremely Cost-efficient**: No more paying Pimlico-sponsored Gas for meaningless `claim` transactions
3. **Absolute Security**: Backend **locks database first** → then uses vault private key to send funds on-chain → fundamentally eliminates front-end concurrent theft

---

> 💡 This is what the Web3 industry calls **"Web2.5 Ultimate Form"** — Web3 asset transparency + Web2's seamless experience

---

## เวอร์ชันภาษาไทย

### 🏇 Pari-mutuel (การเดิมพันร่วมกัน / ระบบโป๊กเกอร์สระ)

รูปแบบที่ใช้กันทั่วไปในการแข่งม้าแบบดั้งเดิมและการพนันยุคแรก: ผู้ชนะแบ่งเงินผู้แพ้ตามสัดส่วนการเดิมพัน

> ⚠️ **ข้อบกพร่องร้ายแรงใน Web3**: ผู้ใช้**ไม่สามารถล็อคอัตราต่อรองได้** นักลงทุนรายใหญ่วางเดิมพันมหาศาลก่อนการแข่งขันเริ่มต้นจะทำให้กำไรของคุณถูกลดทอนทันที เทรดเดอร์เกลียดประสบการณ์ "กล่องปริศนา" แบบนี้

---

### 🤖 วิธีแก้ของเรา: AMM + CTF รุ่นผลคูณคงที่

**กฎหลัก: 1 Share = 1 USDC**

เมื่อการแข่งขันจบลง:
- 🟢 ชนะ → มูลค่า Share = **$1**
- 🔴 แพ้ → มูลค่า Share = **$0**

---

### 🔒 ล็อคราคาแบบเรียลไทม์ ปลอดภัยจากการลดทอน

สมมติราคาปัจจุบันของทีมเหย้า = `$0.50`

ผู้ใช้ฝาก `10 USDC` → ระบบออก `20 Shares` ตามราคาปัจจุบัน

`20 Shares` นี้**ล็อคอย่างสมบูรณ์**:
- ไม่ว่าหลายหมื่นคนจะซื้อจนราคาขึ้นถึง `$0.99`
- ถ้าทีมเหย้าชนะ → นำ 20 Shares ไปแลก `20 USDC` ได้แน่นอน

---

### 🛡️ คลังเงินปลอดภัยอย่างแน่นอน ไม่มีการขาดทุนขาดชำระ

สูตรคณิตศาสตร์ที่ซับซ้อน (วิธีซ้ำนิวตัน) รับประกันว่า AMM มีเงินเพียงพอสำหรับการจ่าย:

> เมื่อผู้ใช้ซื้อสิทธิ์ "ชนะ" → AMM ใช้เงินนั้นสร้างชุดสิทธิ์ `[ชนะ, แพ้, เสมอ]` แบบครบถ้วน → "ชนะ" ให้ผู้ใช้, "แพ้"+"เสมอ" เก็บในคลังสำรองเป็นสินทรัพย์ป้องกันความเสี่ยง

---

## ⚡ Web3 แบบดั้งเดิม vs สถาปัตยกรรมผสม

| | Web3 แบบบริสุทธิ์ | GlobalCup (สถาปัตยกรรมผสม) |
|---|---|---|
| **คำสั่ง/ราคา/การชำระ** | อยู่บน Smart Contract Ethereum | อยู่บน **Backend** |
| **ผู้ใช้ถอนเงิน** | ส่งธุรกรรมบนเชนเอง → เรียก `claim` | **คลิกเดียวไป Backend** |
| **เวลารอ** | รอ Ethereum สร้างบล็อก | **ทันที (1 วินาที)** |
| **ค่า Gas** | ผู้ใช้จ่ายค่าธุรกรรม `claim` | **Gas ศูนย์** |

> 📌 Smart Contract `PredictPro.sol` ทำหน้าที่เพียง**คลังเก็บเงิน**; การคำนวณราคาและบันทึกคำสั่งหลักอยู่บน Backend

---

### ✨ ข้อดีของสถาปัตยกรรมผสม

1. **ประสบการณ์ลื่นไหล**: คลิก "ถอนเงิน" → ไม่ต้องเซ็นกระเป๋า → ไม่ต้องรอบล็อก → **เอฟเฟกต์ดอกไม้ไฟภายใน 1 วินาที**
2. **ประหยัดเงินอย่างยิ่ง**: ไม่ต้องจ่าย Gas ที่ Pimlico สนับสนุนสำหรับธุรกรรม `claim` ที่ไร้ความหมาย
3. **ความปลอดภัยยิ่ง**: Backend **ล็อคฐานข้อมูลก่อน** → ใช้กุญแจส่วนตัวของคลังส่งเงินบนเชน → ป้องกันการขโมยจากส่วนหน้าได้อย่างรากฐาน

---

> 💡 นี่คือสิ่งที่อุตสาหกรรม Web3 เรียกว่า **"รูปแบบสุดท้ายของ Web2.5"** — ความโปร่งใสของสินทรัพย์ Web3 + ประสบการณ์ลื่นไส้ของ Web2