# SignalTW – Order Block & FVG [MTF] สำหรับ TradingView

อินดิเคเตอร์ Pine Script v6 แสดงสัญญาณ Smart Money Concepts บนกราฟ TradingView และใช้ได้กับทุก Timeframe

## ฟีเจอร์

| ฟีเจอร์ | รายละเอียด |
|---|---|
| **Order Block (OB)** | หาจากการเบรกโครงสร้าง (BOS/CHoCH): Bullish OB คือแท่งที่ low ต่ำสุดของขาก่อนเบรก Swing High, Bearish OB คือแท่งที่ high สูงสุดของขาก่อนเบรก Swing Low |
| **Fair Value Gap (FVG)** | ช่องว่างราคาจาก 3 แท่ง (Bullish: `low > high[2]`, Bearish: `high < low[2]`) กรองขนาดขั้นต่ำเป็น % ได้ |
| **Multi-Timeframe** | แสดงโซนของ TF กราฟปัจจุบัน และ HTF เพิ่มได้อีก 3 TF พร้อมกัน (ค่าเริ่มต้น 15m, 1H, 4H) ป้ายบอก TF เช่น `OB 4H`, `FVG 1H` และใช้เส้นขอบประ |
| **ไม่ Repaint** | ข้อมูล HTF ใช้เฉพาะแท่งที่ปิดแล้ว ส่วน TF ปัจจุบันจะวาดโซนเมื่อแท่งปิด |
| **ลดความรก** | แสดงเฉพาะโซนที่ใกล้ราคาที่สุด N โซนต่อฝั่ง, โซน TF เล็กที่ซ้อนอยู่ในโซน TF ใหญ่จะถูกรวมเป็นป้ายเดียว เช่น `OB 4H+1H`, OB ที่หนาเกินจะถูกตัดตาม ATR, FVG ที่ซ้อนกันใน TF เดียวกันจะรวมเป็นโซนเดียว |
| **Mitigation** | โซนที่ราคาเข้าไปใช้แล้วจะถูกซ่อน (หรือหยุดยืดและทำให้จางลง ปรับได้ในการตั้งค่า) |
| **BOS / CHoCH** | เส้นและป้ายโครงสร้างตลาดบน TF ปัจจุบัน (ค่าเริ่มต้นปิด เลือก `All` / `CHoCH only` ได้) |
| **BUY / SELL** | 2 แบบ: **LTF CHoCH** (ค่าเริ่มต้น) = ราคาเข้าโซน HTF → กวาด liquidity → CHoCH ด้วยแท่งแรงที่ทิ้ง FVG → เข้าที่ FVG และ **Zone retest** = ไส้เทียนแตะโซนแล้วปิดกลับ (แบบเดิม) กรองด้วยเทรนด์ HTF พร้อมเส้น Entry / SL / TP (เอาเมาส์ชี้ป้ายเพื่อดูราคาและรายละเอียด setup) |
| **Backtest (หลังหักต้นทุน)** | ทดสอบย้อนหลังบนข้อมูลที่โหลดในกราฟ โดยคิด spread + slippage, ถือได้ทีละออเดอร์, มี time stop ป้ายสัญญาณจะเปลี่ยนเป็น `BUY ✔ +1.9R` / `BUY ✘ -1.1R` / `BUY ⏱ +0.3R` |
| **Session / News / Volatility** | เข้าเทรดเฉพาะช่วง London / New York, ไม่เข้าช่วง rollover, ช่วงประกาศข่าว 08:30 NY และข่าวที่ใส่เอง, ไม่เข้าเมื่อ ATR ต่ำหรือสูงผิดปกติ |
| **SL / Entry** | SL ใต้ไส้เทียนที่กวาด liquidity + buffer ที่คิด spread, ข้ามเทรดที่ SL แคบหรือกว้างเกิน หรือราคาวิ่งไปไกลแล้ว, เลือกเข้าแบบ Market หรือ Limit ที่ขอบโซนได้ |
| **Liquidity** | แนว liquidity: PDH/PDL, PWH/PWL, Asia High/Low, Equal Highs/Lows (EQH/EQL) และเครื่องหมาย `$` เมื่อถูกกวาด (ไส้ทะลุแล้วปิดกลับ) สัญญาณที่กวาดแนวสำคัญจะมี `$` ในป้าย เช่น `BUY $` |
| **Dashboard** | ตารางแสดงเทรนด์ของแต่ละ TF |
| **Alerts** | แจ้งเตือนเมื่อเกิด OB/FVG ใหม่, ราคาเข้าโซน, เกิด BOS/CHoCH |

## วิธีติดตั้ง

1. เปิด TradingView → แท็บ **Pine Editor** ด้านล่างกราฟ
2. คัดลอกโค้ดทั้งหมดจากไฟล์ [`SMC_OrderBlock_FVG_MTF.pine`](SMC_OrderBlock_FVG_MTF.pine) ไปวาง (แทนโค้ดเดิม)
3. กด **Save** แล้วกด **Add to chart**
4. ปรับค่าได้ที่ ⚙️ Settings ของอินดิเคเตอร์

## การดูหลาย TF

- **Chart TF**: โซนของ TF ที่เปิดอยู่ (เปลี่ยน TF กราฟเมื่อไรก็คำนวณใหม่ตาม TF นั้น)
- **Auto HTF** (ค่าเริ่มต้น): เลือก HTF ให้ตาม TF ของกราฟ: 1m → 5m/15m/1H, 3m–5m → 15m/1H/4H, 15m → 1H/4H/D, 1H → 4H/D/W
- **HTF 1/2/3**: ปิด Auto HTF แล้วเลือก TF เองได้ (ค่าเริ่มต้น 15m / 1H / 4H)
- HTF จะแสดงเฉพาะเมื่อสูงกว่า TF ของกราฟ (เช่น เปิดกราฟ 4H ก็จะเห็นเฉพาะโซน 4H และ D)

## การตั้งแจ้งเตือน

- กด **Create Alert** → Condition เลือก `SignalTW OB/FVG`
  - เลือก **Any alert() function call** เพื่อรับข้อความละเอียด (ระบุ TF และช่วงราคาของโซน)
  - หรือเลือกเงื่อนไขเฉพาะ เช่น `BUY signal`, `SELL signal`, `Buy-side liquidity swept`, `Sell-side liquidity swept`, `New Bullish OB/FVG (any TF)`, `Price in Bullish zone`, `Bullish BOS/CHoCH`

## การตั้งค่าที่สำคัญ

| ค่า | ความหมาย |
|---|---|
| Swing length | ความยาว pivot สำหรับหา Swing High/Low (ค่ามาก = โครงสร้างใหญ่ขึ้น สัญญาณน้อยลง) |
| Show only nearest zones | แสดงเฉพาะ N โซนที่ใกล้ราคาที่สุดต่อฝั่ง (แยก OB / FVG) |
| Hide lower-TF zones inside higher-TF zones | ซ่อนโซน TF เล็กที่ซ้อนกับโซน TF ใหญ่ และเพิ่มชื่อ TF ในป้าย |
| Max OB height (x ATR) | จำกัดความหนา OB, `Trim` = ตัดเหลือส่วนที่ใกล้ราคา, `Skip` = ไม่แสดง |
| Label size | ขนาดตัวอักษรป้ายโซน |
| BOS / CHoCH | `All` / `CHoCH only` / `None` |
| Signal frequency | `Scalp` (ค่าเริ่มต้น) / `High` / `Medium` / `Low` / `Custom` ตั้งค่าตัวกรองสัญญาณทั้งหมดในคลิกเดียว (Custom = ใช้ค่าที่ตั้งเองด้านล่าง) |
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

| | Scalp (ค่าเริ่มต้น) | High | Medium | Low |
|---|---|---|---|---|
| โซนที่ใช้ | ทุกโซน (รวมโซนของกราฟ) | ทุกโซน | HTF + OB ของกราฟ | HTF |
| ตัวกรองเทรนด์ | HTF หรือ CHoCH กลับตัว | HTF หรือ CHoCH กลับตัว | HTF หรือ CHoCH กลับตัว | HTF |
| Premium / Discount | ❌ | ❌ | ✅ | ✅ |
| ระยะออกจากโซนก่อนกลับมาแตะ | 0.5 ATR | 0.5 ATR | 0.75 ATR | 1.0 ATR |
| ระยะห่างระหว่างสัญญาณ | 15 นาที | 30 นาที | 60 นาที | 120 นาที |

## Entry model

| | LTF CHoCH (ค่าเริ่มต้น) | Zone retest |
|---|---|---|
| แนวคิด | ใช้ HTF หาโซน แล้วรอการกลับตัวบน TF ของกราฟ (TF สำหรับเข้าเทรด) | ไส้เทียนแตะโซนแล้วปิดกลับ |
| TF กราฟที่แนะนำ | **1m – 5m** (โซนจาก Auto HTF) | 15m |
| โซนที่ใช้ | โซนของ HTF (โซนของ TF กราฟใช้เมื่อ Signal zones = `All TF` หรือไม่มี HTF) | ตาม Signal zones |

ขั้นตอนของ **LTF CHoCH** (BUY; SELL กลับกัน)

1. **Arm:** ราคาเข้าโซน Bullish ของ HTF ที่ยังไม่ถูกใช้ (ต้องเคยวิ่งออกจากโซนมาก่อน)
2. **Sweep:** ไส้เทียนทะลุ swing low ย่อยล่าสุดของกราฟ (กวาด stop / liquidity)
3. **Displacement:** แท่งเขียวแรง (ตัวเทียน ≥ 50% ของแท่ง และยาว ≥ 0.8 ATR) ทิ้ง Bullish FVG ไว้
4. **CHoCH:** ราคาปิดเหนือ swing high ย่อยล่าสุด
5. **Entry:** เข้าที่ราคาปิดแท่งยืนยัน (หรือ Limit ที่ขอบ / 50% ของ FVG), **SL** ใต้จุดต่ำสุดที่กวาด + buffer, **TP** = R:R
6. ต้องครบภายใน **Setup window** (12 แท่งของกราฟ) และถ้าราคาปิดหลุดทั้งโซน setup จะถูกยกเลิก

| ค่า | ค่าเริ่มต้น | ความหมาย |
|---|---|---|
| LTF entry | Market on confirm | `Market on confirm` (เข้าทุกสัญญาณ) / `FVG edge` / `FVG 50%` (Limit: SL แคบกว่าแต่บางครั้งไม่ได้เข้า) |
| Require liquidity sweep | เปิด | ต้องกวาด swing ย่อยก่อน CHoCH |
| Internal swing length | 2 | ความไวของ swing ย่อยที่ใช้หา sweep และ CHoCH |
| Displacement | body ≥ 0.5 × range, range ≥ 0.8 ATR | ความแรงของแท่งที่สร้าง FVG |
| Setup window | 12 แท่ง | จำนวนแท่งสูงสุดจากแตะโซนจนเกิดสัญญาณ (กราฟ 5m = 1 ชั่วโมง) |

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
| Retest entry | Market | (เฉพาะ Zone retest) `Market (bar close)` หรือ `Limit at zone edge` |
| Limit order valid (bars) | 4 | Limit ที่ยังไม่ได้เข้าจะยกเลิกเมื่อครบจำนวนแท่ง หรือราคาไปถึง TP ก่อน |
| Retest stop-loss at | Signal wick | (เฉพาะ Zone retest) ใต้ไส้เทียนต่ำสุดของแท่งสัญญาณและแท่งก่อนหน้า (BUY) หรือเลือก `Zone edge` แบบเดิม |
| SL buffer | 2 × spread หรือ 0.15 ATR | ใช้ค่าที่มากกว่า, SELL บวก spread ให้อีก |
| Skip if SL < x spread | 3 | SL แคบเกิน ต้นทุนกินกำไร |
| Skip if SL > x ATR | 3.0 | SL กว้างเกิน (เทียบ ATR ของกราฟ) |
| Skip if close ran > x R | 0.5 | แท่งสัญญาณปิดไกลจากโซนเกินไป ไม่ไล่ราคา |
| Time stop | 120 นาที | ปิดเทรดที่ค้างนานเกิน |
| One position at a time | เปิด | ระหว่างมีออเดอร์ (รวม limit ที่รอ) จะข้ามสัญญาณใหม่ |
| Show unfilled limit orders | ปิด | แสดงป้าย `LMT ⌀` ของคำสั่ง Limit ที่ราคาไม่ย้อนกลับมาให้เข้า (ไม่นับเป็นเทรด) |
| Show filtered signals (×) | ปิด | แสดง × ตรงสัญญาณที่ถูกกรอง ชี้เมาส์ดูเหตุผล |

## Exit management

| ค่า | ค่าเริ่มต้น | ความหมาย |
|---|---|---|
| Partial take-profit + break-even | เปิด | ปิด **50% ที่ 1R** แล้วเลื่อน SL ของส่วนที่เหลือไปที่ราคาเข้า ส่วนที่เหลือวิ่งไป TP (R:R) ป้ายผลลัพธ์ `½` = ปิดส่วนที่เหลือที่จุดคุ้มทุน |
| TP1 at x R / close % | 1.0R / 50% | ต้องน้อยกว่า R:R ของ TP สุดท้าย |
| Skip if opposing HTF zone < x R away | 0 (ปิด) | ไม่เข้าเทรดถ้ามีโซน HTF ฝั่งตรงข้ามขวางอยู่ใกล้กว่า x R (เช่น BUY แต่มีโซน Bearish อยู่ข้างบนใกล้ ๆ) เปิดแล้วสัญญาณจะลดลงมาก |

ผลของเทรดคำนวณเป็น R ของทั้งออเดอร์: เช่น ปิด 50% ที่ 1R แล้วส่วนที่เหลือไปถึง 2R = +1.5R, ส่วนที่เหลือโดน break-even = +0.5R (หักต้นทุนแล้ว)
ถ้าในแท่งเดียวราคาแตะ TP1 แล้วย้อนกลับมาถึงราคาเข้า จะนับว่าส่วนที่เหลือโดน break-even (สมมติกรณีแย่กว่า)

## Risk management

| ค่า | ค่าเริ่มต้น | ความหมาย |
|---|---|---|
| Account balance | 10,000 | ทุนที่ใช้คำนวณ lot |
| Risk per trade (%) | 0.5% | lot = (ทุน × %) ÷ (ระยะ SL × หน่วยต่อ lot) แสดงในป้ายสัญญาณ (ชี้เมาส์) และข้อความ Alert |
| Units per 1.0 lot / lot step | 100 / 0.01 | XAUUSD 1 lot = 100 oz |
| Daily limits | เปิด | หยุดรับสัญญาณใหม่ในวันนั้นเมื่อขาดทุนถึง **−2R**, แพ้ติดกัน **3 ครั้ง** หรือเทรดครบ **8 ครั้ง** (วันเทรดของโบรกเกอร์) |

## Liquidity

แนวที่มี stop order กองอยู่ ราคามักวิ่งไปกวาดก่อนกลับตัว (เฉพาะกราฟ intraday)

| แนว | ความหมาย | ค่าเริ่มต้น |
|---|---|---|
| **PDH / PDL** | High / Low ของวันก่อนหน้า | เปิด |
| **PWH / PWL** | High / Low ของสัปดาห์ก่อนหน้า | ปิด |
| **Asia H / Asia L** | High / Low ของ Asian session 00:00–07:00 UTC (07:00–14:00 ไทย) มักถูกกวาดตอน London เปิด | เปิด |
| **EQH / EQL** | Swing high / low 2 จุดที่เท่ากัน (ห่างกันไม่เกิน 0.1 ATR) จุดที่ stop กองรวมกัน | เปิด (สูงสุด 6 เส้น) |

- เส้นจะหยุดเมื่อราคาผ่าน: **swept** = ไส้ทะลุแล้วปิดกลับ (กวาด liquidity) มี `$` ที่แท่งนั้น (ชี้เมาส์ดูชื่อแนว), **taken** = ปิดทะลุไปเลย
- BUY / SELL ที่กวาดแนวสำคัญระหว่าง setup จะมี `$` ต่อท้ายป้าย (เช่น `BUY $`) และระบุชื่อแนวใน tooltip
- `Signals must sweep a key level` (ค่าเริ่มต้นปิด): รับเฉพาะสัญญาณที่กวาดแนวสำคัญ BUY ต้องกวาด sell-side (PDL / Asia L / EQL ...) SELL ต้องกวาด buy-side

## Session / news / volatility

ใช้กับกราฟ TF ไม่เกิน 1H เวลาอ้างอิงตามเมืองนั้น ๆ จึงปรับ Daylight Saving อัตโนมัติ (เปิด `Shade sessions / blocked times` เพื่อแรเงาพื้นหลัง: สีฟ้า = ช่วงเทรด, สีแดง = ช่วงห้ามเทรด)

| ค่า | ค่าเริ่มต้น | หมายเหตุ |
|---|---|---|
| London | 07:00–12:00 เวลาลอนดอน | ≈ 13:00–18:00 ไทย (ช่วง BST) / 14:00–19:00 (ช่วง GMT) |
| New York | 08:00–13:00 เวลานิวยอร์ก | ≈ 19:00–00:00 ไทย (ช่วง EDT) / 20:00–01:00 (ช่วง EST) |
| Block rollover | 16:45–18:15 NY | spread ทองถ่างมาก |
| Block US data window | 08:20–09:00 NY อังคาร–ศุกร์ | ช่วงประกาศข่าว 08:30 เช่น CPI, NFP |
| News 1–3 | ปิด | ใส่เวลาข่าวแรงอื่น เช่น FOMC แล้วจะไม่เข้าเทรด 15 นาทีก่อนถึง 30 นาทีหลัง |
| ATR / avg ATR | 0.6 – 2.5 | ATR14 เทียบค่าเฉลี่ย 100 แท่ง ต่ำเกิน = ตลาดเงียบ, สูงเกิน = ช่วงกระชาก |

## ตาราง Backtest

ค่าเริ่มต้นเป็นแบบ **Compact** (Trades, TP / BE / SL / Time, Win rate, Expectancy, Net, Profit factor, Max drawdown, Avg SL / hold, Setups → trades + ตัวกรองที่ตัดมากที่สุด) ปิด `Compact stats table` เพื่อดูทุกแถว

| แถว | ความหมาย |
|---|---|
| Trades (closed / open) | เทรดที่จบแล้ว / ที่ยังค้างหรือรอ limit |
| TP / BE / SL / Time | จำนวนเทรดที่จบด้วยแต่ละแบบ (BE = ปิดที่ TP1 แล้วส่วนที่เหลือโดน break-even) |
| Win rate | % เทรดที่ได้กำไร (R > 0) |
| **Expectancy / trade** | **กำไรเฉลี่ยต่อเทรดหลังหักต้นทุน เป็นตัวเลขหลักที่ต้องเป็นบวก** |
| Net result / Profit factor | กำไรรวม (R และประมาณเป็น $ ตามทุนและ % ความเสี่ยง) / กำไรรวม ÷ ขาดทุนรวม |
| Max drawdown / Max losing streak | ขาดทุนสะสมสูงสุดจากจุดสูงสุด (R) / แพ้ติดกันมากสุด |
| Avg SL / cost | ระยะ SL เฉลี่ยเป็นราคา / ต้นทุน spread + slippage เฉลี่ยเป็น R |
| Avg hold | เวลาถือเฉลี่ย |
| BUY / SELL | จำนวน · win rate · กำไรสุทธิ แยกฝั่ง |
| Setups → trades | จำนวน setup ที่ครบเงื่อนไข → จำนวนที่ได้เข้าเทรดจริง และตัวกรองที่ตัดทิ้งมากที่สุด |
| Filtered: time / setup / bias / risk | จำนวนสัญญาณที่ถูกกรองทิ้งด้วยแต่ละเหตุผล |

> ⚠️ อินดิเคเตอร์นี้เป็นเครื่องมือช่วยวิเคราะห์ ไม่ใช่คำแนะนำการลงทุน ผล backtest ในอดีตไม่รับประกันผลในอนาคต
