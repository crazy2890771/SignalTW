# SignalTW – Order Block & FVG [MTF] สำหรับ TradingView

อินดิเคเตอร์ Pine Script v6 แสดง **Order Block, Fair Value Gap, CHoCH / BOS, Liquidity และ RSI Divergence** บนกราฟ TradingView ใช้ได้ทุก Timeframe และดูโซนของ TF ที่สูงกว่าซ้อนบนกราฟได้

## ฟีเจอร์

| ฟีเจอร์ | รายละเอียด |
|---|---|
| **Order Block (OB)** | หาจากการเบรกโครงสร้าง: Bullish OB คือแท่งที่ low ต่ำสุดของขาก่อนเบรก Swing High, Bearish OB คือแท่งที่ high สูงสุดของขาก่อนเบรก Swing Low |
| **Fair Value Gap (FVG)** | ช่องว่างราคาจาก 3 แท่ง (Bullish: `low > high[2]`, Bearish: `high < low[2]`) |
| **CHoCH / BOS** | เส้นและป้ายโครงสร้างตลาด (ค่าเริ่มต้นแสดงเฉพาะ CHoCH) กราฟ 5m หรือเล็กกว่าใช้โครงสร้างของ 15m ป้ายเขียนว่า `CHoCH 15m` ส่วนกราฟ TF อื่นใช้ TF ของกราฟ ป้ายเขียนว่า `CHoCH` |
| **Liquidity** | PDH/PDL, PWH/PWL, Asia High/Low, Equal Highs/Lows (EQH/EQL) และเครื่องหมาย `$` เมื่อถูกกวาด |
| **RSI Divergence** | `Bull Div` = ราคาทำ Low ต่ำลงแต่ RSI ทำ Low สูงขึ้น, `Bear Div` = ราคาทำ High สูงขึ้นแต่ RSI ทำ High ต่ำลง เทียบกับ Low ต่ำสุด / High สูงสุดภายใน 60 แท่งก่อนหน้า (ไม่ใช่แค่ Swing ล่าสุด) มีเส้นเชื่อม 2 จุดบนกราฟราคา (RSI 14 เหมือน RSI ปกติ) |
| **Multi-Timeframe** | โซนของ TF กราฟ + HTF ได้สูงสุด 5 TF (ค่าเริ่มต้น 5m · 15m · 1H · 4H · 1D) ป้ายบอก TF เช่น `OB 4H`, `FVG 1H` (เส้นขอบประ) |
| **ไม่ Repaint** | ข้อมูล HTF ใช้เฉพาะแท่งที่ปิดแล้ว ส่วน TF กราฟวาดเมื่อแท่งปิด |
| **ลดความรก** | แสดงเฉพาะ 2 โซนที่ใกล้ราคาที่สุดต่อฝั่ง, รวมโซน TF เล็กที่ซ้อนในโซน TF ใหญ่เป็นป้ายเดียว (เช่น `OB 4H+1H`), โซน 4H / 1D จางลง, FVG หดตามส่วนที่ถูกเติม, จำกัดความหนา OB ตาม ATR, ซ่อนโซนที่ถูกใช้แล้ว |
| **Dashboard** | เทรนด์ของแต่ละ TF แบบแถวเดียว เช่น `5m ▼ · 15m ▼ · 1H ▼ · 4H ▲ · 1D ▼` และราคาที่ทำให้เทรนด์กลับทิศ (flip level) ใต้แต่ละช่อง (เลือกแบบตารางเต็มได้) |
| **Alerts / แจ้งเตือนมือถือ** | แจ้งเตือนเมื่อราคาเข้าโซน OB/FVG ของ 5m, 15m, 1H, 4H, 1D (ครั้งเดียวต่อโซน), ราคาเข้าใกล้โซน 1H/4H/1D, เทรนด์ 1H/4H กลับทิศ และเลือกเพิ่มได้: OB/FVG ใหม่, CHoCH/BOS, กวาด liquidity |

## วิธีติดตั้ง

1. เปิด TradingView → แท็บ **Pine Editor** ด้านล่างกราฟ
2. ถ้ามีสคริปต์ SignalTW อยู่แล้ว ให้เปิดสคริปต์นั้น ถ้ายังไม่มี ให้ **Create new → Indicator**
3. ลบโค้ดเดิมทั้งหมด แล้ววางโค้ดจากไฟล์ [`SMC_OrderBlock_FVG_MTF.pine`](SMC_OrderBlock_FVG_MTF.pine)
4. กด **Save** แล้วกด **Add to chart** (ถ้าเคยใส่ไว้บนกราฟแล้ว แนะนำให้ลบออกแล้ว Add ใหม่ เพื่อให้ใช้ค่าเริ่มต้นใหม่)

## การดูหลาย TF (HTF set)

- **5m · 15m · 1H · 4H · 1D** (ค่าเริ่มต้น): ใช้ชุดนี้เสมอ TF ที่ต่ำกว่าหรือเท่ากับกราฟจะใช้โซนของกราฟแทน เปิดกราฟ **5m** จะได้ครบทั้ง 5 TF
- **Auto by chart TF**: 1m → 5m/15m/1H, 3m–5m → 15m/1H/4H, 15m → 1H/4H/D, 1H → 4H/D/W
- **Manual**: เลือก HTF 1–5 เอง
- HTF จะแสดงเฉพาะ TF ที่สูงกว่ากราฟ (เช่น เปิดกราฟ 15m จะไม่มีโซน 5m)

## แจ้งเตือนเข้ามือถือ เมื่อราคาเข้าโซน OB / FVG

1. ติดตั้งแอป **TradingView** บนมือถือ แล้ว login บัญชีเดียวกับบนคอมพิวเตอร์ และอนุญาตการแจ้งเตือนของแอป
2. บนคอมพิวเตอร์ เปิดกราฟ **XAUUSD 5m** ที่มี SignalTW อยู่ (ต้องเป็น 5m หรือเล็กกว่า จึงจะได้โซน 5m ด้วย)
3. กด **Alert** (⏰) → **Condition** เลือก `SignalTW OB/FVG` → เลือก **Any alert() function call**
4. แท็บ **Notifications** เปิด **Notify in app** (ส่งเข้ามือถือ) แล้วกด **Create**

- Alert ทำงานบน server ของ TradingView ปิดคอมหรือเปลี่ยนไปดู TF อื่นก็ยังแจ้งเตือน
- แจ้ง **ครั้งแรก** ที่ราคาเข้าแต่ละโซน (ไม่แจ้งซ้ำทุกแท่งระหว่างที่ราคาอยู่ในโซน) และแจ้งทันทีที่ราคาแตะ ไม่ต้องรอปิดแท่ง
- ตัวอย่างข้อความ: `XAUUSD 4152.300 entered Bullish OB 1H 4150.200–4155.800 | Bullish FVG 15m 4151.000–4153.400`
- เลือก TF (5m / 15m / 1H / 4H / 1D) และประเภท (OB / FVG) ที่ต้องการได้ในกลุ่ม **Alerts (phone notifications)** ถ้าแจ้งถี่เกินไป แนะนำปิด 5m
- **ราคาเข้าใกล้โซน 1H / 4H / 1D** (เปิดเป็นค่าเริ่มต้น): เตือนล่วงหน้าเมื่อราคาห่างจากโซนไม่เกิน 2 × ATR14 ของ 5m ครั้งเดียวต่อโซน เช่น `XAUUSD 4160.200 approaching Bullish OB 4H 4140.000–4145.500 (14.7 away)` โซนที่เกิดใกล้ราคาจะไม่เตือนจนกว่าราคาจะเคยห่างออกไปก่อน
- **เทรนด์ 1H / 4H กลับทิศ** (เปิดเป็นค่าเริ่มต้น): แจ้งหลังแท่ง 1H / 4H ปิด เช่น `XAUUSD 4148.300 1H trend turned Bearish (closed below 4150.2)`
- แจ้งเตือนประเภทอื่น (OB/FVG ใหม่, CHoCH/BOS, กวาด liquidity, RSI divergence) ปิดไว้เป็นค่าเริ่มต้น เปิดได้ในกลุ่มเดียวกัน
- หลังแก้ Settings ของ indicator ต้องลบ Alert เดิมแล้วสร้างใหม่ เพราะ Alert จำค่าตอนที่สร้างไว้
- แพ็กเกจฟรีของ TradingView จำกัดจำนวน Alert และ Alert มีวันหมดอายุ (ตั้งได้ตอนสร้าง) วิธีนี้ใช้ Alert เดียวครอบคลุมทุก TF

## Liquidity

แนวที่มี stop order กองอยู่ ราคามักวิ่งไปกวาดก่อนกลับตัว (เฉพาะกราฟ intraday)

| แนว | ความหมาย | ค่าเริ่มต้น |
|---|---|---|
| **PDH / PDL** | High / Low ของวันก่อนหน้า | เปิด |
| **PWH / PWL** | High / Low ของสัปดาห์ก่อนหน้า | ปิด |
| **Asia H / Asia L** | High / Low ของ Asian session 00:00–07:00 UTC (07:00–14:00 ไทย) มักถูกกวาดตอน London เปิด | เปิด |
| **EQH / EQL** | Swing high / low 2 จุดที่เท่ากัน (ห่างกันไม่เกิน 0.1 ATR) จุดที่ stop กองรวมกัน | เปิด (สูงสุด 6 เส้น) |

- เส้นจะหยุดและจางลงเมื่อราคาผ่าน (ไม่มีป้ายข้อความเพื่อลดความรก): ถ้าไส้ทะลุแล้วปิดกลับ (กวาด liquidity) จะมี `$` ที่แท่งนั้น ชี้เมาส์ดูชื่อแนว

## การตั้งแจ้งเตือน

กด **Create Alert** → Condition เลือก `SignalTW OB/FVG` แล้วเลือก

- **Any alert() function call**: ข้อความละเอียด (ระบุ TF, ช่วงราคาของโซน, ชื่อแนว liquidity) ตามที่เปิดไว้ในกลุ่ม Alerts
- หรือเงื่อนไขเฉพาะ: `Price entered Bullish OB/FVG`, `Price entered Bearish OB/FVG`, `New Bullish OB/FVG (any TF)`, `New Bearish OB/FVG (any TF)`, `Bullish BOS/CHoCH`, `Bearish BOS/CHoCH`, `Buy-side liquidity swept`, `Sell-side liquidity swept`, `Bullish RSI divergence`, `Bearish RSI divergence`

## การตั้งค่าที่สำคัญ

| ค่า | ความหมาย |
|---|---|
| Swing length | ความยาว pivot สำหรับหา Swing High/Low (ค่ามาก = โครงสร้างใหญ่ขึ้น โซนน้อยลง) |
| Show only nearest zones | แสดงเฉพาะ N โซนที่ใกล้ราคาที่สุดต่อฝั่ง (แยก OB / FVG, ค่าเริ่มต้น 2) |
| Hide lower-TF zones inside higher-TF zones | ซ่อนโซน TF เล็กที่ซ้อนกับโซน TF ใหญ่ และเพิ่มชื่อ TF ในป้าย |
| Hide mitigated zones | ซ่อนโซนที่ราคาเข้าไปใช้แล้ว |
| Fade 4H / 1D zones | โซน 4H จางลง, 1D ขึ้นไปจางมาก ไม่บังแท่งเทียน (เปิดเป็นค่าเริ่มต้น) |
| 50% line (mean threshold) | เส้นประกลางโซน OB / FVG (ปิดเป็นค่าเริ่มต้น) |
| Shrink FVG as it fills | FVG หดเหลือเฉพาะช่องว่างที่ยังไม่ถูกเติม (เปิดเป็นค่าเริ่มต้น) |
| Label size | ขนาดตัวอักษรป้ายโซน |
| History: CHoCH / BOS / swept liquidity (days) | แสดง CHoCH / BOS, `$` และแนว liquidity ที่ถูกกวาดแล้ว ย้อนหลังกี่วัน (ค่าเริ่มต้น 2, 0 = ทั้งหมด) แนวที่ยังไม่ถูกกวาดแสดงเสมอ |
| Max OB / FVG stored per TF / side | จำนวนโซนสูงสุดที่เก็บไว้ต่อ TF ต่อฝั่ง |
| OB mitigation | `Close` = ปิดแท่งทะลุโซน, `Wick` = ไส้เทียนทะลุโซน |
| Max OB height (x ATR) | จำกัดความหนา OB, `Trim` = ตัดเหลือส่วนที่ใกล้ราคา, `Skip` = ไม่แสดง |
| Min gap size (%) | กรอง FVG ที่เล็กเกินไป |
| BOS / CHoCH | `CHoCH only` (ค่าเริ่มต้น) / `All` / `None` |
| CHoCH / BOS timeframe | `Auto` (ค่าเริ่มต้น: กราฟ 5m หรือเล็กกว่าใช้ 15m, TF อื่นใช้ TF ของกราฟ) / `Chart TF` / `15m` / `1H` ป้ายของ TF ที่สูงกว่ากราฟจะมีชื่อ TF ต่อท้าย เช่น `CHoCH 15m` |
| Liquidity | เปิด/ปิดแต่ละแนว, เวลา Asia session, ความใกล้ของ EQH/EQL, สี |
| RSI Divergence | เปิด/ปิด, RSI length (14), Pivot left / right (5 / 3: ป้ายขึ้นหลังจุด Low / High 3 แท่ง), ระยะห่างสูงสุดของ 2 จุด (60 แท่ง), กรองเฉพาะ RSI จุดแรก ≤ 40 (Bull) / ≥ 60 (Bear) |
| Show trend dashboard | เปิด/ปิดตารางเทรนด์ ตำแหน่ง และรูปแบบ (`Compact` แถวเดียว เหมาะกับมือถือ / `Full` ตาราง TF–Trend) |
| Show flip levels | ราคาที่ต้องปิดผ่าน เทรนด์ของ TF นั้นจึงจะกลับทิศ: `> 4168.5` = ปิดเหนือแล้วกลับเป็น Bullish, `< 4140.2` = ปิดต่ำกว่าแล้วกลับเป็น Bearish, `–` = ยังไม่มี Swing ใหม่ (แบบ Full แสดงระยะห่างจากราคาด้วย) |

> ⚠️ อินดิเคเตอร์นี้เป็นเครื่องมือช่วยวิเคราะห์ ไม่ใช่คำแนะนำการลงทุน
