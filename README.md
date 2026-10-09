# SignalTW – Order Block & FVG [MTF] สำหรับ TradingView

อินดิเคเตอร์ Pine Script v6 แสดงสัญญาณ Smart Money Concepts บนกราฟ TradingView และใช้ได้กับทุก Timeframe

## ฟีเจอร์

| ฟีเจอร์ | รายละเอียด |
|---|---|
| **Order Block (OB)** | หาจากการเบรกโครงสร้าง (BOS/CHoCH): Bullish OB คือแท่งที่ low ต่ำสุดของขาก่อนเบรก Swing High, Bearish OB คือแท่งที่ high สูงสุดของขาก่อนเบรก Swing Low |
| **Fair Value Gap (FVG)** | ช่องว่างราคาจาก 3 แท่ง (Bullish: `low > high[2]`, Bearish: `high < low[2]`) กรองขนาดขั้นต่ำเป็น % ได้ |
| **Multi-Timeframe** | แสดงโซนของ TF กราฟปัจจุบัน และ HTF เพิ่มได้อีก 3 TF พร้อมกัน (ค่าเริ่มต้น 1H, 4H, D) ป้ายบอก TF เช่น `OB 4H`, `FVG 1H` และใช้เส้นขอบประ |
| **ไม่ Repaint** | ข้อมูล HTF ใช้เฉพาะแท่งที่ปิดแล้ว ส่วน TF ปัจจุบันจะวาดโซนเมื่อแท่งปิด |
| **Mitigation** | โซนที่ราคาเข้าไปใช้แล้วจะถูกซ่อน (หรือหยุดยืดและทำให้จางลง ปรับได้ในการตั้งค่า) |
| **BOS / CHoCH** | เส้นและป้ายโครงสร้างตลาดบน TF ปัจจุบัน |
| **Dashboard** | ตารางแสดงเทรนด์ของแต่ละ TF |
| **Alerts** | แจ้งเตือนเมื่อเกิด OB/FVG ใหม่, ราคาเข้าโซน, เกิด BOS/CHoCH |

## วิธีติดตั้ง

1. เปิด TradingView → แท็บ **Pine Editor** ด้านล่างกราฟ
2. คัดลอกโค้ดทั้งหมดจากไฟล์ [`SMC_OrderBlock_FVG_MTF.pine`](SMC_OrderBlock_FVG_MTF.pine) ไปวาง (แทนโค้ดเดิม)
3. กด **Save** แล้วกด **Add to chart**
4. ปรับค่าได้ที่ ⚙️ Settings ของอินดิเคเตอร์

## การดูหลาย TF

- **Chart TF**: โซนของ TF ที่เปิดอยู่ (เปลี่ยน TF กราฟเมื่อไรก็คำนวณใหม่ตาม TF นั้น)
- **HTF 1/2/3**: เลือก TF ที่สูงกว่าได้ตามต้องการ เช่น ดูกราฟ 5m แต่เห็น OB/FVG ของ 1H, 4H, D
- HTF จะแสดงเฉพาะเมื่อสูงกว่า TF ของกราฟ (เช่น เปิดกราฟ 4H ก็จะเห็นเฉพาะโซน 4H และ D)

## การตั้งแจ้งเตือน

- กด **Create Alert** → Condition เลือก `SignalTW OB/FVG`
  - เลือก **Any alert() function call** เพื่อรับข้อความละเอียด (ระบุ TF และช่วงราคาของโซน)
  - หรือเลือกเงื่อนไขเฉพาะ เช่น `New Bullish OB/FVG (any TF)`, `Price in Bullish zone`, `Bullish BOS/CHoCH`

## การตั้งค่าที่สำคัญ

| ค่า | ความหมาย |
|---|---|
| Swing length | ความยาว pivot สำหรับหา Swing High/Low (ค่ามาก = โครงสร้างใหญ่ขึ้น สัญญาณน้อยลง) |
| Max OB / FVG per TF / side | จำนวนโซนสูงสุดต่อ TF ต่อฝั่ง |
| OB mitigation | `Close` = ปิดแท่งทะลุโซน, `Wick` = ไส้เทียนทะลุโซน |
| Min gap size (%) | กรอง FVG ที่เล็กเกินไป |
| Hide mitigated zones | ซ่อนโซนที่ถูกใช้แล้ว |

> ⚠️ อินดิเคเตอร์นี้เป็นเครื่องมือช่วยวิเคราะห์ ไม่ใช่คำแนะนำการลงทุน
