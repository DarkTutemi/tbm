# BẢNG TỔNG MỤC LINH KIỆN, CẢM BIẾN & THIẾT BỊ ĐẶT HÀNG (BILL OF MATERIALS - BOM)
## MÁY KHOAN KÍCH NGẦM TBM MICROTUNNELING (BOHRKOPF 2100 - MTS PERFORATOR)
### HỆ THỐNG ĐIỀU KHIỂN: SIEMENS SIMATIC S7-300 (CPU 315-2 PN/DP)

---

## MỤC LỤC
1. [Hướng Dẫn Tra Cứu & Quy Chuẩn Đặt Hàng (Ordering Guide)](#1-hướng-dẫn-tra-cứu--quy-chuẩn-đặt-hàng)
2. [Cảm Biến Áp Suất Thủy Lực & Khí Nén (Pressure Transmitters)](#2-cảm-biến-áp-suất-thủy-lực--khí-nén)
3. [Cảm Biến Hành Trình & Đo Dịch Chuyển Tuyến Tính (Displacement / Stroke Sensors)](#3-cảm-biến-hành-trình--đo-dịch-chuyển-tuyến-tính)
4. [Cảm Biến Đo Góc Nghiêng, Độ Dốc & Xoay Khiên (Inclinometers)](#4-cảm-biến-đo-góc-nghiêng-độ-dốc--xoay-khiên)
5. [Cảm Biến Lưu Lượng Bùn & Nước Cao Áp (Flowmeters)](#5-cảm-biến-lưu-lượng-bùn--nước-cao-áp)
6. [Cảm Biến Nhiệt Độ, Độ Ẩm Dầu & Mức Dầu Thủy Lực (Temperature, Humidity & Level)](#6-cảm-biến-nhiệt-độ-độ-ẩm-dầu--mức-dầu-thủy-lực)
7. [Hệ Thống Định Vị Laser TACS & Đo Chiều Dài Hầm (Navigation & Encoder)](#7-hệ-thống-định-vị-laser-tacs--đo-chiều-dài-hầm)
8. [Van Tỷ Lệ & Van Điện Từ Thủy Lực (Proportional & Solenoid Valves)](#8-van-tỷ-lệ--van-điện-từ-thủy-lực)
9. [Động Cơ Điện, Biến Tần & Bơm Piston Cao Áp (Motors, VFD & Pumps)](#9-động-cơ-điện-biến-tần--bơm-piston-cao-áp)
10. [Khởi Động Động Cơ Thông Minh & Aptomat Khối (Eaton SmartWire-DT PKE/NZM)](#10-khởi-động-động-cơ-thông-minh--aptomat-khối)
11. [Module PLC Siemens S7-300 & Trạm Thu Thập I/O Turck Profibus-DP](#11-module-plc-siemens-s7-300--trạm-thu-thập-io-turck-profibus-dp)
12. [Sơ Đồ Đấu Dây Chân Chuẩn Công Nghiệp (Pinout & Wiring Diagram)](#12-sơ-đồ-đấu-dây-chân-chuẩn-công-nghiệp)

---

## 1. HƯỚNG DẪN TRA CỨU & QUY CHUẨN ĐẶT HÀNG

Tài liệu này tổng hợp toàn bộ các linh kiện, thiết bị đo lường và cơ cấu chấp hành được sử dụng trong hệ thống máy khoan ngầm TBM **Bohrkopf 2100** (Dự án Siemens STEP 7 v5.x). 

Mỗi thiết bị được cung cấp đầy đủ:
* **Địa chỉ PLC:** Tọa độ kênh I/O trên S7-300 (`PIW`, `PQW`, `I`, `Q`, `DB`).
* **Ký hiệu kỹ thuật (Symbol Tag):** Tên định danh chuẩn theo bản vẽ P&ID và sơ đồ thủy lực.
* **Hãng sản xuất chính hãng (OEM Manufacturer):** Hydac, Trafag, Novotechnik, ASM, Seika, Krohne, Rexroth, Turck, Eaton, Siemens, Schneider...
* **Mã đặt hàng chi tiết (Exact Part Number / Order Code):** Mã sản phẩm đầy đủ để gửi cho nhà cung cấp.
* **Mã tương đương thay thế (Alternative Model):** Các hãng dự phòng đạt chuẩn tương đương khi hãng chính cháy hàng.
* **Thông số kỹ thuật & Chuẩn kết nối:** Dải đo, tín hiệu đầu ra (4–20mA / 0–10V / 24VDC), ren kết nối cơ khí (G1/4", G1/2", Flange) và chuẩn giắc điện (M12x1 4-Pin, DIN EN 175301-803 Form A).

---

## 2. CẢM BIẾN ÁP SUẤT THỦY LỰC & KHÍ NÉN

> [!NOTE]
> Tất cả các cảm biến áp suất làm việc trong hầm ẩm ướt đều sử dụng chuẩn bảo vệ tối thiểu **IP65/IP67**, chân cắm **M12x1** hoặc giắc **DIN 43650 Form A**, tín hiệu dòng **4–20mA (2 dây)** chống nhiễu trên đường truyền cáp dài.

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Ren & Giắc Nối | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PIW 406` | `JacksPresSens_PP` | **HYDAC** | `HDA 4744-A-400-000` | *Trafag* NAH 8254.83<br>*WIKA* S-20 (400 bar) | • Dải đo: **0 – 400 bar**<br>• Output: **4 – 20 mA** (2 dây)<br>• Nguồn: 9 – 36 VDC<br>• Cấp chính xác: $\le \pm 0.25\%$ FS<br>• Áp suất chịu quá tải: 800 bar | Ren: G 1/4" DIN 3852-E<br>Giắc: DIN EN 175301-803 (Form A) | Đo áp suất dầu trạm 4 xi lanh kích đẩy chính tại Container |
| 2 | `PIW 410` | `CutterPresSens_PP` | **HYDAC** | `HDA 4744-A-400-000` | *Trafag* NAH 8254.83<br>*Keller* PA-21Y (400 bar) | • Dải đo: **0 – 400 bar**<br>• Output: **4 – 20 mA**<br>• Nguồn: 9 – 36 VDC<br>• Quá tải: 800 bar | Ren: G 1/4"<br>Giắc: DIN Form A | Đo áp lực cụm bơm nguồn thủy lực mâm cắt Schürfrad |
| 3 | `PIW 378` | `PresSensor_Cutter` | **HYDAC** | `HDA 4745-A-350-000` | *Trafag* NAT 8252.82<br>*WIKA* IS-3 (Chống nổ) | • Dải đo: **0 – 350 bar**<br>• Output: **4 – 20 mA**<br>• Cấp bảo vệ: **IP67** (Đầu khiên chịu nước)<br>• Cấp chính xác: $\le \pm 0.25\%$ | Ren: G 1/4" A<br>Giắc: M12x1 4-Pin đực | Đo trực tiếp áp lực thủy lực trên đĩa mâm cắt đầu khiên |
| 4 | `PIW 364` | `PresSensor_Cyl1` | **HYDAC** | `HDA 4745-A-350-000` | *Trafag* NAH 8254.82<br>*Gefran* TK-E-1-E-B35D | • Dải đo: **0 – 350 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 700 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp lực nén xi lanh bẻ lái số 1 (Đỉnh trên $0^\circ$) |
| 5 | `PIW 366` | `PresSensor_Cyl2` | **HYDAC** | `HDA 4745-A-350-000` | *Trafag* NAH 8254.82<br>*Gefran* TK-E-1-E-B35D | • Dải đo: **0 – 350 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 700 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp lực nén xi lanh bẻ lái số 2 (Phải $120^\circ$) |
| 6 | `PIW 368` | `PresSensor_Cyl3` | **HYDAC** | `HDA 4745-A-350-000` | *Trafag* NAH 8254.82<br>*Gefran* TK-E-1-E-B35D | • Dải đo: **0 – 350 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 700 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp lực nén xi lanh bẻ lái số 3 (Trái $240^\circ$) |
| 7 | `PIW 362` | `PresSensor_SteerFeed` | **HYDAC** | `HDA 4745-A-250-000` | *Trafag* NAT 8252.81<br>*WIKA* S-20 (250 bar) | • Dải đo: **0 – 250 bar**<br>• Output: **4 – 20 mA**<br>• Cấp chính xác: $\pm 0.5\%$ | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp suất đường nguồn cấp bơm lái đầu khiên |
| 8 | `PIW 338` | `Pres_Inter1` | **HYDAC** | `HDA 4745-A-400-000` | *Trafag* NAH 8254.83<br>*Keller* PA-21Y | • Dải đo: **0 – 400 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 800 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp suất dầu thủy lực trạm kích trung gian Dehner 1 |
| 9 | `PIW 346` | `Pres_Inter2` | **HYDAC** | `HDA 4745-A-400-000` | *Trafag* NAH 8254.83<br>*Keller* PA-21Y | • Dải đo: **0 – 400 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 800 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp suất dầu thủy lực trạm kích trung gian Dehner 2 |
| 10 | `PIW 382` | `Pres_Inter3` | **HYDAC** | `HDA 4745-A-400-000` | *Trafag* NAH 8254.83<br>*Keller* PA-21Y | • Dải đo: **0 – 400 bar**<br>• Output: **4 – 20 mA**<br>• Quá tải: 800 bar | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Đo áp suất dầu thủy lực trạm kích trung gian Dehner 3 |
| 11 | `PIW 256` | `Pressure_HD-Pump` | **TRAFAG** | `NAH 8254.83.2517.04` | *Hydac* HDA 4745-A-400<br>*WIKA* HP-2 (Siêu cao áp) | • Dải đo: **0 – 400 bar** (Nước áp lực cao)<br>• Output: **4 – 20 mA**<br>• Màng cảm biến: Inox 1.4542 chống xói mòn nước | Ren: G 1/4" Female<br>Giắc: M12x1 4-Pin | Đo áp lực ngõ ra bơm nước piston cao áp xối rửa đĩa cắt |
| 12 | `PIW 374` | `SlurryChamber_Pres` | **KELLER** | `PA-21Y / 10bar / 81645` | *WIKA* S-11 (Màng phẳng)<br>*Endress+Hauser* PMC131 | • Dải đo: **0 – 10 bar** (Dung dịch bùn)<br>• Output: **4 – 20 mA**<br>• Màng Flush Diaphragm (Màng phẳng chống bùn bám đóng cục) | Ren: G 1/2" Màng phẳng<br>Giắc: M12x1 4-Pin | Đo áp lực cân bằng đất trong buồng đào bùn bentonite |
| 13 | `PIW 376` | `SlurryDischarge_Pres`| **KELLER** | `PA-21Y / 10bar / 81645` | *Trafag* NAT 8252.74<br>*WIKA* S-11 (10 bar) | • Dải đo: **0 – 10 bar**<br>• Output: **4 – 20 mA**<br>• Màng Flush chống mài mòn cát | Ren: G 1/2" Màng phẳng<br>Giắc: M12x1 4-Pin | Đo áp suất đường ống bùn thải hút ra khỏi buồng đào |
| 14 | `PIW 334` | `SlurryCharge_Pres` | **KELLER** | `PA-21Y / 10bar / 81645` | *Trafag* NAT 8252.74<br>*WIKA* S-11 (10 bar) | • Dải đo: **0 – 10 bar**<br>• Output: **4 – 20 mA** | Ren: G 1/2" Màng phẳng<br>Giắc: M12x1 4-Pin | Đo áp suất đường ống bùn nạp bentonite sạch vào buồng đào |
| 15 | `PIW 372` | `PresSens_CaseDrain` | **HYDAC** | `HDA 4745-A-010-000` | *Trafag* NAT 8252.74<br>*Keller* 21Y (10 bar) | • Dải đo: **0 – 10 bar**<br>• Output: **4 – 20 mA**<br>• Độ phân giải cao $\pm 0.1\%$ | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Cảm biến áp lực dầu rò rỉ (Lecköl) gối trục đĩa cắt |
| 16 | `PIW 294` | `PresSens_DrainNachl`| **HYDAC** | `HDA 4745-A-010-000` | *Trafag* NAT 8252.74 | • Dải đo: **0 – 10 bar**<br>• Output: **4 – 20 mA** | Ren: G 1/4"<br>Giắc: M12x1 4-Pin | Cảm biến áp lực dầu rò rỉ cụm máy kéo Nachläufer |
| 17 | `PIW 308` | `FeedingFilter_Pres` | **HYDAC** | `HDA 4344-A-010-000` | *Hydac* VD 5 D.0 /-L24<br>*Trafag* DPS 8510 | • Dải đo: **0 – 10 bar Delta-P**<br>• Output: **4 – 20 mA**<br>• Cảm biến chênh áp $\Delta P$ màng đôi | Ren: 2x G 1/4" (High/Low)<br>Giắc: DIN Form A | Giám sát độ nghẹt phin lọc dầu nạp thủy lực Container |
| 18 | `PIW 292` | `ReturnFilter_Head` | **HYDAC** | `VD 5 D.0 /-L24` | *Hydac* VR 2 D.0 /-L24 | • Tín hiệu: Tiếp điểm điện 24VDC $\Delta P > 3.0\text{ bar}$<br>• Output: Digital NO/NC | Ren: G 1/2"<br>Giắc: DIN 43650 | Công tắc báo nghẹt phin lọc đường hồi đầu khiên |

---

## 3. CẢM BIẾN HÀNH TRÌNH & ĐO DỊCH CHUYỂN TUYẾN TÍNH

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Chiều Dài / Cơ Khí | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PIW 356` | `StrokeSensor_Cyl1` | **NOVOTECHNIK** | `LWH-0200` | *Gefran* LT-M-0200-S<br>*Temposonics* MH-Series 200mm | • Hành trình đo: **0 – 200 mm**<br>• Công nghệ: Chiết áp thanh trượt dẫn điện conductive-plastic<br>• Tuyến tính: $\pm 0.05\%$ FS<br>• Output: 0 – 10VDC (hoặc 4–20mA qua bộ chuyển đổi MUP)<br>• Cấp bảo vệ: **IP65/IP67** | Thân nhôm đúc định hình<br>Đầu gắn khớp cầu tự lựa M5 | Đo chính xác hành trình thò/thụt xi lanh bẻ lái số 1 |
| 2 | `PIW 358` | `StrokeSensor_Cyl2` | **NOVOTECHNIK** | `LWH-0200` | *Gefran* LT-M-0200-S<br>*Temposonics* MH-Series 200mm | • Hành trình đo: **0 – 200 mm**<br>• Tuyến tính: $\pm 0.05\%$ FS<br>• Output: 0 – 10V / 4–20mA | Khớp cầu M5<br>Giắc: M12 4-Pin | Đo chính xác hành trình thò/thụt xi lanh bẻ lái số 2 |
| 3 | `PIW 360` | `StrokeSensor_Cyl3` | **NOVOTECHNIK** | `LWH-0200` | *Gefran* LT-M-0200-S<br>*Temposonics* MH-Series 200mm | • Hành trình đo: **0 – 200 mm**<br>• Tuyến tính: $\pm 0.05\%$ FS<br>• Output: 0 – 10V / 4–20mA | Khớp cầu M5<br>Giắc: M12 4-Pin | Đo chính xác hành trình thò/thụt xi lanh bẻ lái số 3 |
| 4 | `PIW 336` | `Way_Inter1` | **ASM SENSOR** | `WS10-1250-420A-L10-M4` | *Novotechnik* F200-1250<br>*Micro-Epsilon* WDS-1500 | • Hành trình đo: **0 – 1250 mm**<br>• Công nghệ: Dây kéo ruột thép không gỉ (Draw-wire rope encoder)<br>• Output: **4 – 20 mA** (2 dây)<br>• Cấp bảo vệ: **IP67** (Kín nước trong lòng hầm) | Cáp thép bọc Nylon $\varnothing 0.9\text{mm}$<br>Giắc: M12 4-Pin đúc | Đo độ mở rộng trạm kích trung gian Dehner 1 |
| 5 | `PIW 344` | `Way_Inter2` | **ASM SENSOR** | `WS10-1250-420A-L10-M4` | *Novotechnik* F200-1250<br>*Micro-Epsilon* WDS-1500 | • Hành trình: **0 – 1250 mm**<br>• Output: **4 – 20 mA**<br>• Cấp bảo vệ: IP67 | Cáp thép không gỉ<br>Giắc: M12 4-Pin | Đo độ mở rộng trạm kích trung gian Dehner 2 |
| 6 | `PIW 380` | `Way_Inter3` | **ASM SENSOR** | `WS10-1250-420A-L10-M4` | *Novotechnik* F200-1250<br>*Micro-Epsilon* WDS-1500 | • Hành trình: **0 – 1250 mm**<br>• Output: **4 – 20 mA**<br>• Cấp bảo vệ: IP67 | Cáp thép không gỉ<br>Giắc: M12 4-Pin | Đo độ mở rộng trạm kích trung gian Dehner 3 |

---

## 4. CẢM BIẾN ĐO GÓC NGHIÊNG, ĐỘ DỐC & XOAY KHIÊN

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Cơ Khí & Cấp Bảo Vệ | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PIW 370` | `Inclinometer_mts` | **SEIKA MIKROSYSTEME** | `NG3I / 4-20mA / +-30Deg` | *Turck* B2N60H-Q20L60<br>*Kübler* IN88 | • Dải đo: **$\pm 30.0^\circ$** (Góc xoay Roll)<br>• Output: **4 – 20 mA** (12mA = $0.0^\circ$, 4mA = $-30.0^\circ$, 20mA = $+30.0^\circ$)<br>• Độ phân giải: $< 0.001^\circ$<br>• Độ trễ nhiệt: Cực thấp, chống sốc rung 1000g | Vỏ đồng thau mạ niken đúc kín áp lực cao<br>Cấp bảo vệ: **IP68** (Chịu ngập nước 10 bar) | Đo góc xoay nghiêng quanh trục dọc thân khiên (Roll) điều khiển cánh chống xoay Wing |
| 2 | `PIW 332` | `mts_Neigung` | **SEIKA MIKROSYSTEME** | `NG3I / 4-20mA / +-30Deg` | *Turck* B2N60H-Q20L60<br>*Kübler* IN88 | • Dải đo: **$\pm 30.0^\circ$** (Góc dốc Pitch)<br>• Output: **4 – 20 mA**<br>• Đo độ nghiêng trục dọc | Vỏ đồng thau đúc kín IP68 | Đo độ dốc lên/xuống của thân khiên điều khiển lái cao độ |

---

## 5. CẢM BIẾN LƯU LƯỢNG BÙN & NƯỚC CAO ÁP

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Kích Thước & Mặt Bích | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PIW 420` | `Chargeline` | **KROHNE** | `OPTIFLUX 4300 C / DN150` | *Endress+Hauser* Promag 55S<br>*Siemens* MAG 5100W | • Dải đo: **0 – 500 m³/h**<br>• Output: **4 – 20 mA + Hart + Pulse**<br>• Lớp lót: Cao su kỹ thuật Hard Rubber / Polyurethane chống mài mòn hạt cát bentonite<br>• Điện cực: Hastelloy C4<br>• Cấp bảo vệ Sensor: **IP68** | Đường kính: **DN150** (6 Inch)<br>Mặt bích: EN 1092-1 PN16 | Lưu lượng kế điện từ đo thể tích bùn nạp sạch vào buồng đào |
| 2 | `PIW 422` | `Dischargeline` | **KROHNE** | `OPTIFLUX 4300 C / DN150` | *Endress+Hauser* Promag 55S<br>*Siemens* MAG 5100W | • Dải đo: **0 – 500 m³/h**<br>• Output: **4 – 20 mA**<br>• Lớp lót chống cát sỏi cào mòn | Mặt bích DN150 PN16 | Lưu lượng kế điện từ đo thể tích bùn thải lẫn đất đá hút ra ngoài |
| 3 | `PIW 258` | `Flow_HD-Pump` | **IFM ELECTRONIC** | `SI5000 / SID10ADBFPKG/US-100` | *E+H* DTI200<br>*Turck* FCS-G1/2A4 | • Dải đo: **0 – 100%** (Dòng chảy 3..300 cm/s)<br>• Output: **4 – 20 mA** (hoặc tiếp điểm PNP cảnh báo mất dòng chảy)<br>• Nguyên lý đo: Nhiệt động học (Calorimetric) | Ren: G 1/2" Inox 316L<br>Giắc: M12x1 4-Pin | Giám sát lưu lượng nước cấp làm mát và mồi cho bơm piston 400 bar |
| 4 | `I 16.0` | `Flow_HD-Switch` | **BEDIA CABLE** | `CLS-40 / 421575` | *IFM* SI5004 | • Tiếp điểm: **Digital NC** (Mở khi mất dòng chảy nước)<br>• Điện áp: 18 – 32 VDC | Ren: G 1/2"<br>Giắc: M12 4-Pin | Khóa liên động bảo vệ ngắt bơm cao áp khi hụt nước nguồn |

---

## 6. CẢM BIẾN NHIỆT ĐỘ, ĐỘ ẨM DẦU & MỨC DẦU THỦY LỰC

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Cơ Khí & Kết Nối | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PIW 404` | `OilTempSens_PP` | **HYDAC** | `ETS 4144-A-000` | *WIKA* TR10-C (Pt100)<br>*Trafag* TBN 8253 | • Dải đo: **-20°C đến +100°C**<br>• Output: **4 – 20 mA**<br>• Sensor phần tử: Pt1000 Class A | Ren: G 1/2" Inox<br>Giắc: DIN Form A | Đo nhiệt độ dầu thủy lực bể chứa Container trạm nguồn |
| 2 | `PIW 288` | `OilTempSens_Head`| **HYDAC** | `ETS 4545-A-000` | *WIKA* TR10-C<br>*Trafag* TBN 8253 | • Dải đo: **-20°C đến +100°C**<br>• Output: **4 – 20 mA**<br>• Cấp bảo vệ: IP67 | Ren: G 1/4"<br>Giắc: M12 4-Pin | Đo nhiệt độ dầu thủy lực trong khiên đào mâm cắt |
| 3 | `PIW 408` | `HumOil_PP` | **HYDAC** | `AS 1008-C-000` | *Vaisala* MM70 / MMT330<br>*Parker* SensoControl AquaSensor | • Dải đo: **0 – 100% Saturation** (Độ bão hòa nước trong dầu)<br>• Output: **4 – 20 mA**<br>• Áp suất làm việc: Lên tới 50 bar | Ren: G 3/8"<br>Giắc: M12x1 5-Pin | Giám sát chất lượng dầu, phát hiện rò rỉ nước ngầm vào bể dầu |
| 4 | `I 12.0` | `OilLevel_Cont_Low`| **HYDAC** | `FS-127-1.0-O-L24` | *Barksdale* UDA-3<br>*Bedia* PLS-40 | • Tín hiệu: **Digital NC** (Mở khi mức dầu hạ thấp dưới vạch đỏ)<br>• Tiếp điểm: Reed Switch 24VDC 0.5A | Ren: Bắt bích cạnh bể dầu<br>Có ống thủy quan sát | Phao báo cạn đáy dầu thủy lực trạm nguồn Container |
| 5 | `I 1.2` | `OilLevel_Cont_Mid`| **HYDAC** | `FS-127-1.0-O-L24` | *Barksdale* UDA-3 | • Tín hiệu: **Digital NO** (Đóng khi dầu đạt mức cho phép) | Bắt bích cạnh bể | Phao báo mức dầu trung bình an toàn Container |
| 6 | `I 6.1` | `OilLevel_Head_Low`| **BEDIA** | `PLS-40 / 421580` | *IFM* LMC100 | • Công nghệ: Điện dung Capacitive chống bám dầu giả<br>• Output: Digital PNP NC (24VDC) | Ren: M18x1.5 Inox<br>Giắc: M12 4-Pin | Phao điện dung báo cạn dầu gối đỡ cụm đầu khiên IP68 |

---

## 7. HỆ THỐNG ĐỊNH VỊ LASER TACS & ĐO CHIỀU DÀI HẦM

| STT | Ký hiệu / Khối | Tên Thiết Bị | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Thông Số Kỹ Thuật Chi Tiết | Giao Thức & Cấp Bảo Vệ | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `FC80 / DB19` | Bia Ngắm Quang Điện Tử Laser TACS | **TACS GmbH** (Đức) | `ELS-Target 200 / ELS-26617` | • Vùng bắt tia laser: **$\pm 100\text{ mm} \times \pm 100\text{ mm}$**<br>• Độ chính xác đo: $\pm 0.1\text{ mm}$<br>• Tích hợp cảm biến đo góc xoay Roll & Pitch $\pm 10^\circ$<br>• Bước sóng Laser: 635 – 670 nm (Laser đỏ Class 2) | Giao tiếp: **RS485 / Profibus-DP**<br>Cấp bảo vệ: **IP67** Kín nước hầm mỏ | Thu nhận tia laser từ trạm máy giếng kích, tính toán độ lệch tim hầm $\Delta X, \Delta Y$ |
| 2 | `FC35 / I 0.0` | Bánh Xe Đo Chiều Dài Hầm Đào (Längenvortrieb) | **KÜBLER** (Germany) | `Sendix 5000 / 8.5000.8354.1000` | • Loại Encoder: Tương đối Incremental Encoder<br>• Độ phân giải: **1000 Xung/vòng (PPR)**<br>• Tín hiệu ngõ ra: Push-Pull / HTL 10–30 VDC (Kênh A, B, Z)<br>• Tần số đáp ứng: 300 kHz | Trục: $\varnothing 10\text{mm}$ kèm Bánh xe lăn cao su chu vi 500mm<br>Cấp bảo vệ: **IP67** | Bánh xe lăn tì lên thành ống bê tông phát xung đếm chiều dài kích hầm |

---

## 8. VAN TỶ LỆ & VAN ĐIỆN TỪ THỦY LỰC

| STT | Địa chỉ PLC | Ký hiệu (Symbol) | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Mã Tương Đương Dự Phòng | Thông Số Kỹ Thuật Chi Tiết | Điện Áp & Lưu Lượng | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `PQW 264` | `JacksSpeed_Ampl` | **BOSCH REXROTH** | `4WRE 10 E60-2X/G24K4/V` | *Parker* D3FB01FC4NF00<br>*Moog* D633-313B | • Loại van: Van tỷ lệ 4/3 Proportional Directional Valve<br>• Tích hợp mạch điện tử OBE (On-Board Electronics)<br>• Tín hiệu điều khiển: **0 – 10 VDC** (0–27648)<br>• Áp suất max: 350 bar | Lưu lượng: **60 L/min**<br>Điện áp: 24 VDC (Giắc 6+PE) | Điều chỉnh vô cấp vận tốc hành trình kích đẩy chính |
| 2 | `PQW 266` | `JacksPres_Ampl` | **BOSCH REXROTH** | `DBETR-1X/315G24K4V` | *Parker* RE06M35W2N1KW<br>*Atos* RZMO-A-010/315 | • Loại van: Van xả áp tỷ lệ Proportional Pressure Relief Valve<br>• Dải chỉnh áp: 0 – 315/400 bar<br>• Tín hiệu: **0 – 10 VDC** | Nguồn: 24 VDC<br>Ren bích: NG6 / CETOP 3 | Điều chỉnh vô cấp áp lực nén của trạm kích chính |
| 3 | `PQW 268` | `CutterSpeed_Ampl`| **BOSCH REXROTH** | `VT-VRPA1-100-1X/` (Card) | *Rexroth* A4VG swashplate coil | • Card khuếch đại tỷ lệ điều khiển góc nghiêng đĩa bơm Rexroth A4VG<br>• Tín hiệu vào: **0 – 10 VDC** $\rightarrow$ Dòng ra cuộn hút 0–800 mA | Nguồn: 24 VDC<br>Gắn thanh ray DIN Rail | Điều tốc vô cấp vòng quay mâm cắt Schürfrad (0–6 RPM) |
| 4 | `Q 16.2 / Q 17.2` | `JacksAdv_K6.1/K7.1` | **BOSCH REXROTH** | `4WE 10 E5X/EG24N9K4` | *Yuken* DSG-03-3C2-D24<br>*Vickers* DG4V-5-2AJ-M-U-H6-20 | • Van trượt 4/3 tâm đóng kích thước Size 10 (NG10 / CETOP 5)<br>• Áp suất max: 315 bar<br>• Lưu lượng: Lên tới 120 L/min | Cuộn hút: **24 VDC / 30W**<br>Giắc cắm DIN có đèn LED | Van đóng mở mạch dầu kích tiến bên trái / phải |
| 5 | `Q 16.0 / Q 17.0` | `JacksRet_K6.2/K7.2` | **BOSCH REXROTH** | `4WE 10 E5X/EG24N9K4` | *Yuken* DSG-03-3C2-D24 | • Van trượt 4/3 Size 10 NG10 (CETOP 5) | 24 VDC có LED | Van đóng mở mạch dầu thu kích lùi bên trái / phải |
| 6 | `Q 16.1, 16.3, 17.1`| `Dehner1..3_K10..K12`| **BOSCH REXROTH** | `4WE 6 D6X/EG24N9K4` | *Yuken* DSG-01-2B2-D24<br>*Parker* D1VW001CNJW | • Van trượt 4/2 Size 6 NG6 (CETOP 3)<br>• Áp suất max: 350 bar | 24 VDC / 30W | Van đóng mở cấp dầu đẩy các trạm kích trung gian Dehner 1..3 |
| 7 | `Q 53.0 .. Q 53.5` | `SteeringValves_1..6`| **BOSCH REXROTH** | `4WE 6 E6X/EG24N9K4` | *Yuken* DSG-01-3C2-D24 | • 6 cuộn van trượt 4/3 NG6 điều khiển 3 xi lanh lái 120° | 24 VDC IP67 có LED | Van điều khiển thò/thụt 3 xi lanh bẻ khớp lái đầu khiên |
| 8 | `Q 51.4 / Q 51.5` | `ValveWing_Out/In` | **BOSCH REXROTH** | `4WE 6 D6X/EG24N9K4` | *Parker* D1VW001CNJW | • Van trượt 4/2 NG6 chịu áp 350 bar | 24 VDC IP67 | Van bung / thu cánh thép chống xoay thân khiên (Wing) |
| 9 | `Q 51.2, 51.3, 51.6`| `HD_Valves_1..4` | **COAX VALVES** | `HPB 15 / 500bar / 24VDC` | *Müller Co-ax* MK 15 NC<br>*WOMA* High Pressure Valve | • Van đóng ngắt nước cao áp Coaxial Valve 500 bar<br>• Áp suất max: **500 bar**<br>• Thời gian đóng mở: Cực nhanh $< 50\text{ ms}$ | Ren: G 1/2" Inox<br>Điện áp: 24 VDC IP68 | 4 van điện từ đóng mở 4 cụm béc phun tia nước cao áp mặt gương |

---

## 9. ĐỘNG CƠ ĐIỆN, BIẾN TẦN & BƠM PISTON CAO ÁP

| STT | Ký hiệu / Tag | Tên Thiết Bị | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Model Code) | Thông Số Kỹ Thuật Động Lực | Điện Áp & Tần Số | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `FC14 / DB25` | Động Cơ Điện Kéo Mâm Cắt (Cutterhead Motor) | **LEROY-SOMER** (France) / **SIEMENS** | `FLSD 315 M - 4P` / `1LG6 313-4AA` | • Công suất: **132 kW / 160 kW** (S1/S6)<br>• Tốc độ: 1480 RPM (4 Cực)<br>• Dòng định mức: 240 A (400V Tam giác)<br>• Cấp cách điện: Class H, IP55, tích hợp 3 cảm biến nhiệt PTC cuộn dây | 400V / 690V (50 Hz)<br>Khởi động: Biến tần / Sao-Tam giác | Động cơ điện chính kéo cụm bơm thủy lực quay mâm cắt đào đất |
| 2 | `FC13 / Q 1.0` | Động Cơ Điện Bơm Kích Chính (Main Jacks Pump Motor) | **SIEMENS** | `1LE1501-3AA23-4AA4` (Simotics GP) | • Công suất: **110 kW / 132 kW**<br>• Tốc độ: 1485 RPM (IE3 Hiệu suất cao)<br>• Dòng điện: 198 A (400V) | 400V / 50 Hz<br>Sao - Tam giác | Động cơ điện kéo cụm bơm nguồn thủy lực cho trạm kích chính |
| 3 | `FC90 / Q 48.0` | Bơm Nước Piston Cao Áp 400 bar | **SPECK TRIPLEX** (Germany) / **HAMMELMANN** | `P55/100-400` / `HDP 70` | • Áp suất làm việc: **400 bar**<br>• Lưu lượng: **100 Lít/phút**<br>• Công suất động cơ kéo: **45 kW** (1450 RPM)<br>• Piston: Gốm Ceramic đặc chủng chống mài mòn | 400V 3 Pha / 50 Hz<br>Đầu bơm Inox 1.4571 | Cụm bơm piston nén nước siêu áp xối rửa đĩa cắt |
| 4 | `FC91 / Béc 1..4`| Béc Phun Tia Nước Mặt Gương | **LECHLER** (Germany) | `065.244.30 (Flat Fan 400 bar)` | • Áp suất max: 500 bar<br>• Góc phun: $30^\circ$ hoặc $0^\circ$ Solid Stream<br>• Lõi béc: Hợp kim Carbide / Sapphire | Ren: 1/4" NPT Male | 4 đầu béc phun nước áp lực cao gọt vụn đất sét dính bết |
| 5 | `FC112 / DB112` | Biến Tần Điều Khiển Bơm Bùn (Slurry VFD) | **SCHNEIDER ELECTRIC** | `ATV71HD37N4` / `ATV630D45N4` (Altivar) | • Công suất: **37 kW / 45 kW** (Dòng 75A)<br>• Tích hợp card truyền thông Profibus-DP `VW3A3307`<br>• Bộ lọc sóng hài tích hợp DC Choke | Điện áp nguồn: 380–480V 3 Pha<br>Tần số: 0.1 – 500 Hz | Điều khiển vô cấp lưu lượng bơm cấp và bơm thải bùn bentonite |

---

## 10. KHỞI ĐỘNG ĐỘNG CƠ THÔNG MINH & APTOMAT KHỐI

| STT | Địa chỉ PLC | Ký hiệu / Tag | Hãng SX (OEM) | Mã Đặt Hàng Chính Xác (Part Number) | Thông Số Kỹ Thuật Điện Tử | Chuẩn Mạng | Chức Năng Kỹ Thuật |
|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | `DP Addr 1` | SmartWire-DT Gateway | **EATON (MOELLER)** | `EU5C-SWD-DP / 116309` | • Gateway chuyển đổi Profibus-DP sang cáp dẹt SmartWire-DT 8-Pin<br>• Tốc độ DP: Lên tới 12 Mbps<br>• Quản lý tối đa 99 thiết bị SWD | Profibus-DP (DB9 Sub-D)<br>SmartWire 8-Pin Ribbon Cable | Cầu nối truyền thông giữa CPU S7-300 và toàn bộ rơ le động cơ PKE |
| 2 | `FC110 / DB31` | Rơ Le Điện Tử Bảo Vệ Động Cơ (Motor Starter) | **EATON (MOELLER)** | `PKE32/XTU-32` + Module `PKE-SWD-32` (`126408`) | • Dải chỉnh dòng: **8.0 – 32.0 A**<br>• Đọc liên tục: Dòng tải Ampe thực tế, % nhiệt cuộn dây, trạng thái tiếp điểm Contactor | Cáp dẹt SmartWire-DT | Khởi động thông minh và đo dòng điện các động cơ bơm dầu phụ, quạt làm mát |
| 3 | `FC110 / DB31` | Rơ Le Điện Tử Động Cơ Công Suất Lớn | **EATON (MOELLER)** | `PKE65/XTU-65` + `PKE-SWD-65` | • Dải chỉnh dòng: **16.0 – 65.0 A**<br>• Cấp ngắt quá tải: Class 10, 15, 20 điều chỉnh điện tử | SmartWire-DT | Bảo vệ động cơ bơm bẻ lái và bơm cao áp |
| 4 | `FC111 / DB33` | Aptomat Khối Động Lực (MCCB) | **EATON (MOELLER)** | `NZMN2-ME220` (`265780`) + `NZM-XSWD-704` (`135850`) | • Dòng định mức: **220 A / 250 A** (Dòng cắt ngắn mạch 50 kA)<br>• Tích hợp khối truyền thông đo công suất kWh, dòng 3 pha $I_1, I_2, I_3$ | SmartWire-DT | Aptomat tổng cấp nguồn động cơ mâm cắt 132kW |

---

## 11. MODULE PLC SIEMENS S7-300 & TRẠM THU THẬP I/O TURCK PROFIBUS-DP

| STT | Vị Trí Lắp Đặt | Tên Thiết Bị | Hãng SX (OEM) | Mã Đặt Hàng Chính Hãng (Siemens / Turck) | Thông Số & Cấu Hình Module |
|:---|:---|:---|:---|:---|:---|
| 1 | Rack 0, Slot 2 | Bộ Vi Xử Lý Trung Tâm CPU | **SIEMENS** | `6ES7 315-2EH14-0AB0` (`CPU 315-2 PN/DP`) | • Bộ nhớ làm việc: **384 KB RAM**<br>• Cổng 1: **MPI / Profibus-DP Master (12 Mbps)**<br>• Cổng 2: **Profinet 2-Port Switch 100 Mbps (Ethernet)**<br>• Thời gian thực thi lệnh: 0.05 $\mu\text{s}$ |
| 2 | Rack 0, Slot 1 | Bộ Nguồn Công Nghiệp 24VDC | **SIEMENS** | `6EP1436-3BA00` (`SITOP PSU300M 24V/20A`) | • Nguồn vào: 3 Pha 400–500 VAC<br>• Nguồn ra: **24 VDC / 20A** (Công suất 480W, hiệu suất 93%) |
| 3 | CPU Slot | Thẻ Nhớ Vi Chương Trình MMC | **SIEMENS** | `6ES7 953-8LJ30-0AA0` (`Micro Memory Card 512KB`) | • Thẻ nhớ lưu trữ toàn bộ code STEP 7, DBs và cấu hình phần cứng |
| 4 | Container (DP 7) | Trạm I/O Phân Tán IP20 Tủ Điện | **TURCK** | `BL20-GW-DPV1` (`Order: 6827234`) | • Gateway trạm Profibus-DP Slave chuẩn IP20<br>• Ghép nối các module: `BL20-4AI-U/I`, `BL20-16DI`, `BL20-16DO` |
| 5 | Container Module | Module 4 Kênh Analog Input IP20 | **TURCK** | `BL20-4AI-U/I` (`Order: 6827017`) | • 4 kênh Analog đa năng (0–20mA, 4–20mA, 0–10V, $\pm 10\text{V}$), độ phân giải 16-Bit |
| 6 | Khiên Đào (DP 5) | Trạm I/O Đúc Kín Chịu Nước IP67 | **TURCK** | `BL67-GW-DPV1` (`Order: 6827214`) | • Gateway trạm Profibus-DP Slave chuẩn **IP67** (Chống bụi nước hoàn toàn)<br>• Hoạt động bền bỉ trong môi trường bùn đất đầu khiên |
| 7 | Khiên Đào Module | Module 4 Kênh Analog Input IP67 | **TURCK** | `BL67-4AI-U/I` (`Order: 6827176`) | • 4 kênh Analog Input 16-Bit, giắc nối M12x1 5-Pin đúc kín |
| 8 | Khiên Đào Khối | Hộp I/O Analog Đúc Khối Độc Lập | **TURCK** | `SDPB-40A-0005` (`Order: 6824049`) | • Khối I/O Profibus đúc nguyên khối Ultra-compact IP67<br>• 4 ngõ vào Analog 4–20mA kết nối trực tiếp cảm biến xi lanh lái |
| 9 | Khiên Đào Khối | Hộp I/O Digital Đúc Khối Độc Lập | **TURCK** | `SDPB-0404D-0001` / `SDPB-0808D-0001` | • Khối I/O 8 kênh Digital (4DI/4DO hoặc 8DI/8DO) chuẩn M8/M12 IP67 |

---

## 12. SƠ ĐỒ ĐẤU DÂY CHÂN CHUẨN CÔNG NGHIỆP (PINOUT & WIRING DIAGRAM)

### 12.1. Cảm Biến Áp Suất & Độ Nghiêng Giắc M12 4-Pin (4–20mA / 2-Wire Loop)
```
       Giắc M12 4-Pin Đực (Sensor Male)            Đấu Dây Vào PLC / Module Turck
                 [ 2 ]                               Pin 1 (+24VDC) ----> Nguồn +24VDC
               /       \                             Pin 2 (Signal) ----> Chân AI+ (Tín hiệu dòng 4-20mA)
            [ 1 ]     [ 3 ]                          Pin 3 (GND/0V) ----> Chân AI- (Hoặc Nối chung 0V)
               \       /                             Pin 4 (PE/Shield)-> Nối giáp chống nhiễu
                 [ 4 ]
```

### 12.2. Cảm Biến Chiết Áp Hành Trình Novotechnik LWH (0–10VDC)
```
          Đầu Giắc Sensor LWH                         Đấu Dây Vào Module BL67/SDPB
          Chân 1 (Supply +)   -----------------------> Nguồn +10VDC / +24VDC
          Chân 2 (Wiper Out)  -----------------------> Ngõ vào Analog Voltage (0-10V)
          Chân 3 (GND -)      -----------------------> Chân 0V (GND)
          Chân 4 (Vỏ Shield)  -----------------------> Giáp cáp chống nhiễu
```

---
*Tài liệu BOM được biên soạn chi tiết và chuẩn hóa 100% từ hồ sơ phần cứng & danh bạ linh kiện gốc dự án TBM Bohrkopf 2100.*
