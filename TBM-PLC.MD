# TÀI LIỆU CHI TIẾT TOÀN BỘ CHƯƠNG TRÌNH PLC S7-300 - MÁY KHOAN NGẦM TBM (BOHRKOPF 2100)
## NGÔN NGỮ LẬP TRÌNH BẬC THANG (LADDER DIAGRAM - LAD / KOP)

---

## MỤC LỤC TỔNG QUAN

1. [Tổng Quan Kiến Trúc Hệ Thống & Cấu Hình Phần Cứng](#1-tổng-quan-kiến-trúc-hệ-thống--cấu-hình-phần-cứng)
2. [Bảng Ký Hiệu (Symbol Table) & Địa Chỉ I/O Toàn Cục](#2-bảng-ký-hiệu-symbol-table--địa-chỉ-io-toàn-cục)
3. [Phần 1: Khởi Tạo & Chẩn Đoán Mạng Profibus-DP (OB100, OB86, FC125, FB125)](#phần-1-khởi-tạo--chẩn-đoán-mạng-profibus-dp)
4. [Phần 2: Mạch Liên Động An Toàn & Dừng Khẩn Cấp (FC61 Freigaben, FC71/FC72)](#phần-2-mạch-liên-động-an-toàn--dừng-khẩn-cấp)
5. [Phần 3: Điều Khiển Trạm Nguồn Thủy Lực & Động Cơ Chính 132kW (FC13, FC30, FC33)](#phần-3-điều-khiển-trạm-nguồn-thủy-lực--động-cơ-chính-132kw)
6. [Phần 4: Điều Khiển Đảo Chiều & Tốc Độ Đầu Cắt Schürfrad (FC14, FC25, FC26, FC4/FC5)](#phần-4-điều-khiển-đảo-chiều--tốc-độ-đầu-cắt-schürfrad)
7. [Phần 5: Điều Khiển Trạm Kích Đẩy Chính & Quy Đổi Lực Tấn (FC20, FC21, FC22, DB17)](#phần-5-điều-khiển-trạm-kích-đẩy-chính--quy-đổi-lực-tấn)
8. [Phần 6: Điều Khiển Đồng Bộ Các Trạm Kích Trung Gian Dehner 1..5 (FC55..57, FC141)](#phần-6-điều-khiển-đồng-bộ-các-trạm-kích-trung-gian-dehner-15)
9. [Phần 7: Hệ Thống Bẻ Lái 3 Xi Lanh 120°, Laser TACS & Cánh Chống Xoay (FC40..43, FC80..82)](#phần-7-hệ-thống-bẻ-lái-3-xi-lanh-120-laser-tacs--cánh-chống-xoay)
10. [Phần 8: Hệ Thống Tuần Hoàn Bùn Bentonite & Biến Tần Schneider Altivar (FC10..12, FC112)](#phần-8-hệ-thống-tuần-hoàn-bùn-bentonite--biến-tần-schneider-altivar)
11. [Phần 9: Bơm Piston Nước Cao Áp 400 bar & Cụm Béc Phun Mặt Gương (FC90, FC91)](#phần-9-bơm-piston-nước-cao-áp-400-bar--cụm-béc-phun-mặt-gương)
12. [Phần 10: Đo Chiều Dài Hầm Đào & Bộ Lọc Vận Tốc Ngắt Chu Kỳ (FC35, FB100, OB32/33)](#phần-10-đo-chiều-dài-hầm-đào--bộ-lọc-vận-tốc-ngắt-chu-kỳ)
13. [Phần 11: Mạng Truyền Thông SmartWire-DT & Bảo Vệ Động Cơ Eaton PKE/NZM (FC110, FC111)](#phần-11-mạng-truyền-thông-smartwire-dt--bảo-vệ-động-cơ-eaton-pkenzm)
14. [Phần 12: Phanh Kẹp Cống & Khóa Cơ Khí Bàn Đẩy (FC18, FC19)](#phần-12-phanh-kẹp-cống--khóa-cơ-khí-bàn-đẩy)
15. [Phần 13: Hệ Thống Làm Mát, Quạt Két Dầu & Cảnh Báo Rò Rỉ Lecköl (FC15, FC34)](#phần-13-hệ-thống-làm-mát-quạt-két-dầu--cảnh-báo-rò-rỉ-lecköl)
16. [Phần 14: Đèn Pha Mặt Gương & Điều Khiển Camera Đầu Khiên (FC95)](#phần-14-đèn-pha-mặt-gương--điều-khiển-camera-đầu-khiên)
17. [Phần 15: Các Khối Ngắt Chống Sập CPU (OB82, OB121, OB122)](#phần-15-các-khối-ngắt-chống-sập-cpu)
18. [Phần 16: Giao Tiếp HMI Màn Hình Cảm Ứng & Nhật Ký SCADA (DB19, DB57)](#phần-16-giao-tiếp-hmi-màn-hình-cảm-ứng--nhật-ký-scada)
19. [Bảng Tổng Kết Toàn Bộ Khối Lệnh (OB, FC, FB, DB)](#bảng-tổng-kết-toàn-bộ-khối-lệnh)

---

## 1. TỔNG QUAN KIẾN TRÚC HỆ THỐNG & CẤU HÌNH PHẦN CỨNG

* **Thiết bị:** Máy khoan kích ngầm Microtunneling Pipe Jacking (Mã hiệu Bohrkopf 2100 - mts Perforator).
* **Bộ điều khiển trung tâm:** Siemens SIMATIC S7-300 **CPU 315-2 PN/DP** (Order: `6ES7 315-2EH14-0AB0`).
* **Mạng truyền thông trường:**
  * **Profibus-DP Master (12 Mbps):** Quản lý 12 trạm Slaves gồm Turck BL20 (Tủ Container), Turck BL67 (Đầu khiên chịu nước IP67), các khối I/O đúc kín Turck SDPB-40A (Analog) và SDPB-0404D/0808D (Digital).
  * **SmartWire-DT Gateway (Eaton EU5C-SWD-DP):** Kết nối mạng cáp dẹt điều khiển rơ le điện tử Eaton PKE và aptomat khối NZM.
  * **Profinet PN:** Giao tiếp HMI Touchscreen và hệ thống máy tính SCADA.

```
       +-------------------------------------------------------------+
       |           SIEMENS S7-300 CPU 315-2 PN/DP (Rack 0, Slot 2)   |
       +-------------------------------------------------------------+
               | (Profibus-DP 12Mbps)                 | (Profinet / MPI)
   +-----------+-----------+-----------+              +---------+---------+
   |                       |           |              |                   |
[DP Addr 1/6]         [DP Addr 7/16] [DP Addr 5,8,15] [HMI Touchscreen] [SCADA Server]
Eaton SWD-Gateway    Turck BL20     Turck BL67/SDPB      (DB19)            (DB57)
(PKE Motor Starters) (Container IO) (Head IO IP67)
```

---

## 2. BẢNG KÝ HIỆU (SYMBOL TABLE) & ĐỊA CHỈ I/O TOÀN CỤC

### 2.1. Cảm biến Đo Lường Tương Tự (Analog Inputs - PIW)
| Địa chỉ PLC | Ký hiệu (Symbol) | Dải đo vật lý | Vị trí lắp đặt | Chức năng kỹ thuật |
|:---|:---|:---|:---|:---|
| `PIW 406` | `JacksPresSens_PP` | 0 – 400 bar | Container | Áp suất kích đẩy chính |
| `PIW 410` | `CutterPresSens_PP` | 0 – 400 bar | Container | Áp suất bơm nguồn đầu cắt |
| `PIW 378` | `PresSensor_Cutter` | 0 – 350 bar | Đầu khiên | Áp suất đĩa cắt Schürfscheibe |
| `PIW 364` | `PresSensor_Cyl1` | 0 – 350 bar | Đầu khiên | Áp suất xi lanh bẻ lái số 1 |
| `PIW 366` | `PresSensor_Cyl2` | 0 – 350 bar | Đầu khiên | Áp suất xi lanh bẻ lái số 2 |
| `PIW 368` | `PresSensor_Cyl3` | 0 – 350 bar | Đầu khiên | Áp suất xi lanh bẻ lái số 3 |
| `PIW 356` | `StrokeSensor_Cyl1` | 0 – 200 mm | Đầu khiên | Hành trình thò/thụt xi lanh lái 1 |
| `PIW 358` | `StrokeSensor_Cyl2` | 0 – 200 mm | Đầu khiên | Hành trình thò/thụt xi lanh lái 2 |
| `PIW 360` | `StrokeSensor_Cyl3` | 0 – 200 mm | Đầu khiên | Hành trình thò/thụt xi lanh lái 3 |
| `PIW 370` | `Inclinometer_mts` | ±30.0° | Khung khiên | Góc xoay lật (Roll) thân máy |
| `PIW 332` | `mts_Neigung` | ±30.0° | Khung khiên | Góc dốc trục dọc (Pitch) |
| `PIW 336` | `Way_Inter1` | 0 – 1200 mm | Dehner 1 | Hành trình trạm kích trung gian 1 |
| `PIW 338` | `Pres_Inter1` | 0 – 400 bar | Dehner 1 | Áp suất trạm kích trung gian 1 |
| `PIW 344` | `Way_Inter2` | 0 – 1200 mm | Dehner 2 | Hành trình trạm kích trung gian 2 |
| `PIW 346` | `Pres_Inter2` | 0 – 400 bar | Dehner 2 | Áp suất trạm kích trung gian 2 |
| `PIW 380` | `Way_Inter3` | 0 – 1200 mm | Dehner 3 | Hành trình trạm kích trung gian 3 |
| `PIW 382` | `Pres_Inter3` | 0 – 400 bar | Dehner 3 | Áp suất trạm kích trung gian 3 |
| `PIW 256` | `Pressure_HD-Pump` | 0 – 400 bar | Đầu khiên | Áp suất bơm nước cao áp |
| `PIW 258` | `Flow_HD-Pump` | 0 – 100% | Đầu khiên | Cảm biến lưu lượng bảo vệ bơm |
| `PIW 374` | `PresSensor_SlurryChamber` | 0 – 10 bar | Mặt gương | Áp lực buồng đào bùn bentonite |
| `PIW 376` | `PresSensor_SlurryLine` | 0 – 10 bar | Ống thải | Áp suất đường ống bùn xả |
| `PIW 334` | `PresSensor_ChargeLine` | 0 – 10 bar | Ống cấp | Áp suất đường ống cấp bùn |
| `PIW 420` | `Chargeline` | 0 – 500 m³/h | Container | Lưu lượng kế đường bùn nạp |
| `PIW 422` | `Dischargeline` | 0 – 500 m³/h | Container | Lưu lượng kế đường bùn xả |
| `PIW 404` | `OilTempSens_PP` | -20..+100°C | Bể Container | Nhiệt độ dầu thủy lực trạm nguồn |
| `PIW 408` | `HumOil_PP` | 0 – 100% | Bể Container | Độ ẩm / Nước lẫn trong dầu |
| `PIW 372` | `PresSens_CaseDrain` | 0 – 10 bar | Gối trục cắt | Cảm biến áp lực dầu rò rỉ (Lecköl) |
| `PIW 296` | `Flow_Cool_Motor` | 0 – 100 l/min | Đầu khiên | Lưu lượng làm mát motor điện |
| `PIW 298` | `Flow_Cool_Oil` | 0 – 100 l/min | Đầu khiên | Lưu lượng làm mát két dầu |
| `PIW 308` | `FeedingFilter` | 0 – 10 bar | Container | Độ chênh áp phin lọc dầu đường nạp |

### 2.2. Ngõ Ra Tương Tự Điều Khiển Van Tỷ Lệ (Analog Outputs - PQW)
| Địa chỉ PLC | Ký hiệu (Symbol) | Dải điện áp | Card khuếch đại | Chức năng kỹ thuật |
|:---|:---|:---|:---|:---|
| `PQW 264` | `JacksSpeed_Ampl` | 0 – 10V (0–27648) | Amplifier Card | Điều chỉnh vận tốc kích đẩy chính |
| `PQW 266` | `JacksPressure_Ampl`| 0 – 10V (0–27648) | Amplifier Card | Điều chỉnh áp suất kích đẩy chính |
| `PQW 268` | `CutterSpeed_Ampl` | 0 – 10V (0–27648) | Rexroth A4VG / CSG | Điều chỉnh góc nghiêng đĩa bơm biến tích |

---

## PHẦN 1: KHỞI TẠO & CHẨN ĐOÁN MẠNG PROFIBUS-DP

### Network 1.1: OB100 - Khởi Tạo Biến, Reset Cờ Truyền Thông & Cài Đặt Ban Đầu
```text
  Power Rail                                                                              Power Rail
|---| First_Scan |-----------------------------------------------------------------------[ CALL ]---|
|   [   OS100    ]                                                                       | FC100    |
|                                                                                        | (Init)   |
|---| First_Scan |-----------------------------------------------------------------------[ MOVE ]---|
|   [   OS100    ]                                                                       | IN:=0    |
|                                                                                        | OUT:=DB58|
|                                                                                        | .DBD0    |
|---| First_Scan |-----------------------------------------------------------------------( S )------|
|   [   OS100    ]                                                                         M 100.0  |
|                                                                                        (Sys_Init) |
```

### Network 1.2: OB86 / FC125 / FB125 - Quét & Chẩn Đoán 12 Trạm Profibus-DP Slaves
```text
|---| Sys_Init |---| DP_Rack_Fault |-----------------------------------------------------[ CALL ]---|
|   [ M 100.0  ]   [ / ] (OB86)     |                                                     | FC125    |
|                                   |                                                     | DP_DIAG  |
|                                   |                                                     | DB100    |
|                                   |                                                     +----------+
|                                   +---| All_Slaves_OK |--------------------------------(   )------|
|                                       [   M 125.0     ]                                  M 76.0   |
|                                                                                        (DP_Bus_OK)|
```
* **Thuyết minh:** `FC125` đọc thanh ghi chẩn đoán `SFC51 (RDSYSST)` kiểm tra 12 trạm DP (Turck BL20, BL67, SDPB). Nếu tất cả các trạm phản hồi tốt, cờ `M 76.0 (DP_Bus_OK)` bật ON.

---

## PHẦN 2: MẠCH LIÊN ĐỘNG AN TOÀN & DỪNG KHẨN CẤP

### Network 2.1: FC61 - Mạch Liên Động Tổng Cho Phép Vận Hành (Freigaben)
```text
|---| E-Stop_OK |---| OilLevel_OK |---[/] OilTemp_High |---[/] Filter_Clog |---| DP_Bus_OK |---| Mot_Safe |---(   )---|
|   [   I 0.0   ]   [   I 1.2     ]   [ / ]  M 10.4    ]   [ / ]  M 33.1   ]   [  M 76.0   ]   [  M 13.0  ]   ( M61.0 )|
|                                                                                                          (Freigabe)|
```
* **Giải thích tiếp điểm:**
  * `I 0.0`: Nút dừng khẩn cấp (E-Stop) ở trạng thái đóng kín mạch an toàn.
  * `I 1.2`: Phao báo mức dầu thủy lực Container & Đầu khiên đạt mức cho phép.
  * `M 10.4`: Cờ cảnh báo quá nhiệt dầu thủy lực > 70°C (ngắt mạch khi nhiệt cao).
  * `M 33.1`: Cờ báo nghẹt lọc dầu đường nạp/hồi (ngắt khi độ chênh áp cao).
  * `M 76.0`: Xác nhận mạng truyền thông Profibus-DP hoạt động ổn định.
  * `M 13.0`: Xác nhận rơ le nhiệt các động cơ điện PKE không bị nhảy trip.
  * `M 61.0`: Cuộn hút cấp quyền chạy cho toàn bộ hệ thống chấp hành.

### Network 2.2: FC71 / FC72 - Quản Lý & Xóa Cờ Báo Lỗi Hệ Thống
```text
|---| Fault_Reset_PB |---| Freigabe_OK |-------------------------------------------------( R )------|
|   [     I 60.0     ]   [   M 61.0    ]                                                   DB58.DBX0.0
|                                                                                         (Gen_Fault)|
|---| Gen_Fault |------------------------------------------------------------------------(   )------|
|   [DB58.DBX0.0]                                                                          Q 75.0   |
|                                                                                        (Horn/Alarm)|
```

---

## PHẦN 3: ĐIỀU KHIỂN TRẠM NGUỒN THỦY LỰC & ĐỘNG CƠ CHÍNH 132kW

### Network 3.1: FC13 & FC30 - Khởi Động Động Cơ Bơm Thủy Lực Chính 110kW/132kW
```text
|---| Freigabe_OK |--+--| Start_PP_PB |--+---| Stop_PP_PB |---| Mot_Fault |-----------------(   )------|
|   [   M 61.0    ]  |  [   I 61.1    ]  |   [ / ] I 61.0 ]   [ / ] I 1.1  ]                 ( Q 1.0  )  |
|                    |  | Motor_Hold   |  |                                                (MainMot_K) |
|                    +--|   Q 1.0      |--+                                                            |
|                       [              ]                                                               |
```

### Network 3.2: FC33 - Giám Sát Áp Suất & Tắc Lọc Đường Dầu Nạp (Feeding Circuit)
```text
|---| FeedingFilter |--------------------------------------------------------------------[ CMP > ]---|
|   [   PIW 308     ]                                                                    | IN1:=PIW308|
|                                                                                        | IN2:=12441 |
|                                                                                        | (4.5 bar)  |
|                                                                                        +-----+------+
|                                                                                              |
|----------------------------------------------------------------------------------------------( S )---|
|                                                                                                M 33.1 |
|                                                                                          (Filter_Clog)|
```

---

## PHẦN 4: ĐIỀU KHIỂN ĐẢO CHIỀU & TỐC ĐỘ ĐẦU CẮT SCHÜRFRAD

### Network 4.1: FC25 & FC14 - Điều Khiển Chiều Quay Thuận (CW) / Nghịch (CCW)
```text
--[ Rung 1: Quay Phải (CW) ]---------------------------------------------------------------------------
|---| Freigabe_OK |---| Cutter_CW_Sw |--+--| Interlock_CCW |--------------------------------(   )------|
|   [   M 61.0    ]   [    I 90.1    ]  |  [ / ]  M 20.2   ]                               ( M 20.1 )  |
|                                       |  |  Cutter_CW_M  |                               (Cutter_CW) |
|                                       +--|    M 20.1     |                                           |
|                                          [               ]                                           |

--[ Rung 2: Quay Trái (CCW) ]--------------------------------------------------------------------------
|---| Freigabe_OK |---| Cutter_CCW_Sw |-+--| Interlock_CW  |--------------------------------(   )------|
|   [   M 61.0    ]   [    I 90.2     ] |  [ / ]  M 20.1   ]                               ( M 20.2 )  |
|                                       |  |  Cutter_CCW_M |                               (Cutter_CCW)|
|                                       +--|    M 20.2     |                                           |
|                                          [               ]                                           |
```

### Network 4.2: FC26 & FC107 - Chuẩn Hóa Tốc Độ Đặt HMI Xuất Điện Áp Van Tỷ Lệ Đĩa Bơm (PQW 268)
```text
|---| Freigabe_OK |---| Cutter_Run |--+--------------------------------------------------[ SCALE ]---|
|   [   M 61.0    ]   [   M 20.0   ]  |                                                  | FC105    |
|                     (M20.1 v M20.2) |                                                  | IN:=DB21 |
|                                     |                                                  | .DBD0    |
|                                     |                                                  | HI:=100.0|
|                                     |                                                  | LO:=0.0  |
|                                     |                                                  | OUT:=    |
|                                     |                                                  | MW 268   |
|                                     |                                                  +----+-----+
|                                     |                                                       |
|                                     +--------------------------------------------------[ MOVE ]---|
|                                                                                        | IN:=MW268|
|                                                                                        | OUT:=    |
|                                                                                        | PQW 268  |
|                                                                                        +----------+
```

### Network 4.3: FC5 / FC140 - Giới Hạn Áp Suất Cắt & Tự Động Giảm Tốc Khi Quá Tải Mô-men
```text
|---| Cutter_Pres |----------------------------------------------------------------------[ CMP > ]---|
|   [   PIW 378   ]                                                                      | IN1:=PIW378|
|                                                                                        | IN2:=23700 |
|                                                                                        | (300 bar)  |
|                                                                                        +-----+------+
|                                                                                              |
|----------------------------------------------------------------------------------------[ SUB ]-----|
|                                                                                        | IN1:=MW268 |
|                                                                                        | IN2:=8000  |
|                                                                                        | OUT:=MW268 |
```

---

## PHẦN 5: ĐIỀU KHIỂN TRẠM KÍCH ĐẨY CHÍNH & QUY ĐỔI LỰC TẤN

### Network 5.1: FC20 - Mạch Điều Khiển Tiến / Lùi Kích Đẩy Chính (Main Jacks)
```text
--[ Rung 1: Kích Tiến (Advance) ]----------------------------------------------------------------------
|---| Freigabe_OK |---| Cutter_Running |---| Cyl_Unlocked |---| Jacks_Adv_PB |---[/] Jacks_Ret |--(   )|
|   [   M 61.0    ]   [    M 20.0      ]   [   M 12.7     ]   [    I 62.2    ]   [ / ] Q 16.0  ]  ( Q16.2 )
|                                                                                             (JacksAdvL)
|                                                                                                 +----( Q17.2 )
|                                                                                                     (JacksAdvR)

--[ Rung 2: Kích Lùi (Retract) ]-----------------------------------------------------------------------
|---| Freigabe_OK |---| Cyl_Unlocked |---| Jacks_Ret_PB |---[/] Jacks_Adv |-----------------------(   )|
|   [   M 61.0    ]   [   M 12.7     ]   [    I 62.3    ]   [ / ] Q 16.2  ]                       ( Q16.0 )
|                                                                                             (JacksRetL)
|                                                                                                 +----( Q17.0 )
|                                                                                                     (JacksRetR)
```

### Network 5.2: FC22 & DB17 - Nội Suy Quy Đổi Áp Suất (Bar) Sang Tổng Lực Kích (Tấn)
```text
|---| Jacks_Active |--+------------------------------------------------------------------[ SCALE ]---|
|   [    Q 16.2    ]  |                                                                  | FC105    |
|                     |                                                                  | IN:=PIW  |
|                     |                                                                  | 406      |
|                     |                                                                  | HI:=400.0|
|                     |                                                                  | LO:=0.0  |
|                     |                                                                  | OUT:=    |
|                     |                                                                  | DB16.DBD4|
|                     |                                                                  | (Bar)    |
|                     |                                                                  +----+-----+
|                     |                                                                       |
|                     +------------------------------------------------------------------[ MUL_R ]---|
|                                                                                        | IN1:=    |
|                                                                                        | DB16.DBD4|
|                                                                                        | IN2:=    |
|                                                                                        | 0.24543  |
|                                                                                        | (Area C.)|
|                                                                                        | OUT:=    |
|                                                                                        | DB17.DBD0|
|                                                                                        | (Tấn)    |
```

---

## PHẦN 6: ĐIỀU KHIỂN ĐỒNG BỘ CÁC TRẠM KÍCH TRUNG GIAN DEHNER 1..5

### Network 6.1: FC55 / FC56 / FC57 - Điều Khiển Đóng/Mở Trạm Kích Trung Gian 1, 2, 3
```text
--[ Điều khiển Dehner 1 (-K10) ]-----------------------------------------------------------------------
|---| Freigabe_OK |---| Auto_Mode |---| Dehner1_Req |---| Stroke_Safe |--------------------------(   )-|
|   [   M 61.0    ]   [  M 10.0   ]   [   M 55.0    ]   [   M 55.4    ]                          ( Q16.1 )
|                                                       (PIW336 < 1100mm)                       (Dehner1_K10)

--[ Điều khiển Dehner 2 (-K11) ]-----------------------------------------------------------------------
|---| Freigabe_OK |---| Auto_Mode |---| Dehner2_Req |---| Stroke_Safe |--------------------------(   )-|
|   [   M 61.0    ]   [  M 10.0   ]   [   M 56.0    ]   [   M 56.4    ]                          ( Q16.3 )
|                                                       (PIW344 < 1100mm)                       (Dehner2_K11)

--[ Điều khiển Dehner 3 (-K12) ]-----------------------------------------------------------------------
|---| Freigabe_OK |---| Auto_Mode |---| Dehner3_Req |---| Stroke_Safe |--------------------------(   )-|
|   [   M 61.0    ]   [  M 10.0   ]   [   M 57.0    ]   [   M 57.4    ]                          ( Q17.1 )
|                                                       (PIW380 < 1100mm)                       (Dehner3_K12)
```

---

## PHẦN 7: HỆ THỐNG BẺ LÁI 3 XI LANH 120°, LASER TACS & CÁNH CHỐNG XOAY

### Network 7.1: FC40 / FC41 / FC42 - Điều Khiển 6 Cuộn Van Thủy Lực Thò / Thụt 3 Xi Lanh Lái
```text
--[ Xi lanh Lái 1 Thò (Cyl 1 Extend) ]------------------------------------------------------------------
|---| Freigabe_OK |---| Steer_Pump_ON |---| Steer_Cyl1_Out_CMD |---[/] Cyl1_Max_Stroke |---------(   )-|
|   [   M 61.0    ]   [    Q 5.0      ]   [     DB19.DBX10.0   ]   [ / ]  M 40.5         ]         ( Q53.0 )
|                                                                 (PIW356 > 190mm)             (ValveCyl1_Out)

--[ Xi lanh Lái 1 Thụt (Cyl 1 Retract) ]----------------------------------------------------------------
|---| Freigabe_OK |---| Steer_Pump_ON |---| Steer_Cyl1_In_CMD |--[/] Cyl1_Min_Stroke |----------(   )-|
|   [   M 61.0    ]   [    Q 5.0      ]   [     DB19.DBX10.1   ]  [ / ]  M 40.6        ]          ( Q53.1 )
|                                                                (PIW356 < 10mm)               (ValveCyl1_In)
```

### Network 7.2: FC81 & FC82 - Đọc Cảm Biến Nghiêng (Inclinometer mts) & Bù Xoắn Trục Thân Khiên
```text
|---| Freigabe_OK |----------------------------------------------------------------------[ SCALE ]---|
|   [   M 61.0    ]                                                                      | FC105    |
|                                                                                        | IN:=PIW  |
|                                                                                        | 370      |
|                                                                                        | HI:=30.0 |
|                                                                                        | LO:=-30.0|
|                                                                                        | OUT:=    |
|                                                                                        | DB59.DBD0|
|                                                                                        | (Roll_Deg|
|                                                                                        +----+-----+
|                                                                                             |
|---| Roll_Deg > +1.5° |----------------------------------------------------------------( S )------|
|   [ DB59.DBD0 > 1.5  ]                                                                  M 17.5   |
|                                                                                        (Roll_Alarm)|
```

### Network 7.3: FC43 - Tự Động Bung Cánh Thép Chống Xoay Khiên Đào (Wing / Flossenzylinder)
```text
|---| Freigabe_OK |--+--| Wing_Manual_Sw |--+--------------------------------------------(   )------|
|   [   M 61.0    ]  |  [     I 63.1     ]  |                                            ( Q 51.4 ) |
|                    |  | Roll_Alarm_M   |  |                                            (ValveWing |
|                    +--|     M 17.5     |--+                                             _Out)     |
|                       [                ]                                                          |
```

---

## PHẦN 8: HỆ THỐNG TUẦN HOÀN BÙN BENTONITE & BIẾN TẦN SCHNEIDER ALTIVAR

### Network 8.1: FC10 / FC11 - Điều Khiển Bơm Cấp (Charge) & Bơm Thải Bùn (Discharge)
```text
--[ Bơm cấp bùn bentonite (Charge Pump) ]---------------------------------------------------------------
|---| Freigabe_OK |--+--| Start_Charge_PB |--+---| Stop_Charge_PB |---| ChamberPres_Safe |-(   )------|
|   [   M 61.0    ]  |  [    I 50.0       ]  |   [ / ]  I 50.1    ]   [ / ]  M 10.6      ] ( M 10.1 ) |
|                    |  | Charge_Run_M    |  |                        (PIW374 > 4.5 bar)   (Charge_Run)|
|                    +--|    M 10.1       |--+                                             +---( Q 73.0 )
|                       [                 ]                                                    (Charge_H)

--[ Bơm hút thải bùn (Discharge Pump) ]----------------------------------------------------------------
|---| Freigabe_OK |--+--| Start_Disch_PB  |--+---| Stop_Disch_PB  |---| LinePres_Safe |--(   )---------|
|   [   M 61.0    ]  |  [    I 50.2       ]  |   [ / ]  I 50.3    ]   [ / ]  M 11.6     ]  ( M 11.1 )  |
|                    |  | Disch_Run_M     |  |                        (PIW376 > 6.0 bar)  (Disch_Run) |
|                    +--|    M 11.1       |--+                                            +----( Q 76.0 )
|                       [                 ]                                                    (Disch_H)
```

### Network 8.2: FC112 - Giao Tiếp Đặt Tần Số & Tốc Độ Biến Tần Schneider Altivar qua Profibus
```text
|---| Charge_Run |-----------------------------------------------------------------------[ DP_WR_DAT|
|   [   M 10.1   ]                                                                       | LADDR:=  |
|                                                                                        | W#16#100 |
|                                                                                        | RECORD:= |
|                                                                                        | DB112    |
|                                                                                        | .DBB0..3 |
|                                                                                        | (CMD+SPD)|
```

---

## PHẦN 9: BƠM PISTON NƯỚC CAO ÁP 400 BAR & CỤM BÉC PHUN MẶT GƯƠNG

### Network 9.1: FC90 - Khởi Động Động Cơ Bơm Piston Cao Áp 400 bar (Sao - Tam Giác)
```text
|---| Freigabe_OK |--+--| HD_Pump_Sw_PB |--+---| WaterFlow_OK |---| Motor_Safe |---------(   )------|
|   [   M 61.0    ]  |  [    M 55.3     ]  |   [ / ] I 16.0   ]   [ / ] I 16.1 ]         ( Q 48.0 ) |
|                    |  | HD_Pump_Run_M |  |  (Flow_Low_Fault)    (Thermal_Trip)         (HD_Pump_ON)|
|                    +--|    M 55.1     |--+                                                        |
|                       [               ]                                                           |
```

### Network 9.2: FC91 - Điều Khiển Mở 4 Cụm Béc Phun Tia Nước Cao Áp Mặt Gương (HD_Valve 1..4)
```text
|---| HD_Pump_ON |---| HighPres_Achieved |---| Jet_Cmd_PB |--+---------------------------(   )------|
|   [   Q 48.0   ]   [      M 90.5       ]   [   I 52.2   ]  |                           ( Q 51.2 ) |
|                    (PIW256 > 250 bar)                      |                           (HD_Valve1)|
|                                                            +---------------------------( Q 51.3 ) |
|                                                            |                           (HD_Valve2)|
|                                                            +---------------------------( Q 51.6 ) |
|                                                            |                           (HD_Valve3)|
|                                                            +---------------------------( Q 51.7 ) |
|                                                                                        (HD_Valve4)|
```

---

## PHẦN 10: ĐO CHIỀU DÀI HẦM ĐÀO & BỘ LỌC VẬN TỐC NGẮT CHU KỲ

### Network 10.1: FC35 - Đếm Xung Bánh Xe Đo Chiều Dài Kích Đẩy Hầm (Längenvortrieb)
```text
|---| Pulse_Dist_Wheel |-----------------------------------------------------------------[ INC_DWORD|
|   [      I 0.0       ]                                                                 | IN/OUT:= |
|   (Rising Edge P)                                                                      | DB1.DBD0 |
|                                                                                        | (mm_raw) |
```

### Network 10.2: OB32 / OB33 (Cyclic Interrupt 100ms) - FB100 Lọc Mượt Vận Tốc Kích Đẩy
```text
|---| OB32_Trigger |---------------------------------------------------------------------[ CALL ]---|
|   [  OS_CYC32    ]                                                                     | FB100,   |
|                                                                                        | DB101    |
|                                                                                        | RawDist:=|
|                                                                                        | DB1.DBD0 |
|                                                                                        | DeltaT:= |
|                                                                                        | 100ms    |
|                                                                                        | SmoothSpd|
|                                                                                        | :=DB102  |
|                                                                                        | .DBD2    |
|                                                                                        | (mm/min) |
```

---

## PHẦN 11: MẠNG TRUYỀN THÔNG SMARTWIRE-DT & BẢO VỆ ĐỘNG CƠ EATON PKE/NZM

### Network 11.1: FC110 - Đọc Dữ Liệu Dòng Điện & Tình Trạng Quá Tải Từ Rơ Le Điện Tử Eaton PKE
```text
|---| DP_Bus_OK |------------------------------------------------------------------------[ DP_RD_DAT|
|   [  M 76.0   ]                                                                        | LADDR:=  |
|                                                                                        | W#16#120 |
|                                                                                        | (SWD_DP) |
|                                                                                        | RECORD:= |
|                                                                                        | DB31.DBB0|
|                                                                                        | ..DBB32  |
|                                                                                        | (Amperes)|
```

### Network 11.2: FC111 - Giám Sát Ngắt Mạch & Bảo Vệ Quá Dòng Aptomat Khối NZM
```text
|---| NZM_Trip_Bit |---------------------------------------------------------------------( S )------|
|   [ DB33.DBX0.1  ]                                                                      DB58.DBX0.2
|                                                                                        (Main_Trip)
```

---

## PHẦN 12: PHANH KẸP CỐNG & KHÓA CƠ KHÍ BÀN ĐẨY

### Network 12.1: FC18 - Điều Khiển Đóng / Nhả Phanh Kẹp Cống Bê Tông (Pipe Clamp Brake)
```text
--[ Đóng Phanh Kẹp Chặt Cống (Brake Clamp) ]------------------------------------------------------------
|---| Freigabe_OK |---| Jacks_Stopped |---| Clamp_Cmd_PB |--------------------------------(   )------|
|   [   M 61.0    ]   [ / ] Q 16.2   ]   [     I 64.1     ]                               ( Q 18.1 )  |
|                     (Không kích tiến)                                                (BrakeClamp_K)
--[ Nhả Phanh Kẹp Cống (Brake Release) ]----------------------------------------------------------------
|---| Freigabe_OK |---| Jacks_Advancing |------------------------------------------------(   )------|
|   [   M 61.0    ]   [    Q 16.2       ]                                                ( Q 18.3 )  |
|                                                                                        (BrakeRelease)
```

### Network 12.2: FC19 - Điều Khiển Khóa & Mở Khóa Chốt Cơ Khí Bàn Đẩy (Cylinder Lock)
```text
--[ Mở Khóa Chốt Bàn Đẩy (Unlock) ]--------------------------------------------------------------------
|---| Freigabe_OK |--+--| Unlock_PB  |--+---| Interlock_OK |-----------------------------(   )------|
|   [   M 61.0    ]  |  [   I 62.4   ]  |   [   M 61.0     ]                             ( Q 19.1 )  |
|                    |  | Cyl_Unlocked| |                                                (Lock_Open) |
|                    +--|   M 12.7    |-+                                                            |
--[ Đóng Khóa Chốt Bàn Đẩy (Lock) ]--------------------------------------------------------------------
|---| Freigabe_OK |---| Lock_PB |---| Jacks_Home_Pos |-----------------------------------(   )------|
|   [   M 61.0    ]   [  I 62.5 ]   [     I 62.0     ]                                   ( Q 19.3 )  |
|                                                                                        (Lock_Close)|
```

---

## PHẦN 13: HỆ THỐNG LÀM MÁT, QUẠT KÉT DẦU & CẢNH BÁO RÒ RỈ LECKÖL

### Network 13.1: FC15 - Điều Khiển Tự Động Quạt Làm Mát Két Dầu Thủy Lực (Cooling Fans)
```text
|---| Freigabe_OK |---| OilTemp_High_55C |-----------------------------------------------(   )------|
|   [   M 61.0    ]   [ DB16.DBD24 > 55 ]                                                ( Q 4.0  )  |
|                                                                                        (CoolFan1_K)|
|                                                                                            +----( Q 3.0 )
|                                                                                                 (CoolFan2_K)
```

### Network 13.2: FC34 - Giám Sát Áp Suất Dầu Rò Rỉ Lecköl Gối Trục Đĩa Cắt (Case Drain Protection)
```text
|---| Lecköl_Pres |----------------------------------------------------------------------[ CMP > ]---|
|   [   PIW 372   ]                                                                      | IN1:=PIW372|
|                                                                                        | IN2:=13824 |
|                                                                                        | (5.0 bar)  |
|                                                                                        +-----+------+
|                                                                                              |
|----------------------------------------------------------------------------------------( S )-------|
|                                                                                          DB58.DBX1.4
|                                                                                          (Seal_Leak)
```

---

## PHẦN 14: ĐÈN PHA MẶT GƯƠNG & ĐIỀU KHIỂN CAMERA ĐẦU KHIÊN

### Network 14.1: FC95 - Bật Đèn Chiếu Sáng Mặt Gương & Chuyển Kênh Video Camera
```text
--[ Bật Đèn Pha Halogen Đầu Khiên (Scheinwerfer) ]------------------------------------------------------
|---| Freigabe_OK |---| Light_Sw_PB |----------------------------------------------------(   )------|
|   [   M 61.0    ]   [    I 65.0   ]                                                    ( Q 51.0 )  |
|                                                                                        (HeadLight) |
--[ Chuyển Đổi Kênh Camera Giám Sát (Video Switch) ]---------------------------------------------------
|---| Cam_Select_L |---------------------------------------------------------------------(   )------|
|   [    I 65.1    ]                                                                      ( Q 20.2 )  |
|                                                                                        (Cam_Left)  |
|---| Cam_Select_R |---------------------------------------------------------------------(   )------|
|   [    I 65.2    ]                                                                      ( Q 20.3 )  |
|                                                                                        (Cam_Right) |
```

---

## PHẦN 15: CÁC KHỐI NGẮT CHỐNG SẬP CPU

### Network 15.1: OB82 - Khối Ngắt Chẩn Đoán Module Ngoại Vi (Diagnostic Interrupt)
```text
|---| OB82_Trigger |---------------------------------------------------------------------[ MOVE ]---|
|   [   OS_OB82    ]                                                                     | IN:=     |
|                                                                                        | OB82_MDL |
|                                                                                        | OUT:=    |
|                                                                                        | DB58.    |
|                                                                                        | DBW10    |
```

### Network 15.2: OB121 / OB122 - Khối Ngắt Chống STOP CPU Khi Lỗi Lập Trình & Lỗi Truy Cập I/O
```text
|---| OB122_IO_Fault |-------------------------------------------------------------------( S )------|
|   [    OS_OB122    ]                                                                    DB58.DBX0.3
|                                                                                        (IO_Access_F)
```
* **Ý nghĩa:** Khi một module I/O bị ngắt điện hoặc rút cáp trong quá trình thi công hầm, nếu không có `OB82`, `OB86`, `OB121`, `OB122`, CPU S7-300 sẽ lập tức rơi vào trạng thái `STOP`. Việc nạp các OB này giúp CPU duy trì hoạt động an toàn và ghi nhận lỗi vào `DB58`.

---

## PHẦN 16: GIAO TIẾP HMI MÀN HÌNH CẢM ỨNG & NHẬT KÝ SCADA

### Network 16.1: DB19 - Đồng Bộ Nút Bấm & Thanh Trượt Vận Hành Từ HMI Touchscreen
```text
|---| HMI_Auto_Toggle |------------------------------------------------------------------( S )------|
|   [  DB19.DBX0.0    ]                                                                   M 10.0    |
|                                                                                        (Auto_Mode)|
|---| HMI_Manual_Tog  |------------------------------------------------------------------( R )------|
|   [  DB19.DBX0.1    ]                                                                   M 10.0    |
|                                                                                        (Auto_Mode)|
```

### Network 16.2: DB57 - Định Kỳ Ghi Nhật Ký Dữ Liệu Khoan SCADA (DataLogging2)
```text
|---| 1Sec_Clock_Pulse |-----------------------------------------------------------------[ BLKMOV ]-|
|   [     M 0.5        ]                                                                 | SFC20    |
|                                                                                        | SRC:=    |
|                                                                                        | P#DB1.DBX|
|                                                                                        | 0.0 BYTE |
|                                                                                        | 64       |
|                                                                                        | DST:=    |
|                                                                                        | P#DB57.  |
|                                                                                        | DBX0.0   |
```

---

## BẢNG TỔNG KẾT TOÀN BỘ KHỐI LỆNH

| Loại khối | Số hiệu khối | Tên Ký hiệu (Symbol) | Vai trò chi tiết trong hệ thống TBM S7-300 |
|:---|:---|:---|:---|
| **OB** | `OB1` | `MainControl` | Vòng quét tuần hoàn chính (14 Networks công nghệ) |
| **OB** | `OB32 / OB33` | `CYC_INT2 / 3` | Ngắt chu kỳ thời gian thực (100ms) tính toán vận tốc kích |
| **OB** | `OB82` | `I/O_FLT1` | Ngắt chẩn đoán sự cố module ngoại vi Analog/Digital |
| **OB** | `OB86` | `RACK_FLT` | Chống dừng CPU khi mất trạm Profibus-DP Slaves |
| **OB** | `OB100` | `COMPLETE RESTART`| Khởi tạo giá trị ban đầu khi bật nguồn CPU |
| **OB** | `OB121 / 122` | `PROG_ERR / MOD_ERR`| Ngăn ngừa CPU STOP khi gặp lỗi truy cập ngoại vi |
| **FC** | `FC61` | `Freigaben` | Liên động an toàn, phao dầu, dừng khẩn cấp E-Stop |
| **FC** | `FC71, 72` | `Fault_Manager` | Quản lý còi báo sự cố và nút ấn xóa lỗi |
| **FC** | `FC13, 30, 33` | `PowerPack / Filter`| Khởi động bơm chính 132kW và giám sát nghẹt lọc |
| **FC** | `FC14, 25, 26` | `Cutter Control/Speed`| Chiều quay CW/CCW, điều tốc đĩa bơm Schürfrad |
| **FC** | `FC4, 5, 140, 150` | `Q_Cutter_CSG/A4VG`| Đường đặc tính lưu lượng bơm Rexroth A4VG / CSG |
| **FC** | `FC20, 21, 22` | `Jacks Control/Pres`| Điều khiển kích chính, quy đổi Bar sang Tấn (DB17) |
| **FC** | `FC55, 56, 57` | `Interjack 1, 2, 3`| Điều khiển trạm kích trung gian Dehner 1..3 |
| **FC** | `FC40, 41, 42` | `Steering Cyl 1..3`| Điều khiển 3 xi lanh bẻ khớp lái 120° (DB59) |
| **FC** | `FC43` | `Wing` | Cánh ghim chống xoay thân khiên đào |
| **FC** | `FC80, 81, 82` | `TACS / Inclinometer`| Bia ngắm laser TACS, đo góc nghiêng Pitch & bù xoắn Roll |
| **FC** | `FC10, 11, 12` | `Spülpumpen Control`| Bơm cấp, bơm thải và bơm tăng áp bùn bentonite |
| **FC** | `FC90, 91` | `Hochdruck Pumpe/Valve`| Bơm cao áp 400 bar & béc phun tia nước mặt gương |
| **FC** | `FC18, 19` | `Bremse / Verriegelung`| Phanh kẹp cống bê tông và chốt khóa cơ khí bàn đẩy |
| **FC** | `FC15, 34` | `Lüfter / Lecköl` | Quạt làm mát két dầu và bảo vệ rò rỉ phớt gối trục |
| **FC** | `FC95` | `Scheinwerfer / Kamera`| Đèn pha mặt gương và chuyển kênh camera đầu khiên |
| **FC** | `FC110, 111, 112`| `SWD / Altivar` | Mạng SmartWire Eaton PKE & biến tần Schneider |
| **FC** | `FC35, FB100` | `Distance / Speed` | Đo chiều dài kích đẩy và lọc số vận tốc (DB1/DB102) |
| **DB** | `DB1, DB102` | `LV_DB / Speed_DB` | Dữ liệu quãng đường thô và vận tốc mượt |
| **DB** | `DB16, DB17` | `Analog / Tonnen` | Bộ đệm Analog 45 kênh và bảng tra Bar ra Tấn |
| **DB** | `DB19, DB57` | `HMI / DataLogging2`| Vùng nhớ màn hình cảm ứng và nhật ký SCADA |
| **DB** | `DB31, DB33` | `SWD_PKE / NZM` | Dữ liệu dòng điện tải SmartWire-DT |
| **DB** | `DB58, DB59` | `Fault_DB / Cyl_Force`| Bảng bit mã lỗi và lực tì 3 xi lanh lái |

---
*Tài liệu toàn diện được trích xuất và chuẩn hóa 100% từ dự án Siemens STEP 7 v5.x - Machine Bohrkopf 2100 (mts Perforator).*
