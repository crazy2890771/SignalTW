# SignalTW – Order Block & FVG [MTF] สำหรับ TradingView

อินดิเคเตอร์ Pine Script v6 แสดงสัญญาณ Smart Money Concepts บนกราฟ TradingView และใช้ได้กับทุก Timeframe

## ฟีเจอร์

| ฟีเจอร์ | รายละเอียด |
|---|---|
| **Order Block (OB)** | หาจากการเบรกโครงสร้าง (BOS/CHoCH): Bullish OB คือแท่งที่ low ต่ำสุดของขาก่อนเบรก Swing High, Bearish OB คือแท่งที่ high สูงสุดของขาก่อนเบรก Swing Low |
| **Fair Value Gap (FVG)** | ช่องว่างราคาจาก 3 แท่ง (Bullish: `low > high[2]`, Bearish: `high < low[2]`) กรองขนาดขั้นต่ำเป็น % ได้ |
| **Multi-Timeframe** | แสดงโซนของ TF กราฟปัจจุบัน และ HTF เพิ่มได้อีก 3 TF พร้อมกัน (ค่าเริ่มต้น 1H, 4H, D) ป้ายบอก TF เช่น `OB 4H`, `FVG 1H` และใช้เส้นขอบประ |
| **ไม่ Repaint** | ข้อมูล HTF ใช้เฉพาะแท่งที่ปิดแล้ว ส่วน TF ปัจจุบันจะวาดโซนเมื่อแท่งปิด |
| **ลดความรก** | แสดงเฉพาะโซนที่ใกล้ราคาที่สุด N โซนต่อฝั่ง, โซน TF เล็กที่ซ้อนอยู่ในโซน TF ใหญ่จะถูกรวมเป็นป้ายเดียว เช่น `OB 4H+1H`, OB ที่หนาเกินจะถูกตัดตาม ATR, FVG ที่ซ้อนกันใน TF เดียวกันจะรวมเป็นโซนเดียว |
| **Mitigation** | โซนที่ราคาเข้าไปใช้แล้วจะถูกซ่อน (หรือหยุดยืดและทำให้จางลง ปรับได้ในการตั้งค่า) |
| **BOS / CHoCH** | เส้นและป้ายโครงสร้างตลาดบน TF ปัจจุบัน (ค่าเริ่มต้นแสดงเฉพาะ CHoCH) |
| **BUY / SELL** | BUY เมื่อราคาย่อลงแตะโซน Bullish (OB/FVG) แล้วปิดแท่งเขียวกลับขึ้นเหนือโซน, SELL กลับกัน กรองด้วยเทรนด์ HTF พร้อมเส้น Entry / SL / TP ของสัญญาณล่าสุด (เอาเมาส์ชี้ป้ายเพื่อดูราคา) |
| **Backtest (หลังหักต้นทุน)** | ทดสอบย้อนหลังบนข้อมูลที่โหลดในกราฟ โดยคิด spread + slippage, ถือได้ทีละออเดอร์, มี time stop ป้ายสัญญาณจะเปลี่ยนเป็น `BUY ✔ +1.9R` / `BUY ✘ -1.1R` / `BUY ⏱ +0.3R` |
| **Session / News / Volatility** | เข้าเทรดเฉพาะช่วง London / New York, ไม่เข้าช่วง rollover, ช่วงประกาศข่าว 08:30 NY และข่าวที่ใส่เอง, ไม่เข้าเมื่อ ATR ต่ำหรือสูงผิดปกติ |
| **SL / Entry** | SL ใต้ไส้เทียนที่กวาด liquidity + buffer ที่คิด spread, ข้ามเทรดที่ SL แคบหรือกว้างเกิน หรือราคาวิ่งไปไกลแล้ว, เลือกเข้าแบบ Market หรือ Limit ที่ขอบโซนได้ |
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
  - หรือเลือกเงื่อนไขเฉพาะ เช่น `BUY signal`, `SELL signal`, `New Bullish OB/FVG (any TF)`, `Price in Bullish zone`, `Bullish BOS/CHoCH`

## การตั้งค่าที่สำคัญ

| ค่า | ความหมาย |
|---|---|
| Swing length | ความยาว pivot สำหรับหา Swing High/Low (ค่ามาก = โครงสร้างใหญ่ขึ้น สัญญาณน้อยลง) |
| Show only nearest zones | แสดงเฉพาะ N โซนที่ใกล้ราคาที่สุดต่อฝั่ง (แยก OB / FVG) |
| Hide lower-TF zones inside higher-TF zones | ซ่อนโซน TF เล็กที่ซ้อนกับโซน TF ใหญ่ และเพิ่มชื่อ TF ในป้าย |
| Max OB height (x ATR) | จำกัดความหนา OB, `Trim` = ตัดเหลือส่วนที่ใกล้ราคา, `Skip` = ไม่แสดง |
| Label size | ขนาดตัวอักษรป้ายโซน |
| BOS / CHoCH | `All` / `CHoCH only` / `None` |
| Signal frequency | `Low` / `Medium` (ค่าเริ่มต้น) / `High` / `Custom` ตั้งค่าตัวกรองสัญญาณทั้งหมดในคลิกเดียว (Custom = ใช้ค่าที่ตั้งเองด้านล่าง) |
| Trend filter | `HTF trend or CHoCH` = BUY เมื่อ HTF สูงสุดเป็นขาขึ้น หรือ HTF ที่ต่ำที่สุด (เช่น 1H) เกิด CHoCH กลับเป็นขาขึ้นแล้ว, `HTF trend`, `Chart trend`, `Both`, `None` |
| Signal zones | ใช้โซนไหนออกสัญญาณ (`OB + FVG` / `OB only` / `FVG only`, `HTF only` / `HTF + chart OB` / `All TF`) |
| Premium / Discount filter | BUY เฉพาะครึ่งล่างของกรอบราคา HTF (N แท่งล่าสุด), SELL เฉพาะครึ่งบน |
| Min move away before retest (x ATR) | ราคาต้องวิ่งออกจากโซนอย่างน้อยเท่านี้ก่อน แล้วย้อนกลับมาแตะ จึงนับเป็นสัญญาณ  |
| Min minutes between signals | ระยะเวลาขั้นต่ำระหว่างสัญญาณ เป็นนาที |
| R:R | อัตราส่วน TP ต่อ SL |
| Max OB / FVG stored per TF / side | จำนวนโซนสูงสุดที่เก็บไว้ต่อ TF ต่อฝั่ง |
| OB mitigation | `Close` = ปิดแท่งทะลุโซน, `Wick` = ไส้เทียนทะลุโซน |
| Min gap size (%) | กรอง FVG ที่เล็กเกินไป |
| Hide mitigated zones | ซ่อนโซนที่ถูกใช้แล้ว |


### ระดับสัญญาณ (Signal frequency)

| | Low | Medium | High |
|---|---|---|---|
| โซนที่ใช้ | HTF | HTF + OB ของกราฟ | ทุกโซน |
| ตัวกรองเทรนด์ | HTF | HTF หรือ CHoCH กลับตัวของ HTF ที่ต่ำกว่า | HTF หรือ CHoCH กลับตัวของ HTF ที่ต่ำกว่า |
| Premium / Discount | ✅ | ✅ | ❌ |
| ระยะออกจากโซนก่อนกลับมาแตะ | 1.0 ATR | 0.75 ATR | 0.5 ATR |
| ระยะห่างระหว่างสัญญาณ | 120 นาที | 60 นาที | 30 นาที |

## Execution & costs (backtest)

ราคาบนกราฟถือเป็นราคา **Bid** (มาตรฐานของ TradingView สำหรับ Forex / ทองคำ)

| เหตุการณ์ | ราคาที่ใช้ |
|---|---|
| BUY แบบ Market | ราคาปิด + spread + slippage (ซื้อที่ Ask) |
| SELL แบบ Market | ราคาปิด − slippage |
| SL ของ BUY | โดนเมื่อ Bid (low) แตะ SL, ออกที่ SL − slippage |
| SL ของ SELL | โดนเมื่อ Ask (high + spread) แตะ SL, ออกที่ SL + slippage |
| TP | เป็น limit ไม่มี slippage (TP ของ SELL โดนด้วย Ask) |
| แท่งเดียวโดนทั้ง SL และ TP | นับเป็นแพ้ |
| Time stop | ปิดที่ราคาปิดแท่ง เมื่อถือครบเวลาที่ตั้ง |

| ค่า | ค่าเริ่มต้น | ความหมาย |
|---|---|---|
| Spread / Slippage | 0.30 / 0.10 | หน่วยเป็นราคา (XAUUSD) ถ้าใช้กับสินค้าอื่นต้องปรับ |
| Entry | Market | `Market (bar close)` หรือ `Limit at zone edge` (รอราคาย้อนกลับมาที่ขอบโซน ยกเลิกเมื่อครบจำนวนแท่ง หรือราคาไปถึง TP ก่อน) |
| Stop-loss at | Signal wick | ใต้ไส้เทียนต่ำสุดของแท่งสัญญาณและแท่งก่อนหน้า (BUY) หรือเลือก `Zone edge` แบบเดิม |
| SL buffer | 2 × spread หรือ 0.15 ATR | ใช้ค่าที่มากกว่า, SELL บวก spread ให้อีก |
| Skip if SL < x spread | 3 | SL แคบเกิน ต้นทุนกินกำไร |
| Skip if SL > x ATR | 1.5 | SL กว้างเกินสำหรับการเทรดสั้น |
| Skip if close ran > x R | 0.5 | แท่งสัญญาณปิดไกลจากโซนเกินไป ไม่ไล่ราคา |
| Time stop | 240 นาที | ปิดเทรดที่ค้างนานเกิน |
| One position at a time | เปิด | ระหว่างมีออเดอร์ (รวม limit ที่รอ) จะข้ามสัญญาณใหม่ |
| Show filtered signals (×) | ปิด | แสดง × ตรงสัญญาณที่ถูกกรอง ชี้เมาส์ดูเหตุผล |

## Session / news / volatility

ใช้กับกราฟ TF ไม่เกิน 1H เวลาอ้างอิงตามเมืองนั้น ๆ จึงปรับ Daylight Saving อัตโนมัติ (พื้นหลังสีฟ้า = ช่วงเทรด, สีแดง = ช่วงห้ามเทรด)

| ค่า | ค่าเริ่มต้น | หมายเหตุ |
|---|---|---|
| London | 08:00–11:30 เวลาลอนดอน | ≈ 14:00–17:30 ไทย (ช่วง BST) / 15:00–18:30 (ช่วง GMT) |
| New York | 08:00–11:30 เวลานิวยอร์ก | ≈ 19:00–22:30 ไทย (ช่วง EDT) / 20:00–23:30 (ช่วง EST) |
| Block rollover | 16:45–18:15 NY | spread ทองถ่างมาก |
| Block US data window | 08:20–09:00 NY อังคาร–ศุกร์ | ช่วงประกาศข่าว 08:30 เช่น CPI, NFP |
| News 1–3 | ปิด | ใส่เวลาข่าวแรงอื่น เช่น FOMC แล้วจะไม่เข้าเทรด 15 นาทีก่อนถึง 30 นาทีหลัง |
| ATR / avg ATR | 0.6 – 2.5 | ATR14 เทียบค่าเฉลี่ย 100 แท่ง ต่ำเกิน = ตลาดเงียบ, สูงเกิน = ช่วงกระชาก |

## ตาราง Backtest

| แถว | ความหมาย |
|---|---|
| Trades (closed / open) | เทรดที่จบแล้ว / ที่ยังค้างหรือรอ limit |
| TP / SL / Time stop | จำนวนเทรดที่จบด้วยแต่ละแบบ |
| Win rate | % เทรดที่ได้กำไร (R > 0) |
| **Expectancy / trade** | **กำไรเฉลี่ยต่อเทรดหลังหักต้นทุน เป็นตัวเลขหลักที่ต้องเป็นบวก** |
| Net result / Profit factor | กำไรรวม (R) / กำไรรวม ÷ ขาดทุนรวม |
| Max drawdown / Max losing streak | ขาดทุนสะสมสูงสุดจากจุดสูงสุด (R) / แพ้ติดกันมากสุด |
| Avg SL / cost | ระยะ SL เฉลี่ยเป็นราคา / ต้นทุน spread + slippage เฉลี่ยเป็น R |
| Avg hold | เวลาถือเฉลี่ย |
| BUY / SELL | จำนวน · win rate · กำไรสุทธิ แยกฝั่ง |
| Filtered: time / setup | จำนวนสัญญาณที่ถูกกรองทิ้งด้วยแต่ละเหตุผล |

> ⚠️ อินดิเคเตอร์นี้เป็นเครื่องมือช่วยวิเคราะห์ ไม่ใช่คำแนะนำการลงทุน ผล backtest ในอดีตไม่รับประกันผลในอนาคต
