
---

## MỤC LỤC TỔNG QUAN

- [1. NGUYÊN LÝ HOẠT ĐỘNG CÔNG NGHỆ MÁY KHOAN NGẦM TBM](#1-nguyên-lý-hoạt-động-công-nghệ-máy-khoan-ngầm-tbm)
- [2. CẤU TRÚC PHẦN CỨNG PLC & BẢN ĐỒ CẢM BIẾN TRƯỜNG](#2-cấu-trúc-phần-cứng-plc--bản-đồ-cảm-biến-trường)
- [3. KHAI BÁO KIỂU DỮ LIỆU CẤU TRÚC (UDT - USER DATA TYPES)](#3-khai-báo-kiểu-dữ-liệu-cấu-trúc-udt---user-data-types)
- [4. KHAI BÁO CÁC VÙNG NHỚ DỮ LIỆU TOÀN CỤC (DATA BLOCKS - DB)](#4-khai-báo-các-vùng-nhớ-dữ-liệu-toàn-cục-data-blocks---db)
- [5. CÁC KHỐI TỔ CHỨC HỆ THỐNG (ORGANIZATION BLOCKS - OB)](#5-các-khối-tổ-chức-hệ-thống-organization-blocks---ob)
  - [OB100: Khởi Tạo Hệ Thống Khi Cấp Nguồn (Complete Restart)](#ob100-khởi-tạo-hệ-thống-khi-cấp-nguồn-complete-restart)
  - [OB86: Xử Lý Sự Cố Đứt Cáp / Mất Trạm Profibus-DP (Rack Fault)](#ob86-xử-lý-sự-cố-đứt-cáp--mất-trạm-profibus-dp-rack-fault)
  - [OB32 & FB100: Ngắt Chu Kỳ 100ms Lọc Vận Tốc Kích Đẩy (Speed Smoothing Filter)](#ob32--fb100-ngắt-chu-kỳ-100ms-lọc-vận-tốc-kích-đẩy-speed-smoothing-filter)
  - [OB1: Vòng Quét Tuần Hoàn Chính Thực Thi 14 Networks Công Nghệ](#ob1-vòng-quét-tuần-hoàn-chính-thực-thi-14-networks-công-nghệ)
- [6. CÁC HÀM CÔNG NGHỆ CHI TIẾT (FUNCTION BLOCKS - FC)](#6-các-hàm-công-nghệ-chi-tiết-function-blocks---fc)
  - [Module 1: An Toàn & Liên Động Khẩn Cấp (FC61, FC71, FC72, FC76)](#module-1-an-toàn--liên-động-khẩn-cấp-fc61-fc71-fc72-fc76)
  - [Module 2: Trạm Nguồn Thủy Lực & Động Cơ Chính (FC13, FC30, FC33)](#module-2-trạm-nguồn-thủy-lực--động-cơ-chính-fc13-fc30-fc33)
  - [Module 3: Đầu Cắt Schürfrad & Điều Tốc Bơm Rexroth A4VG (FC14, FC25, FC26, FC5, FC140)](#module-3-đầu-cắt-schürfrad--điều-tốc-bơm-rexroth-a4vg-fc14-fc25-fc26-fc5-fc140)
  - [Module 4: Trạm Kích Đẩy Chính & Tính Lực Tấn (FC20, FC21, FC22, DB17)](#module-4-trạm-kích-đẩy-chính--tính-lực-tấn-fc20-fc21-fc22-db17)
  - [Module 5: Đồng Bộ Trạm Kích Trung Gian Dehner 1..5 (FC55, FC56, FC57, FC141)](#module-5-đồng-bộ-trạm-kích-trung-gian-dehner-15-fc55-fc56-fc57-fc141)
  - [Module 6: Hệ Thống Bẻ Lái 3 Xi Lanh 120° & Cánh Chống Xoay (FC40, FC41, FC42, FC43)](#module-6-hệ-thống-bẻ-lái-3-xi-lanh-120--cánh-chống-xoay-fc40-fc41-fc42-fc43)
  - [Module 7: Định Vị Laser TACS & Đo Độ Nghiêng Pitch/Roll (FC80, FC81, FC82)](#module-7-định-vị-laser-tacs--đo-độ-nghiêng-pitchroll-fc80-fc81-fc82)
  - [Module 8: Tuần Hoàn Bùn Bentonite & Biến Tần Schneider (FC10, FC11, FC12, FC112)](#module-8-tuần-hoàn-bùn-bentonite--biến-tần-schneider-fc10-fc11-fc12-fc112)
  - [Module 9: Bơm Nước Cao Áp 400 bar & Béc Phun Rửa Đĩa Cắt (FC90, FC91)](#module-9-bơm-nước-cao-áp-400-bar--béc-phun-rửa-đĩa-cắt-fc90-fc91)
  - [Module 10: Đo Chiều Dài Hầm Đào (FC35 Distance Pulse)](#module-10-đo-chiều-dài-hầm-đào-fc35-distance-pulse)
  - [Module 11: Mạng Cáp Dẹt SmartWire-DT Eaton PKE & NZM (FC110, FC111)](#module-11-mạng-cáp-dẹt-smartwire-dt-eaton-pke--nzm-fc110-fc111)
  - [Module 12: Giao Tiếp HMI & Nhật Ký Khoan SCADA (DB19, DB57, FC126)](#module-12-giao-tiếp-hmi--nhật-ký-khoan-scada-db19-db57-fc126)

---

## 1. NGUYÊN LÝ HOẠT ĐỘNG CÔNG NGHỆ MÁY KHOAN NGẦM TBM

Máy khoan ngầm TBM (Microtunneling Bohrkopf 2100) là cỗ máy thi công ngầm đào đường hầm xuyên lòng đất mà không cần đào hở mặt đường:

```
[MẶT ĐẤT] =============== CONTAINER NGUỒN (PLC S7-300 + HMI + BƠM 132kW) ===============
                                  |
                                  | Ống thủy lực + Cáp Profibus + Ống bùn bentonite
                                  v
[GIẾNG KÍCH] ---> [BÀN ĐẨY KÍCH CHÍNH] ===> [ĐỐT CỐNG 1] ===> [DEHNER 1] ===> [ĐẦU KHIÊN TBM]
                    (FC20 / PQW264)          (Bê tông)       (FC55)        (MÂM CẮT ĐÀO ĐẤT)
```

1. **Đầu cắt (Schürfrad):** Đĩa cắt xoay tròn gắn các răng cào hợp kim kim cương để gọt vụn đất đá phía trước mặt gương đào. Dẫn động bằng động cơ điện 132kW/160kW và cụm bơm thủy lực biến tích điều chỉnh góc nghiêng đĩa bơm (Rexroth A4VG / CSG).
2. **Cân bằng áp lực bùn (Slurry Shield):** Dung dịch bùn bentonite được bơm nạp vào buồng đào để giữ ổn định vách hầm, chống sạt lở đất và làm nguội đĩa cắt. Bùn mang vụn đất đá được bơm hút thải ra ngoài giếng lên trạm xử lý tách cát.
3. **Kích đẩy chính (Main Jacks):** Đặt tại giếng xuất phát, đẩy toàn bộ đoàn cống bê tông và đầu khiên tiến về phía trước theo từng hành trình 2.5m.
4. **Trạm kích trung gian (Intermediate Jacking Stations - Dehner 1..5):** Khi hầm dài hàng trăm mét, lực ma sát giữa đất và vỏ cống rất lớn. Các trạm Dehner phân đoạn tuyến cống thành nhiều khúc và kích đẩy lần lượt từng khúc để tránh làm vỡ cống bê tông.
5. **Hệ thống lái hướng (Steering System):** Gồm 3 xi lanh bố trí lệch nhau 120° nối khớp linh hoạt giữa vỏ khiên trước và vỏ khiên sau. Bằng cách thò/thụt 3 xi lanh này, đầu khiên có thể bẻ cong sang trái, phải, lên, xuống theo tim đường hầm thiết kế.
6. **Định vị Laser TACS & Đo nghiêng mts:** Một chùm tia laser phát từ giếng kích chiếu thẳng vào bia ngắm quang điện tử TACS ở đuôi khiên. Cảm biến nghiêng Inclinometer đo độ dốc (Pitch) và độ xoắn lật (Roll) của thân máy.
7. **Cánh chống xoay (Wing / Flossenzylinder):** Khi đầu cắt quay gặp đất cứng, phản lực mô-men xoắn sẽ làm thân khiên xoay tròn. PLC tự động điều khiển xi lanh bung cánh thép tì chặt vào vách đất để ghim cố định thân khiên.
8. **Tia nước cao áp 400 bar (High Pressure Water Jet):** Bơm piston nén nước áp lực siêu cao 400 bar xịt qua 4 béc phun mặt gương để xối rửa đất sét dẻo quánh dính bết vào răng đĩa cắt.

---

## 2. CẤU TRÚC PHẦN CỨNG PLC & BẢN ĐỒ CẢM BIẾN TRƯỜNG

```
                   SIEMENS S7-300 CPU 315-2 PN/DP
                   (Order: 6ES7 315-2EH14-0AB0)
                                |
               +----------------+----------------+
               | (Profibus-DP 12 Mbps)           | (Profinet / Ethernet)
               |                                 |
      +--------+--------+               +--------+--------+
      |                 |               |                 |
[DP 7 / 16]        [DP 5, 8, 15]     [HMI Touchscreen] [SCADA Máy Tính]
Turck BL20         Turck BL67/SDPB      (DB19)            (DB57)
Tủ điện Container  Đầu khiên IP67
```

### Bản Đồ Tín Hiệu Analog Input (`PIW`) Từ Cảm Biến:
* `PIW 406`: Áp suất kích chính Container (0–400 bar).
* `PIW 410`: Áp suất bơm đầu cắt Container (0–400 bar).
* `PIW 378`: Áp suất thủy lực đĩa cắt trên đầu khiên (0–350 bar).
* `PIW 364 / 366 / 368`: Áp suất 3 xi lanh lái 120° (0–350 bar).
* `PIW 356 / 358 / 360`: Hành trình đo độ thò thụt 3 xi lanh lái 120° (0–200 mm).
* `PIW 370 / 332`: Cảm biến đo góc xoay Roll & góc dốc Pitch ($\pm 30.0^\circ$).
* `PIW 336 / 344 / 380`: Hành trình mở rộng 3 trạm kích trung gian Dehner 1..3 (0–1200 mm).
* `PIW 338 / 346 / 382`: Áp suất thủy lực 3 trạm kích trung gian Dehner 1..3 (0–400 bar).
* `PIW 256 / 258`: Áp suất (0–400 bar) và lưu lượng dòng chảy (0–100%) nước cao áp.
* `PIW 374 / 376 / 334`: Áp lực buồng đào, ống bùn xả và ống bùn nạp (0–10 bar).
* `PIW 420 / 422`: Lưu lượng kế bùn nạp và bùn xả (0–500 m³/h).
* `PIW 404 / 408`: Nhiệt độ (-20..+100°C) và độ ẩm/nước lẫn trong dầu (0–100%).
* `PIW 372 / 294`: Áp suất đường dầu rò rỉ (Lecköl) bảo vệ phớt kín gối trục (0–10 bar).
* `PIW 308 / 306 / 292`: Độ chênh áp cảnh báo nghẹt lọc dầu thủy lực.

### Bản Đồ Tín Hiệu Analog Output (`PQW`) Điều Khiển Van Tỷ Lệ:
* `PQW 264`: Điện áp 0–10V điều chỉnh vận tốc kích đẩy chính.
* `PQW 266`: Điện áp 0–10V điều chỉnh áp suất kích đẩy chính.
* `PQW 268`: Điện áp 0–10V điều chỉnh góc nghiêng đĩa bơm biến tích Rexroth A4VG / CSG (điều tốc mâm cắt).

---

## 3. KHAI BÁO KIỂU DỮ LIỆU CẤU TRÚC (UDT - USER DATA TYPES)

Trong Siemens SCL, `TYPE ... END_TYPE` được dùng để gom nhóm các thuộc tính của một thiết bị phức tạp lại với nhau:

```pascal
TYPE UDT_AnalogSensor :
STRUCT
    Raw_PIW         : INT;      // Giá trị số nguyên thô đọc từ thanh ghi ngoại vi (0 - 27648)
    Val_Scaled      : REAL;     // Giá trị thực tế sau khi đã quy đổi sang đơn vị kỹ thuật (bar, mm, độ C)
    Min_Physical    : REAL;     // Giới hạn nhỏ nhất của dải đo cảm biến
    Max_Physical    : REAL;     // Giới hạn lớn nhất của dải đo cảm biến
    High_Alarm_Thresh : REAL;   // Ngưỡng cảnh báo cao
    Alarm_High_Bit  : BOOL;     // Cờ báo vượt ngưỡng an toàn
    Wire_Break_Fault: BOOL;     // Cờ báo đứt dây (Raw < -200 hoặc > 28500)
END_STRUCT
END_TYPE

TYPE UDT_SteeringCylinder :
STRUCT
    Pressure_Bar    : REAL;     // Áp lực dầu thực tế trong xi lanh (0 - 350 bar)
    Stroke_mm       : REAL;     // Độ thò thụt thực tế đo bằng cảm biến LVDT (0 - 200 mm)
    Target_Stroke_mm: REAL;     // Chiều dài mục tiêu người vận hành muốn bẻ lái
    Valve_Out_Cmd   : BOOL;     // Lệnh kích mở van điện từ đẩy xi lanh ra
    Valve_In_Cmd    : BOOL;     // Lệnh kích mở van điện từ kéo xi lanh vào
    Soft_Limit_Max  : BOOL;     // Giới hạn phần mềm hành trình cực đại (195 mm)
    Soft_Limit_Min  : BOOL;     // Giới hạn phần mềm hành trình cực tiểu (5 mm)
END_STRUCT
END_TYPE

TYPE UDT_DehnerStation :
STRUCT
    Station_ID      : INT;      // Số thứ tự trạm kích trung gian (1 .. 5)
    Pressure_Bar    : REAL;     // Áp suất thủy lực của trạm (0 - 400 bar)
    Stroke_mm       : REAL;     // Hành trình mở rộng của đốt cống (0 - 1200 mm)
    Valve_Advance_Q : BOOL;     // Ngõ ra điều khiển van kích đẩy của trạm
    Auto_Sequence_En: BOOL;     // Cho phép tham gia vào chu trình kích tuần hoàn tự động
    Stroke_Completed: BOOL;     // Cờ xác nhận trạm đã đẩy hết hành trình thiết kế
END_STRUCT
END_TYPE

TYPE UDT_AltivarVFD :
STRUCT
    Control_Word    : WORD;     // Lệnh điều khiển gửi tới biến tần (CMD: Run, Stop, Reset)
    Target_Freq_Hz  : INT;      // Tần số đặt cho động cơ (0.1 Hz, ví dụ 500 = 50.0 Hz)
    Status_Word     : WORD;     // Trạng thái biến tần phản hồi về PLC (ETA: Ready, Running, Fault)
    Actual_Freq_Hz  : INT;      // Tần số thực tế động cơ đang quay
    Motor_Current_A : REAL;     // Dòng điện tải thực tế (A)
    Fault_Code      : WORD;     // Mã lỗi chẩn đoán từ biến tần Schneider
END_STRUCT
END_TYPE
```

---

## 4. KHAI BÁO CÁC VÙNG NHỚ DỮ LIỆU TOÀN CỤC (DATA BLOCKS - DB)

Data Block là "bộ nhớ kho" của PLC để lưu trữ dữ liệu tập trung:

```pascal
DATA_BLOCK DB16 // DB Analog Values
TITLE = 'Bộ Đệm Lưu Trữ Toàn Bộ Giá Trị Cảm Biến Trường Sau Chuẩn Hóa'
STRUCT
    // Nhóm 1: Cảm biến Áp Suất Thủy Lực (Hydraulic Pressures)
    Jacks_Pres_PP       : REAL; // Áp suất kích chính Container (0 - 400 bar)
    Cutter_Pres_PP      : REAL; // Áp suất bơm đầu cắt Container (0 - 400 bar)
    Cutter_Pres_Head    : REAL; // Áp suất trực tiếp trên đĩa cắt đầu khiên (0 - 350 bar)
    Cyl1_Pres           : REAL; // Áp suất xi lanh bẻ lái 1 (0 - 350 bar)
    Cyl2_Pres           : REAL; // Áp suất xi lanh bẻ lái 2 (0 - 350 bar)
    Cyl3_Pres           : REAL; // Áp suất xi lanh bẻ lái 3 (0 - 350 bar)
    Steering_Feed_Pres  : REAL; // Áp suất mạch bơm cấp bẻ lái (0 - 250 bar)

    // Nhóm 2: Cảm biến Hành Trình Đo Dịch Chuyển (Displacements)
    Cyl1_Stroke_mm      : REAL; // Hành trình xi lanh lái 1 (0 - 200 mm)
    Cyl2_Stroke_mm      : REAL; // Hành trình xi lanh lái 2 (0 - 200 mm)
    Cyl3_Stroke_mm      : REAL; // Hành trình xi lanh lái 3 (0 - 200 mm)
    Dehner1_Stroke_mm   : REAL; // Hành trình trạm kích trung gian 1 (0 - 1200 mm)
    Dehner2_Stroke_mm   : REAL; // Hành trình trạm kích trung gian 2 (0 - 1200 mm)
    Dehner3_Stroke_mm   : REAL; // Hành trình trạm kích trung gian 3 (0 - 1200 mm)

    // Nhóm 3: Cảm biến Góc Nghiêng & Định Vị (Inclinometer & Navigation)
    Roll_Angle_Deg      : REAL; // Góc xoay lật quanh trục dọc thân khiên (-30.0 đến +30.0 Deg)
    Pitch_Angle_Deg     : REAL; // Góc dốc lên/xuống của thân khiên (-30.0 đến +30.0 Deg)

    // Nhóm 4: Cụm Nước Cao Áp 400 bar (High Pressure Water Jet)
    HD_Water_Pres_bar   : REAL; // Áp suất nước ngõ ra bơm cao áp (0 - 400 bar)
    HD_Water_Flow_pct   : REAL; // Lưu lượng dòng chảy nước cấp (0 - 100 %)

    // Nhóm 5: Áp Suất & Lưu Lượng Bùn Bentonite (Slurry Circuit)
    Slurry_Chamber_bar  : REAL; // Áp lực buồng đào mặt gương (0 - 10 bar)
    Slurry_Line_bar     : REAL; // Áp suất đường ống bùn xả (0 - 10 bar)
    Slurry_Charge_bar   : REAL; // Áp suất đường ống bùn nạp (0 - 10 bar)
    Slurry_Charge_m3h   : REAL; // Lưu lượng kế nạp bùn (0 - 500 m3/h)
    Slurry_Disch_m3h    : REAL; // Lưu lượng kế xả bùn (0 - 500 m3/h)

    // Nhóm 6: Nhiệt Độ, Độ Ẩm & Bảo Vệ Gối Trục (Health & Protection)
    Oil_Temp_Container  : REAL; // Nhiệt độ dầu thủy lực bể Container (-20 đến +100 °C)
    Oil_Temp_Head       : REAL; // Nhiệt độ dầu thủy lực cụm van đầu khiên (-20 đến +100 °C)
    Oil_Humidity_Pct    : REAL; // Tỷ lệ hàm lượng nước lẫn trong dầu thủy lực (0 - 100 %)
    CaseDrain_Pres_bar  : REAL; // Áp suất đường dầu rò rỉ phớt trục cắt Lecköl (0 - 10 bar)
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB17 // DB Druck-Tonnen
TITLE = 'Bảng Quy Đổi Áp Suất Thủy Lực (Bar) Thành Lực Kích Đẩy (Tấn)'
STRUCT
    Total_Jacking_Tons  : REAL; // Tổng lực đẩy thực tế của 4 xi lanh chính (Tấn)
    Piston_Diameter_mm  : REAL := 280.0;  // Đường kính piston xi lanh chính
    Num_Cylinders       : INT  := 4;      // Số lượng xi lanh kích chính
    Piston_Area_Total   : REAL := 2454.3; // Tổng tiết diện (cm2) = 4 * (pi * 28^2 / 4)
    Bar_To_Ton_Factor   : REAL := 0.24543;// Hệ số: 1 bar trên 2454.3 cm2 = 0.24543 Tấn
    Dehner1_Force_Tons  : REAL; // Lực kích đẩy trạm Dehner 1
    Dehner2_Force_Tons  : REAL; // Lực kích đẩy trạm Dehner 2
    Dehner3_Force_Tons  : REAL; // Lực kích đẩy trạm Dehner 3
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB19 // DB Touchscreen HMI
TITLE = 'Vùng Nhớ Giao Tiếp Màn Hình Điều Khiển HMI'
STRUCT
    Btn_Mode_Auto       : BOOL; // Nút chọn chế độ vận hành Tự động
    Btn_Mode_Manual     : BOOL; // Nút chọn chế độ vận hành Thủ công
    Btn_Cutter_CW       : BOOL; // Nút chọn quay mâm cắt chiều thuận (Quay phải)
    Btn_Cutter_CCW      : BOOL; // Nút chọn quay mâm cắt chiều nghịch (Quay trái)
    Btn_Cutter_Start    : BOOL; // Nút ấn khởi động quay đầu cắt
    Btn_Cutter_Stop     : BOOL; // Nút ấn dừng quay đầu cắt
    Btn_Jacks_Advance   : BOOL; // Nút ấn kích đẩy tiến
    Btn_Jacks_Retract   : BOOL; // Nút ấn thu hồi kích lùi
    Btn_Water_Jet_On    : BOOL; // Nút bật van béc phun tia nước cao áp 400 bar
    Btn_Reset_Fault     : BOOL; // Nút xác nhận xóa cờ lỗi hệ thống
    Btn_Wing_Extend     : BOOL; // Nút bung cánh ghim chống xoay khiên
    Btn_Wing_Retract    : BOOL; // Nút thu cánh chống xoay về thân khiên
    Slider_Cutter_Speed : REAL; // Chiết áp cài đặt tốc độ đầu cắt (0.0 - 100.0 %)
    Slider_Jacks_Speed  : REAL; // Chiết áp cài đặt vận tốc kích đẩy (0.0 - 100.0 %)
    Slider_Steer_Angle  : REAL; // Chiết áp cài đặt góc bẻ lái (-30.0 đến +30.0 Deg)
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB58 // DB Fault_DB
TITLE = 'Bảng Cờ Báo Lỗi & Sự Cố Toàn Hệ Thống'
STRUCT
    Master_Fault_Trip   : BOOL; // Cờ sự cố tổng ngắt toàn bộ máy
    EStop_Pushed        : BOOL; // Nút dừng khẩn cấp E-Stop bị nhấn
    Profibus_Loss_Slave : BOOL; // Rớt kết nối trạm I/O Profibus-DP
    Hydr_Oil_Level_Low  : BOOL; // Cạn dầu thủy lực bể chứa
    Hydr_Oil_Temp_High  : BOOL; // Dầu thủy lực quá nhiệt > 70°C
    Main_Filter_Clogged : BOOL; // Nghẹt phin lọc dầu áp lực
    Motor_Current_Trip  : BOOL; // Động cơ quá tải dòng điện PKE
    HD_Water_No_Flow    : BOOL; // Bơm cao áp 400 bar bị mất nước mồi
    Cutter_Stall_Torque : BOOL; // Đĩa cắt bị kẹt đá quá mô-men xoắn
    Slurry_Pressure_High: BOOL; // Quá áp lực buồng đào mặt gương
    Shield_Roll_Excess  : BOOL; // Thân khiên bị xoay nghiêng quá +-1.5 độ
END_STRUCT
BEGIN
END_DATA_BLOCK
```

---

## 5. CÁC KHỐI TỔ CHỨC HỆ THỐNG (ORGANIZATION BLOCKS - OB)

Khối tổ chức (OB) là cầu nối giữa hệ điều hành của CPU S7-300 và chương trình người dùng.

### OB100: Khởi Tạo Hệ Thống Khi Cấp Nguồn (Complete Restart)
```pascal
ORGANIZATION_BLOCK OB100
TITLE = 'Khởi Tạo Toàn Bộ Tham Số Khi Bật Điện CPU'
VAR_TEMP
    OB100_EV_CLASS  : BYTE;
    OB100_STRT_INFO : BYTE;
END_VAR

BEGIN
    // 1. Xóa toàn bộ các bit cờ báo lỗi
    DB58.Master_Fault_Trip    := FALSE;
    DB58.EStop_Pushed         := FALSE;
    DB58.Profibus_Loss_Slave  := FALSE;
    DB58.Hydr_Oil_Level_Low   := FALSE;
    DB58.Hydr_Oil_Temp_High   := FALSE;
    DB58.Main_Filter_Clogged  := FALSE;
    DB58.Motor_Current_Trip   := FALSE;
    DB58.HD_Water_No_Flow     := FALSE;
    DB58.Cutter_Stall_Torque  := FALSE;
    DB58.Slurry_Pressure_High := FALSE;
    DB58.Shield_Roll_Excess   := FALSE;

    // 2. Thiết lập giá trị chiết áp khởi điểm an toàn
    DB19.Slider_Cutter_Speed  := 30.0; // Tốc độ cắt khởi động 30%
    DB19.Slider_Jacks_Speed   := 20.0; // Tốc độ kích khởi động 20%
    DB19.Slider_Steer_Angle   := 0.0;  // Giữ lái hướng thẳng 0.0 độ
    DB19.Btn_Mode_Auto        := FALSE;
    DB19.Btn_Mode_Manual      := TRUE; // Mặc định ở chế độ Thủ công (Manual)

    // 3. Reset các ngõ ra van thủy lực về trạng thái đóng an toàn
    Q16.0 := FALSE; // Kích lùi trái
    Q16.2 := FALSE; // Kích tiến trái
    Q17.0 := FALSE; // Kích lùi phải
    Q17.2 := FALSE; // Kích tiến phải
    Q48.0 := FALSE; // Bơm cao áp 400 bar
    Q51.4 := FALSE; // Cánh chống xoay

    // 4. Bật cờ hoàn tất khởi tạo hệ thống
    M100.0 := TRUE; // Sys_Init_Done
END_ORGANIZATION_BLOCK
```

### OB86: Xử Lý Sự Cố Đứt Cáp / Mất Trạm Profibus-DP (Rack Fault)
```pascal
ORGANIZATION_BLOCK OB86
TITLE = 'Chống Dừng CPU Khi Đứt Cáp Hoặc Mất Nguồn Trạm DP Slave'
VAR_TEMP
    OB86_EV_CLASS : BYTE; // 16#39: Sự cố xuất hiện, 16#38: Sự cố đã hết
    OB86_FLT_ID   : BYTE;
    OB86_RACK_NUM : BYTE; // Địa chỉ trạm DP bị sự cố (Station Address)
END_VAR

BEGIN
    // Khi một trạm I/O IP67 trên đầu khiên bị mất kết nối:
    IF (OB86_EV_CLASS = 16#39) THEN
        DB58.Profibus_Loss_Slave := TRUE;
        DB58.Master_Fault_Trip   := TRUE;
        M76.0                    := FALSE; // Cờ DP_Bus_OK bị ngắt
    ELSE
        // Trạm I/O đã kết nối lại bình thường:
        DB58.Profibus_Loss_Slave := FALSE;
        M76.0                    := TRUE;  // Cờ DP_Bus_OK được phục hồi
    END_IF;
END_ORGANIZATION_BLOCK
```

### OB32 & FB100: Ngắt Chu Kỳ 100ms Lọc Vận Tốc Kích Đẩy (Speed Smoothing Filter)
* **Nguyên lý cho học viên:** Cảm biến đo quãng đường phát ra các xung rời rạc. Nếu lấy hiệu số quãng đường chia thời gian trực tiếp thì vận tốc sẽ bị nhấp nháy, dao động rất mạnh do nhiễu số. Vì vậy, ta phải dùng **Bộ lọc trung bình trượt 10 điểm (10-point Moving Average Filter)** thực thi đều đặn trong ngắt chu kỳ thời gian thực `OB32 (100ms)`.

```pascal
FUNCTION_BLOCK FB100
TITLE = 'Bộ Lọc Trung Bình Trượt Làm Mịn Vận Tốc Kích Đẩy'
VAR_INPUT
    Raw_Dist_Input_mm : DINT;   // Quãng đường thô từ bộ đếm xung encoder (DB1.DBD0)
    Sample_Period_sec : REAL;   // Chu kỳ ngắt thời gian thực (0.1 giây)
END_VAR
VAR_OUTPUT
    Filtered_Speed_mmpm: REAL;  // Vận tốc kích sau lọc mượt (mm/phút)
END_VAR
VAR
    Last_Dist_mm      : DINT := 0;
    Speed_Samples     : ARRAY[0..9] OF REAL; // Mảng 10 mẫu vận tốc gần nhất
    Sample_Pointer    : INT := 0;
    Sum_Speed         : REAL := 0.0;
    Inst_Speed_mmpm   : REAL;
    i                 : INT;
END_VAR

BEGIN
    // Bước 1: Tính vận tốc tức thời: (mm / giây) * 60 giây = mm / phút
    Inst_Speed_mmpm := (DINT_TO_REAL(Raw_Dist_Input_mm - Last_Dist_mm) / Sample_Period_sec) * 60.0;
    Last_Dist_mm := Raw_Dist_Input_mm; // Lưu lại mẫu quãng đường cho chu kỳ sau

    // Khử nhiễu giá trị âm khi máy dừng
    IF Inst_Speed_mmpm < 0.0 THEN
        Inst_Speed_mmpm := 0.0;
    END_IF;

    // Bước 2: Đẩy giá trị mới vào mảng đệm vòng tròn (Ring Buffer)
    Speed_Samples[Sample_Pointer] := Inst_Speed_mmpm;
    Sample_Pointer := (Sample_Pointer + 1) MOD 10;

    // Bước 3: Tính trung bình cộng của 10 mẫu liên tiếp
    Sum_Speed := 0.0;
    FOR i := 0 TO 9 DO
        Sum_Speed := Sum_Speed + Speed_Samples[i];
    END_FOR;

    Filtered_Speed_mmpm := Sum_Speed / 10.0;
END_FUNCTION_BLOCK

ORGANIZATION_BLOCK OB32
TITLE = 'Khối Ngắt Chu Kỳ 100ms'
VAR_TEMP
    info : ARRAY[0..19] OF BYTE;
END_VAR

BEGIN
    // Gọi khối hàm FB100 với khối dữ liệu thực thể Instance DB101
    FB100.DB101(
        Raw_Dist_Input_mm   := DB1.DBD0,
        Sample_Period_sec   := 0.1,
        Filtered_Speed_mmpm => DB102.DBD2
    );
END_ORGANIZATION_BLOCK
```

### OB1: Vòng Quét Tuần Hoàn Chính Thực Thi 14 Networks Công Nghệ
```pascal
ORGANIZATION_BLOCK OB1
TITLE = 'Vòng Quét Chính Thực Thi Tuần Tự Toàn Bộ Các Hàm Công Nghệ'
VAR_TEMP
    OB1_EV_CLASS : BYTE;
    OB1_SCAN_1   : BYTE;
    OB1_PRIORITY : BYTE;
    OB1_OB_NUMBR : BYTE;
END_VAR

BEGIN
    // Network 1-2: Quét chẩn đoán trạng thái phần cứng & 12 trạm Profibus-DP
    FC125_DP_Diagnostics();

    // Network 3: Kiểm tra liên động an toàn Freigaben & Xử lý cờ báo lỗi
    FC61_Safety_Freigaben();
    FC71_Fault_Management();

    // Network 4-5: Khởi động bộ nguồn thủy lực Container & Bơm dầu chính
    FC30_Hydraulic_Container();
    FC13_Main_Motor_Control();
    FC33_Filter_Monitoring();

    // Network 6-7: Điều khiển chiều quay thuận/nghịch & Tốc độ đầu cắt Schürfrad
    FC25_Cutter_Direction();
    FC26_Cutter_Speed_Scale();

    // Network 8-9: Tính đường đặc tính lưu lượng bơm biến tích Rexroth A4VG / CSG
    FC5_Pump_Characteristic_A4VG();

    // Network 10: Điều khiển kích đẩy chính & 5 trạm kích trung gian Dehner
    FC20_MainJacks_Control();
    FC22_Bar_To_Tonnage_Calc();
    FC55_Dehner_Synchronizer();

    // Network 11: Điều khiển hệ thống tuần hoàn bùn bentonite & Biến tần Altivar
    FC10_Slurry_Charge_Pump();
    FC11_Slurry_Discharge_Pump();
    FC112_Altivar_VFD_Control();

    // Network 12-13: Định vị Laser TACS, Đo nghiêng & Bẻ khớp 3 xi lanh lái 120°
    FC80_TACS_Laser_Parser();
    FC81_Inclinometer_Read();
    FC40_Steering_Hydraulics();
    FC43_AntiRoll_Wing_Control();

    // Network 14: Đo chiều dài hầm, Phun nước cao áp 400 bar & Nhật ký SCADA
    FC35_Distance_Measurement();
    FC90_HighPressure_Pump();
    FC91_HD_Nozzles_Control();
    FC57_SCADA_DataLogging();
END_ORGANIZATION_BLOCK
```

---

## 6. CÁC HÀM CÔNG NGHỆ CHI TIẾT (FUNCTION BLOCKS - FC)

---

### Module 1: An Toàn & Liên Động Khẩn Cấp (FC61, FC71, FC72, FC76)

#### 🎯 Mục đích công nghệ:
Bảo vệ tính mạng thợ vận hành trong lòng hầm và ngăn ngừa hư hỏng cơ khí. Cờ `M61.0 (Freigabe_OK)` là **"chiếc chìa khóa tổng"** – nếu mất cờ này thì toàn bộ van thủy lực kích đẩy, mâm cắt và bơm cao áp sẽ lập tức ngắt nguồn.

#### 🔌 Cảm biến liên quan:
* `I 0.0`: Tiếp điểm NC nút Dừng khẩn cấp (E-Stop).
* `I 1.2 / I 6.1 / I 12.0`: Cảm biến phao báo mức dầu thủy lực trong Container và Đầu khiên.
* `PIW 404 / PIW 288`: Cảm biến nhiệt độ dầu thủy lực.
* `PIW 308`: Cảm biến độ chênh áp phin lọc dầu.
* `M 76.0`: Cờ xác nhận 12 trạm Profibus-DP kết nối thông suốt từ `FC76`.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC61_Safety_Freigaben : VOID
TITLE = 'Mạch Liên Động Kiểm Tra Điều Kiện An Toàn Tổng Thể'
VAR_TEMP
    Is_EStop_Healthy     : BOOL;
    Is_Oil_Level_Healthy : BOOL;
    Is_Oil_Temp_Healthy  : BOOL;
    Is_Filter_Healthy    : BOOL;
    Is_Fieldbus_Healthy  : BOOL;
    Is_Motors_Healthy    : BOOL;
END_VAR

BEGIN
    // 1. Kiểm tra nút Dừng khẩn cấp (E-Stop): Tiếp điểm vật lý là thường đóng NC
    IF I0.0 THEN
        Is_EStop_Healthy := TRUE;
        DB58.EStop_Pushed := FALSE;
    ELSE
        Is_EStop_Healthy := FALSE;
        DB58.EStop_Pushed := TRUE; // Bật cờ lỗi E-Stop
    END_IF;

    // 2. Kiểm tra mức dầu thủy lực: Phao mức giữa (I 1.2), phao cạn Container (I 12.0), phao cạn Đầu khiên (I 6.1)
    IF I1.2 AND I12.0 AND I6.1 THEN
        Is_Oil_Level_Healthy := TRUE;
        DB58.Hydr_Oil_Level_Low := FALSE;
    ELSE
        Is_Oil_Level_Healthy := FALSE;
        DB58.Hydr_Oil_Level_Low := TRUE; // Cạn dầu bể chứa
    END_IF;

    // 3. Kiểm tra nhiệt độ dầu: Không được vượt quá 70°C ở Container và 75°C ở Đầu khiên
    IF (DB16.Oil_Temp_Container <= 70.0) AND (DB16.Oil_Temp_Head <= 75.0) THEN
        Is_Oil_Temp_Healthy := TRUE;
        DB58.Hydr_Oil_Temp_High := FALSE;
    ELSE
        Is_Oil_Temp_Healthy := FALSE;
        DB58.Hydr_Oil_Temp_High := TRUE; // Quá nhiệt dầu
    END_IF;

    // 4. Kiểm tra độ nghẹt lọc dầu thủy lực
    IF NOT DB58.Main_Filter_Clogged THEN
        Is_Filter_Healthy := TRUE;
    ELSE
        Is_Filter_Healthy := FALSE;
    END_IF;

    // 5. Kiểm tra trạng thái mạng truyền thông Profibus-DP (FC76 / M76.0)
    Is_Fieldbus_Healthy := M76.0;

    // 6. Kiểm tra các rơ le bảo vệ động cơ PKE
    Is_Motors_Healthy := NOT DB58.Motor_Current_Trip;

    // 7. Tổng hợp cờ Freigabe M61.0
    M61.0 := Is_EStop_Healthy AND Is_Oil_Level_Healthy AND Is_Oil_Temp_Healthy 
             AND Is_Filter_Healthy AND Is_Fieldbus_Healthy AND Is_Motors_Healthy;

    // 8. Tác động an toàn: Nếu M61.0 bị mất, cưỡng bức ngắt toàn bộ van chấp hành
    IF NOT M61.0 THEN
        Q16.2 := FALSE; // Dừng kích tiến trái
        Q17.2 := FALSE; // Dừng kích tiến phải
        Q16.1 := FALSE; // Dừng Dehner 1
        Q16.3 := FALSE; // Dừng Dehner 2
        Q17.1 := FALSE; // Dừng Dehner 3
        M20.1 := FALSE; // Dừng quay thuận mâm cắt
        M20.2 := FALSE; // Dừng quay nghịch mâm cắt
        Q48.0 := FALSE; // Ngắt bơm nước cao áp 400 bar
        Q51.2 := FALSE; // Đóng béc phun 1
        Q51.3 := FALSE; // Đóng béc phun 2
        Q51.6 := FALSE; // Đóng béc phun 3
        Q51.7 := FALSE; // Đóng béc phun 4
    END_IF;
END_FUNCTION

FUNCTION FC71_Fault_Management : VOID
TITLE = 'Quản Lý Còi Báo Sự Cố & Xóa Lỗi Khi Đã Khắc Phục'
BEGIN
    // Bật còi / đèn báo động khi có bất kỳ sự cố nào
    IF DB58.EStop_Pushed OR DB58.Profibus_Loss_Slave OR DB58.Hydr_Oil_Level_Low 
       OR DB58.Hydr_Oil_Temp_High OR DB58.Main_Filter_Clogged OR DB58.Motor_Current_Trip 
       OR DB58.HD_Water_No_Flow OR DB58.Cutter_Stall_Torque OR DB58.Shield_Roll_Excess THEN
        DB58.Master_Fault_Trip := TRUE;
        Q75.0 := TRUE; // Ngõ ra còi hú / đèn chớp vàng (Horn / Alarm)
    END_IF;

    // Xóa cờ lỗi khi thợ vận hành ấn nút Reset (I 60.0 hoặc HMI) và các điều kiện an toàn đã OK
    IF (I60.0 OR DB19.Btn_Reset_Fault) AND M61.0 THEN
        DB58.Master_Fault_Trip := FALSE;
        Q75.0 := FALSE; // Tắt còi báo
    END_IF;
END_FUNCTION
```

---

### Module 2: Trạm Nguồn Thủy Lực & Động Cơ Chính (FC13, FC30, FC33)

#### 🎯 Mục đích công nghệ:
Cung cấp lưu lượng và áp suất dầu thủy lực cho toàn bộ máy. Động cơ bơm chính 110kW/132kW kéo cụm bơm nguồn, khởi động qua mạch Sao-Tam giác hoặc bộ khởi động mềm điện tử.

#### 🔌 Thiết bị liên quan:
* `I 61.1 / I 61.0`: Nút bấm Start / Stop bơm chính trên tủ điện Container.
* `I 1.1`: Tiếp điểm phụ rơ le nhiệt bảo vệ quá tải động cơ.
* `Q 1.0`: Cuộn hút contactor động lực khởi động động cơ chính (`MainJackMot_K`).
* `PIW 308`: Cảm biến đo chênh áp lọc đường nạp dầu (`Feedingfilter`).

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC13_Main_Motor_Control : VOID
TITLE = 'Điều Khiển Động Cơ Bơm Thủy Lực Chính 110kW/132kW'
BEGIN
    // Điều kiện khởi động: Có liên động an toàn M61.0, có lệnh Start, không bị ấn Stop, rơ le nhiệt không nhảy
    IF M61.0 AND (I61.1 OR DB19.Btn_Mode_Auto) AND (NOT I61.0) AND (NOT I1.1) THEN
        Q1.0 := TRUE; // Đóng khởi động từ cấp nguồn động cơ chính
        Q71.0 := TRUE;// Bật đèn báo Động cơ chính đang chạy (JacksMotOn_H)
    ELSIF I61.0 OR (NOT M61.0) OR I1.1 THEN
        Q1.0 := FALSE;
        Q71.0 := FALSE;
    END_IF;

    // Báo đèn lỗi động cơ chính nếu rơ le nhiệt nhảy (I 1.1)
    Q72.0 := I1.1; // JacksMotFault_H
END_FUNCTION

FUNCTION FC33_Filter_Monitoring : VOID
TITLE = 'Giám Sát Độ Chênh Áp Lọc Dầu Thủy Lực Tránh Nổ Bể Lọc'
BEGIN
    // Đọc cảm biến áp suất chênh lệch qua phin lọc nạp PIW 308 (0 - 10 bar)
    // Nếu áp lực chênh lệch > 4.5 bar (Raw ADC > 12441) nghĩa là lưới lọc bị cặn bẩn bít kín
    IF PIW308 > 12441 THEN
        DB58.Main_Filter_Clogged := TRUE;
    ELSE
        DB58.Main_Filter_Clogged := FALSE;
    END_IF;
END_FUNCTION
```

---

### Module 3: Đầu Cắt Schürfrad & Điều Tốc Bơm Rexroth A4VG (FC14, FC25, FC26, FC5, FC140)

#### 🎯 Mục đích công nghệ:
Điều khiển đĩa cắt xoay tròn để đào đất. Đĩa cắt có thể quay thuận (CW) hoặc quay nghịch (CCW) để chống kẹt và cân bằng lực xoắn. Tốc độ quay được điều chỉnh vô cấp từ 0 đến 6.0 RPM bằng cách xuất điện áp `0–10V` tới van tỷ lệ `PQW 268` điều chỉnh góc nghiêng đĩa bơm thủy lực Rexroth A4VG 250cc.

#### 🔌 Thiết bị liên quan:
* `I 90.1 / I 90.2`: Công tắc chọn chiều quay Thuận (CW) / Nghịch (CCW).
* `PIW 378`: Cảm biến áp suất đĩa cắt trên đầu khiên (0–350 bar).
* `PQW 268`: Ngõ ra Analog 0–10V điều khiển card khuếch đại góc nghiêng đĩa bơm.
* `Q 90.0 / Q 92.0`: Đèn báo trạng thái quay phải / quay trái.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC25_Cutter_Direction : VOID
TITLE = 'Điều Khiển Chiều Quay Thuận (CW) / Nghịch (CCW) Mâm Cắt Schürfrad'
BEGIN
    // 1. Chiều Quay Thuận Phải (CW): Khóa chéo với lệnh quay trái (M 20.2)
    IF M61.0 AND (I90.1 OR DB19.Btn_Cutter_CW) AND (NOT M20.2) THEN
        M20.1 := TRUE; // Cờ Cutter_CW
    ELSIF (NOT M61.0) OR I61.0 OR (NOT I90.1 AND NOT DB19.Btn_Cutter_CW) THEN
        M20.1 := FALSE;
    END_IF;

    // 2. Chiều Quay Nghịch Trái (CCW): Khóa chéo với lệnh quay phải (M 20.1)
    IF M61.0 AND (I90.2 OR DB19.Btn_Cutter_CCW) AND (NOT M20.1) THEN
        M20.2 := TRUE; // Cờ Cutter_CCW
    ELSIF (NOT M61.0) OR I61.0 OR (NOT I90.2 AND NOT DB19.Btn_Cutter_CCW) THEN
        M20.2 := FALSE;
    END_IF;

    // Xuất tín hiệu đèn báo trên bàn điều khiển thợ lái
    Q90.0 := M20.1; // Đèn báo Quay phải (CutterRight_H)
    Q92.0 := M20.2; // Đèn báo Quay trái (CutterLeft_H)
END_FUNCTION

FUNCTION FC26_Cutter_Speed_Scale : VOID
TITLE = 'Chuẩn Hóa Chiết Áp HMI Xuất Tín Hiệu Điện Áp Van Tỷ Lệ Đĩa Bơm PQW 268'
VAR_TEMP
    Target_Percent : REAL;
    Analog_Out_Int : INT;
    Cutter_Pres_bar: REAL;
END_VAR

BEGIN
    // Đọc áp lực trực tiếp từ đĩa cắt qua cảm biến PIW 378 (0 - 350 bar)
    Cutter_Pres_bar := (INT_TO_REAL(PIW378) / 27648.0) * 350.0;
    DB16.Cutter_Pres_Head := Cutter_Pres_bar;

    IF M20.1 OR M20.2 THEN
        Target_Percent := DB19.Slider_Cutter_Speed; // 0.0 - 100.0 %

        // Giới hạn trong khoảng 0 - 100%
        IF Target_Percent > 100.0 THEN Target_Percent := 100.0; END_IF;
        IF Target_Percent < 0.0   THEN Target_Percent := 0.0;   END_IF;

        // Quy đổi sang thang số nguyên DAC của PLC (0 - 27648 tương ứng 0 - 10V)
        Analog_Out_Int := REAL_TO_INT((Target_Percent / 100.0) * 27648.0);

        // THUẬT TOÁN TỰ ĐỘNG CHỐNG QUÁ TẢI MÔ-MEN (ANTI-STALL TORQUE LIMITER):
        // Nếu áp lực đầu cắt vượt quá 300 bar (đất quá cứng hoặc kẹt đá), tự động giảm 30% tốc độ
        IF Cutter_Pres_bar > 300.0 THEN
            Analog_Out_Int := Analog_Out_Int - 8000;
            IF Analog_Out_Int < 4000 THEN Analog_Out_Int := 4000; END_IF;
            DB58.Cutter_Stall_Torque := TRUE;
        ELSE
            DB58.Cutter_Stall_Torque := FALSE;
        END_IF;

        // Ghi giá trị ra thanh ghi ngõ ra Analog PQW 268
        PQW268 := INT_TO_WORD(Analog_Out_Int);
    ELSE
        PQW268 := W#16#0000; // Khi dừng mâm cắt, xuất 0V để đĩa bơm về góc 0 độ
    END_IF;
END_FUNCTION

FUNCTION FC5_Pump_Characteristic_A4VG : VOID
TITLE = 'Tính Toán Đường Đặc Tính Bơm Thủy Lực Rexroth A4VG 250cc'
VAR_INPUT
    Speed_Setpoint_RPM : REAL; // Vòng quay đặt từ HMI (0 - 6.0 RPM)
END_VAR
VAR_OUTPUT
    Flow_Rate_Liters_Min: REAL;// Lưu lượng dầu thủy lực yêu cầu (L/min)
END_VAR

BEGIN
    // Công thức thủy lực: Q (L/min) = V_disp (cm3/rev) * n_motor (RPM) * eta / 1000
    // Với bơm Rexroth A4VG dung tích 250cc kéo bởi động cơ 1480 RPM qua hộp giảm tốc i=246
    Flow_Rate_Liters_Min := Speed_Setpoint_RPM * 246.0 * (250.0 / 1000.0) * 0.95;
END_FUNCTION
```

---

### Module 4: Trạm Kích Đẩy Chính & Tính Lực Tấn (FC20, FC21, FC22, DB17)

#### 🎯 Mục đích công nghệ:
Điều khiển 4 xi lanh kích đẩy chính tại đáy giếng để ép toàn bộ đoàn cống và đầu khiên tiến vào lòng đất. Đồng thời đọc cảm biến áp suất `PIW 406` để tự động tính ra tổng lực đẩy theo đơn vị **Tấn (Tons)** giúp người vận hành không kích quá tải gây vỡ nứt đốt cống bê tông.

#### 🔌 Thiết bị liên quan:
* `I 62.2 / I 62.3`: Nút ấn Kích Tiến (`Jacks_Adv`) / Kích Lùi (`Jacks_Ret`).
* `M 12.7`: Cảm biến xác nhận chốt khóa cơ khí bàn đẩy đã rút mở an toàn.
* `PIW 406`: Cảm biến áp suất dầu thủy lực trạm kích chính (0–400 bar).
* `PQW 264 / PQW 266`: Ngõ ra Analog 0–10V điều chỉnh lưu lượng (vận tốc kích) và áp lực nén.
* `Q 16.2 / Q 17.2`: Van điện từ kích tiến trái / phải.
* `Q 16.0 / Q 17.0`: Van điện từ kích lùi trái / phải.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC20_MainJacks_Control : VOID
TITLE = 'Điều Khiển Van Điện Từ Kích Tiến / Lùi Trạm Kích Chính'
BEGIN
    // 1. ĐIỀU KHIỂN KÍCH TIẾN (ADVANCE):
    // Điều kiện an toàn: Có Freigabe M61.0, Mâm cắt PHẢI ĐANG QUAY (M20.1 hoặc M20.2), Chốt khóa đã rút (M12.7)
    IF M61.0 AND (M20.1 OR M20.2) AND M12.7 AND (I62.2 OR DB19.Btn_Jacks_Advance) AND (NOT Q16.0) THEN
        Q16.2 := TRUE; // Van kích tiến bên trái (-K6.1)
        Q17.2 := TRUE; // Van kích tiến bên phải (-K7.1)
        Q89.0 := TRUE; // Đèn báo Kích đang tiến (JacksAdv_H)
    ELSE
        Q16.2 := FALSE;
        Q17.2 := FALSE;
        Q89.0 := FALSE;
    END_IF;

    // 2. ĐIỀU KHIỂN KÍCH LÙI (RETRACT):
    // Thu hồi xi lanh kích về vị trí ban đầu để đưa đốt cống mới vào giếng
    IF M61.0 AND M12.7 AND (I62.3 OR DB19.Btn_Jacks_Retract) AND (NOT Q16.2) THEN
        Q16.0 := TRUE; // Van kích lùi bên trái (-K6.2)
        Q17.0 := TRUE; // Van kích lùi bên phải (-K7.2)
        Q87.0 := TRUE; // Đèn báo Kích đang lùi (JacksRet_H)
    ELSE
        Q16.0 := FALSE;
        Q17.0 := FALSE;
        Q87.0 := FALSE;
    END_IF;

    // 3. XUẤT TÍN HIỆU ĐIỀU CHỈNH VẬN TỐC KÍCH ĐẨY (PQW 264):
    IF Q16.2 THEN
        PQW264 := INT_TO_WORD(REAL_TO_INT((DB19.Slider_Jacks_Speed / 100.0) * 27648.0));
    ELSE
        PQW264 := W#16#0000;
    END_IF;
END_FUNCTION

FUNCTION FC22_Bar_To_Tonnage_Calc : VOID
TITLE = 'Thuật Toán Quy Đổi Áp Suất Thủy Lực (Bar) Sang Tổng Lực Đẩy (Tấn)'
VAR_TEMP
    Jacks_Pres_bar : REAL;
END_VAR

BEGIN
    // Bước 1: Đọc tín hiệu 4-20mA từ cảm biến áp suất PIW 406 (0 - 400 bar)
    Jacks_Pres_bar := (INT_TO_REAL(PIW406) / 27648.0) * 400.0;
    IF Jacks_Pres_bar < 0.0 THEN Jacks_Pres_bar := 0.0; END_IF;
    DB16.Jacks_Pres_PP := Jacks_Pres_bar; // Lưu vào DB16

    // Bước 2: Tính lực kích theo công thức vật lý:
    // Lực F (kG) = Áp suất P (kG/cm2) * Tổng diện tích 4 Piston A (cm2)
    // Lực F (Tấn) = F (kG) / 1000 = P (bar) * 2454.3 cm2 / 1000 = P (bar) * 0.24543
    // Ví dụ: Khi áp suất kích đạt 200 bar -> Lực đẩy = 200 * 0.24543 = 490.86 Tấn
    DB17.Total_Jacking_Tons := DB16.Jacks_Pres_PP * DB17.Bar_To_Ton_Factor;
END_FUNCTION
```

---

### Module 5: Đồng Bộ Trạm Kích Trung Gian Dehner 1..5 (FC55, FC56, FC57, FC141)

#### 🎯 Mục đích công nghệ:
Khi tuyến hầm dài, lực ma sát đất rất lớn. Hệ thống kích trung gian chia chiều dài cống thành các đoạn độc lập. PLC sẽ điều khiển kích tuần tự: Dehner 1 đẩy đoạn đầu, sau đó Dehner 2 đẩy đoạn giữa, rồi Dehner 3 đẩy đoạn sau, và cuối cùng trạm kích chính đẩy toàn bộ.

#### 🔌 Thiết bị liên quan:
* `PIW 336 / 344 / 380`: Cảm biến hành trình đo độ mở rộng Dehner 1, 2, 3 (0–1200 mm).
* `PIW 338 / 346 / 382`: Cảm biến áp suất thủy lực Dehner 1, 2, 3 (0–400 bar).
* `Q 16.1 / Q 16.3 / Q 17.1`: Van điện từ kích mở các trạm Dehner 1, 2, 3.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC55_Dehner_Synchronizer : VOID
TITLE = 'Thuật Toán Phối Hợp Kích Tuần Hoàn Đồng Bộ 3 Trạm Kích Trung Gian'
BEGIN
    // Bước 1: Đọc và quy đổi hành trình (mm) và áp suất (bar) của 3 trạm Dehner
    DB16.Dehner1_Stroke_mm := (INT_TO_REAL(PIW336) / 27648.0) * 1200.0;
    DB16.Dehner1_Pres      := (INT_TO_REAL(PIW338) / 27648.0) * 400.0;

    DB16.Dehner2_Stroke_mm := (INT_TO_REAL(PIW344) / 27648.0) * 1200.0;
    DB16.Dehner2_Pres      := (INT_TO_REAL(PIW346) / 27648.0) * 400.0;

    DB16.Dehner3_Stroke_mm := (INT_TO_REAL(PIW380) / 27648.0) * 1200.0;
    DB16.Dehner3_Pres      := (INT_TO_REAL(PIW382) / 27648.0) * 400.0;

    // Bước 2: Chu trình kích tuần tự tự động (Automatic Step Sequence):
    IF M61.0 AND DB19.Btn_Mode_Auto THEN
        // GIAI ĐOẠN 1: Kích Dehner 1 đẩy đầu khiên tiến lên (Hành trình đạt 1100 mm)
        IF (DB16.Dehner1_Stroke_mm < 1100.0) AND (NOT Q16.3) AND (NOT Q17.1) THEN
            Q16.1 := TRUE; // Bật van Dehner 1 (-K10)
        ELSE
            Q16.1 := FALSE;
        END_IF;

        // GIAI ĐOẠN 2: Khi Dehner 1 đã đẩy xong, kích Dehner 2 đẩy đoạn cống giữa
        IF (DB16.Dehner1_Stroke_mm >= 1100.0) AND (DB16.Dehner2_Stroke_mm < 1100.0) AND (NOT Q17.1) THEN
            Q16.3 := TRUE; // Bật van Dehner 2 (-K11)
        ELSE
            Q16.3 := FALSE;
        END_IF;

        // GIAI ĐOẠN 3: Khi Dehner 2 đã đẩy xong, kích Dehner 3 đẩy đoạn cống sau
        IF (DB16.Dehner2_Stroke_mm >= 1100.0) AND (DB16.Dehner3_Stroke_mm < 1100.0) THEN
            Q17.1 := TRUE; // Bật van Dehner 3 (-K12)
        ELSE
            Q17.1 := FALSE;
        END_IF;
    ELSE
        // Chế độ Thủ công: Tắt van tự động
        IF NOT DB19.Btn_Mode_Auto THEN
            Q16.1 := FALSE;
            Q16.3 := FALSE;
            Q17.1 := FALSE;
        END_IF;
    END_IF;
END_FUNCTION
```

---

### Module 6: Hệ Thống Bẻ Lái 3 Xi Lanh 120° & Cánh Chống Xoay (FC40, FC41, FC42, FC43)

#### 🎯 Mục đích công nghệ:
* **Bẻ lái 3 trục 120°:** 3 xi lanh bố trí tạo thành hình tam giác đều $120^\circ$. Bằng cách phối hợp độ thò thụt của 3 xi lanh, đầu khiên có thể uốn cong hướng đào theo bất kỳ vector không gian 3D nào.
* **Cánh chống xoay (Wing):** Khi mô-men xoắn đầu cắt làm thân khiên xoay nghiêng quá $\pm 1.5^\circ$, PLC tự động kích hoạt van bung cánh thép tì vào vách đất để giữ thân khiên thăng bằng.

#### 🔌 Thiết bị liên quan:
* `PIW 356 / 358 / 360`: Cảm biến đo hành trình 3 xi lanh lái (0–200 mm).
* `PIW 364 / 366 / 368`: Cảm biến đo áp suất 3 xi lanh lái (0–350 bar).
* `Q 53.0 .. Q 53.5`: 6 van điện từ điều khiển thò/thụt 3 xi lanh lái.
* `Q 51.4 / Q 51.5`: Van bung / thu cánh chống xoay khiên đào.
* `Q 5.0`: Động cơ bơm dầu thủy lực mạch bẻ lái.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC40_Steering_Hydraulics : VOID
TITLE = 'Điều Khiển 3 Cụm Xi Lanh Bẻ Khớp Lái Bố Trí Lệch 120 Độ'
BEGIN
    // Đọc hành trình đo được của 3 xi lanh lái từ cảm biến LVDT (PIW 356, 358, 360)
    DB16.Cyl1_Stroke_mm := (INT_TO_REAL(PIW356) / 27648.0) * 200.0;
    DB16.Cyl2_Stroke_mm := (INT_TO_REAL(PIW358) / 27648.0) * 200.0;
    DB16.Cyl3_Stroke_mm := (INT_TO_REAL(PIW360) / 27648.0) * 200.0;

    // Đọc áp lực tì nén của 3 xi lanh lái (PIW 364, 366, 368)
    DB16.Cyl1_Pres := (INT_TO_REAL(PIW364) / 27648.0) * 350.0;
    DB16.Cyl2_Pres := (INT_TO_REAL(PIW366) / 27648.0) * 350.0;
    DB16.Cyl3_Pres := (INT_TO_REAL(PIW368) / 27648.0) * 350.0;

    // Bơm dầu lái hướng Q 5.0 phải chạy khi có Freigabe M61.0
    IF M61.0 THEN
        Q5.0 := TRUE; // Khởi động động cơ bơm bẻ lái

        // Điều khiển Xi lanh 1 (Đỉnh trên): Thò ra (Q53.0) / Thụt vào (Q53.1)
        IF (DB19.Slider_Steer_Angle > 1.0) AND (DB16.Cyl1_Stroke_mm < 190.0) THEN
            Q53.0 := TRUE;  // ValveCyl1_out (Đẩy xi lanh 1 ra)
            Q53.1 := FALSE;
        ELSIF (DB19.Slider_Steer_Angle < -1.0) AND (DB16.Cyl1_Stroke_mm > 10.0) THEN
            Q53.0 := FALSE;
            Q53.1 := TRUE;  // ValveCyl1_in (Thu xi lanh 1 vào)
        ELSE
            Q53.0 := FALSE;
            Q53.1 := FALSE;
        END_IF;
    ELSE
        Q5.0  := FALSE;
        Q53.0 := FALSE;
        Q53.1 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC43_AntiRoll_Wing_Control : VOID
TITLE = 'Thuật Toán Tự Động Bung Cánh Ghim Vách Đất Chống Xoay Khiên Đào'
BEGIN
    // Nếu góc xoắn Roll vượt quá ngưỡng an toàn +-1.5 độ hoặc có lệnh cưỡng bức từ HMI
    IF (ABS(DB16.Roll_Angle_Deg) > 1.5) OR I63.1 OR DB19.Btn_Wing_Extend THEN
        Q51.4 := TRUE;  // ValveWing_Out: Kích van thủy lực bung cánh thép ghim vào đất
        Q51.5 := FALSE;
        DB58.Shield_Roll_Excess := TRUE; // Cảnh báo xoắn khiên
    ELSIF I63.3 OR DB19.Btn_Wing_Retract THEN
        Q51.4 := FALSE;
        Q51.5 := TRUE;  // ValveWing_In: Thu cánh thép về sát vỏ khiên
        DB58.Shield_Roll_Excess := FALSE;
    END_IF;
END_FUNCTION
```

---

### Module 7: Định Vị Laser TACS & Đo Độ Nghiêng Pitch/Roll (FC80, FC81, FC82)

#### 🎯 Mục đích công nghệ:
Giúp máy khoan đi đúng tim đường hầm với độ chính xác milimet.
* **Laser TACS:** Đo độ lệch tọa độ ngang $\Delta X$ và cao độ $\Delta Y$.
* **Inclinometer mts:** Đo góc dốc trục dọc (Pitch) và góc nghiêng lật ngang (Roll).

#### 🔌 Cảm biến liên quan:
* `PIW 370`: Cảm biến đo góc xoay Roll ($\pm 30.0^\circ$).
* `PIW 332`: Cảm biến đo góc dốc Pitch ($\pm 30.0^\circ$).

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC81_Inclinometer_Read : VOID
TITLE = 'Đọc Cảm Biến Nghiêng 2 Trục Inclinometer mts'
BEGIN
    // Chuẩn hóa cảm biến góc xoay Roll PIW 370 (Dải đo -30.0 đến +30.0 độ)
    // Tín hiệu Analog 0-27648 với điểm 0 độ ở mức giữa 13824 (0V / 12mA)
    DB16.Roll_Angle_Deg := ((INT_TO_REAL(PIW370) - 13824.0) / 13824.0) * 30.0;

    // Chuẩn hóa cảm biến góc dốc Pitch PIW 332 (Dải đo -30.0 đến +30.0 độ)
    DB16.Pitch_Angle_Deg := ((INT_TO_REAL(PIW332) - 13824.0) / 13824.0) * 30.0;
END_FUNCTION

FUNCTION FC80_TACS_Laser_Parser : VOID
TITLE = 'Giải Mã Tín Hiệu Tọa Độ Quang Điện Bia Ngắm Laser TACS'
VAR_INPUT
    Raw_Laser_X : INT; // Tín hiệu RS485 trục ngang X
    Raw_Laser_Y : INT; // Tín hiệu RS485 trục dọc Y
END_VAR
VAR_OUTPUT
    Deviation_X_mm : REAL; // Độ lệch tâm ngang (mm)
    Deviation_Y_mm : REAL; // Độ lệch tâm đứng (mm)
END_VAR

BEGIN
    // Độ phân giải 0.1 mm/LSB
    Deviation_X_mm := INT_TO_REAL(Raw_Laser_X) * 0.1;
    Deviation_Y_mm := INT_TO_REAL(Raw_Laser_Y) * 0.1;
END_FUNCTION
```

---

### Module 8: Tuần Hoàn Bùn Bentonite & Biến Tần Schneider (FC10, FC11, FC12, FC112)

#### 🎯 Mục đích công nghệ:
Vận chuyển mùn khoan đào ra khỏi hầm. Bơm cấp bùn (Charge Pump) đẩy dung dịch bentonite sạch vào buồng đào, bơm hút thải (Discharge Pump) hút bùn lẫn đất cát về trạm sàng tách trên mặt đất. Biến tần Schneider Altivar điều khiển tốc độ bơm qua mạng truyền thông Profibus-DP.

#### 🔌 Thiết bị liên quan:
* `PIW 374`: Cảm biến áp lực buồng đào (0–10 bar).
* `PIW 376`: Cảm biến áp suất đường ống bùn xả (0–10 bar).
* `PIW 420 / 422`: Đồng hồ đo lưu lượng bùn nạp và bùn xả (0–500 m³/h).
* `DB112`: Khối dữ liệu thanh ghi truyền thông Profibus với biến tần Altivar.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC10_Slurry_Charge_Pump : VOID
TITLE = 'Điều Khiển Bơm Cấp Dung Dịch Bùn Bentonite Vào Buồng Đào'
BEGIN
    // Đọc cảm biến áp lực buồng đào PIW 374 (0 - 10 bar)
    DB16.Slurry_Chamber_bar := (INT_TO_REAL(PIW374) / 27648.0) * 10.0;

    // Bơm cấp bùn chạy khi có Freigabe M61.0 và áp lực buồng đào chưa vượt mức an toàn (< 4.5 bar)
    IF M61.0 AND (I50.0 OR DB19.Btn_Mode_Auto) AND (NOT I50.1) AND (DB16.Slurry_Chamber_bar < 4.5) THEN
        M10.1 := TRUE;  // Cờ Charge_Pump_Running
        Q73.0 := TRUE;  // Đèn báo Bơm cấp bùn đang chạy
        DB58.Slurry_Pressure_High := FALSE;
    ELSE
        M10.1 := FALSE;
        Q73.0 := FALSE;
        IF DB16.Slurry_Chamber_bar >= 4.5 THEN
            DB58.Slurry_Pressure_High := TRUE; // Cảnh báo quá áp buồng đào
        END_IF;
    END_IF;
END_FUNCTION

FUNCTION FC11_Slurry_Discharge_Pump : VOID
TITLE = 'Điều Khiển Bơm Hút Thải Bùn Về Trạm Tách Cát'
BEGIN
    // Đọc cảm biến áp suất đường ống xả PIW 376 (0 - 10 bar)
    DB16.Slurry_Line_bar := (INT_TO_REAL(PIW376) / 27648.0) * 10.0;

    // Bơm xả chạy khi áp suất ống xả chưa bị nghẹt (< 6.0 bar)
    IF M61.0 AND (I50.2 OR DB19.Btn_Mode_Auto) AND (NOT I50.3) AND (DB16.Slurry_Line_bar < 6.0) THEN
        M11.1 := TRUE;  // Cờ Discharge_Pump_Running
        Q76.0 := TRUE;  // Đèn báo Bơm thải bùn đang chạy
    ELSE
        M11.1 := FALSE;
        Q76.0 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC112_Altivar_VFD_Control : VOID
TITLE = 'Truyền Thông Số Giao Tiếp Biến Tần Schneider Altivar Qua Profibus'
BEGIN
    IF M10.1 THEN
        DB112.Control_Word   := 16#000F; // Lệnh RUN biến tần Altivar (000F hex)
        DB112.Target_Freq_Hz := 500;     // Đặt tần số chạy định mức 50.0 Hz
    ELSE
        DB112.Control_Word   := 16#0000; // Lệnh STOP biến tần
        DB112.Target_Freq_Hz := 0;
    END_IF;
END_FUNCTION
```

---

### Module 9: Bơm Nước Cao Áp 400 bar & Béc Phun Rửa Đĩa Cắt (FC90, FC91)

#### 🎯 Mục đích công nghệ:
Khi đào qua tầng đất sét dẻo quánh, đất sẽ dính bết bịt kín các khe hở và răng đĩa cắt (hiện tượng "Clogging"). Bơm piston tạo tia nước áp lực cực cao 400 bar phun qua 4 béc phun mặt gương để xối rửa, cắt vụn đất sét ra từng mảnh nhỏ hòa vào dòng bùn bentonite.

#### 🔌 Thiết bị liên quan:
* `PIW 256`: Cảm biến áp suất bơm nước cao áp (0–400 bar).
* `I 16.0`: Cảm biến dòng chảy nước nguồn (Flow Switch - tiếp điểm NC mở khi mất nước).
* `I 16.1`: Rơ le nhiệt bảo vệ động cơ bơm cao áp.
* `Q 48.0`: Khởi động động cơ bơm cao áp qua mạch Sao-Tam giác (`HD-Pump on_K`).
* `Q 51.2, Q 51.3, Q 51.6, Q 51.7`: 4 van điện từ đóng mở 4 cụm béc phun cao áp (`HD_Valve_1..4`).

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC90_HighPressure_Pump : VOID
TITLE = 'Điều Khiển Khởi Động Động Cơ Bơm Nước Piston Cao Áp 400 bar'
BEGIN
    // Đọc cảm biến áp suất nước cao áp PIW 256 (0 - 400 bar)
    DB16.HD_Water_Pres_bar := (INT_TO_REAL(PIW256) / 27648.0) * 400.0;

    // Khởi động bơm khi có Freigabe M61.0, có nước cấp mồi (I 16.0 = 0 là có nước) và rơ le nhiệt không nhảy (I 16.1 = 0)
    IF M61.0 AND (M55.3 OR DB19.Btn_Water_Jet_On) AND (NOT I16.0) AND (NOT I16.1) THEN
        Q48.0 := TRUE; // Đóng khởi động từ Sao-Tam giác bơm cao áp
        M55.1 := TRUE; // Cờ HD_Pump_Running
        DB58.HD_Water_No_Flow := FALSE;
    ELSE
        Q48.0 := FALSE;
        M55.1 := FALSE;
        IF I16.0 THEN
            DB58.HD_Water_No_Flow := TRUE; // Báo lỗi mất nước cấp bảo vệ phớt bơm piston
        END_IF;
    END_IF;
END_FUNCTION

FUNCTION FC91_HD_Nozzles_Control : VOID
TITLE = 'Điều Khiển Đóng Mở 4 Cụm Van Béc Phun Tia Nước 400 bar Mặt Gương'
BEGIN
    // Mở 4 cụm béc phun khi áp suất nước đã đạt ngưỡng cao (> 250 bar) và có lệnh phun từ thợ vận hành
    IF M55.1 AND (DB16.HD_Water_Pres_bar > 250.0) AND (I52.2 OR DB19.Btn_Water_Jet_On) THEN
        Q51.2 := TRUE; // Mở béc phun số 1 (HD_Valve_1)
        Q51.3 := TRUE; // Mở béc phun số 2 (HD_Valve_2)
        Q51.6 := TRUE; // Mở béc phun số 3 (HD_Valve_3)
        Q51.7 := TRUE; // Mở béc phun số 4 (HD_Valve_4)
    ELSE
        Q51.2 := FALSE;
        Q51.3 := FALSE;
        Q51.6 := FALSE;
        Q51.7 := FALSE;
    END_IF;
END_FUNCTION
```

---

### Module 10: Đo Chiều Dài Hầm Đào (FC35 Distance Pulse)

#### 🎯 Mục đích công nghệ:
Một bánh xe thép tì trực tiếp vào thân cống bê tông tại giếng kích, khi cống chuyển động bánh xe quay làm encoder phát xung về ngõ vào số tốc độ cao `I 0.0` để đo chiều dài đường hầm đã hoàn thành (`Längenvortrieb`).

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC35_Distance_Measurement : VOID
TITLE = 'Đếm Xung Bánh Xe Encoder Đo Quãng Đường Kích Đẩy Hầm'
VAR
    Last_Pulse_Bit : BOOL;
END_VAR

BEGIN
    // Bắt sườn xung lên (Rising Edge Detection):
    IF I0.0 AND (NOT Last_Pulse_Bit) THEN
        // Mỗi xung encoder tương ứng dịch chuyển 1 mm
        DB1.DBD0 := DB1.DBD0 + 1; // Tăng biến tổng quãng đường (DINT, đơn vị mm)
    END_IF;
    Last_Pulse_Bit := I0.0;
END_FUNCTION
```

---

### Module 11: Mạng Cáp Dẹt SmartWire-DT Eaton PKE & NZM (FC110, FC111)

#### 🎯 Mục đích công nghệ:
Thay vì phải đi hàng trăm sợi dây điện từ tủ điều khiển đến từng rơ le nhiệt, công nghệ SmartWire-DT kết nối tất cả các bộ khởi động động cơ điện tử Eaton PKE và aptomat khối NZM bằng 1 sợi cáp dẹt duy nhất qua Gateway `EU5C-SWD-DP`.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC110_SmartWire_PKE_Read : VOID
TITLE = 'Đọc Dữ Liệu Dòng Điện Tải Từ Rơ Le Điện Tử Eaton PKE Qua Mạng SmartWire'
BEGIN
    IF M76.0 THEN // Khi mạng Profibus DP hoạt động ổn định
        // Đọc giá trị dòng điện thực tế động cơ cắt (A) lưu trong DB31
        // Nếu dòng điện vượt quá 115% dòng định mức (165 Ampe) liên tục -> Báo lỗi quá tải
        IF DB31.DBD0 > 165.0 THEN
            DB58.Motor_Current_Trip := TRUE;
        ELSE
            DB58.Motor_Current_Trip := FALSE;
        END_IF;
    END_IF;
END_FUNCTION
```

---

### Module 12: Giao Tiếp HMI & Nhật Ký Khoan SCADA (DB19, DB57, FC126)

#### 🎯 Mục đích công nghệ:
* **HMI Touchscreen (DB19):** Cập nhật các nút bấm điều khiển, hiển thị số liệu đo lường theo thời gian thực cho thợ lái máy trong cabin hầm.
* **SCADA DataLogging (DB57):** Cứ mỗi 1 giây, PLC tự động chụp một bức ảnh toàn cảnh dữ liệu (Snapshot) gồm quãng đường, vận tốc, áp suất, lực tấn, góc nghiêng gửi về máy chủ SCADA trên mặt đất để lưu trữ phục vụ lập báo cáo và nghiệm thu công trình.

#### 💡 Mã nguồn SCL:
```pascal
FUNCTION FC57_SCADA_DataLogging : VOID
TITLE = 'Ghi Nhật Ký Toàn Bộ Thông Số Khoan Định Kỳ 1 Giây'
VAR
    Last_Clock_1Hz : BOOL;
END_VAR

BEGIN
    // Sử dụng bit xung nhịp 1Hz tích hợp sẵn của CPU S7-300 (M 0.5)
    IF M0.5 AND (NOT Last_Clock_1Hz) THEN
        // Sao chép tức thời toàn bộ dữ liệu đo lường sang khối ghi nhật ký DB57:
        DB57.DBD0  := DB1.DBD0;                 // 1. Quãng đường đào tổng cộng (mm)
        DB57.DBD4  := DB102.DBD2;               // 2. Vận tốc kích đẩy sau làm mịn (mm/min)
        DB57.DBD8  := DB16.Jacks_Pres_PP;       // 3. Áp suất trạm kích chính (bar)
        DB57.DBD12 := DB17.Total_Jacking_Tons;  // 4. Tổng lực kích đẩy thực tế (Tấn)
        DB57.DBD16 := DB16.Cutter_Pres_Head;    // 5. Áp suất thủy lực đĩa cắt (bar)
        DB57.DBD20 := DB16.Roll_Angle_Deg;      // 6. Góc xoay nghiêng thân khiên (Deg)
        DB57.DBD24 := DB16.Pitch_Angle_Deg;     // 7. Góc dốc trục dọc thân khiên (Deg)
        DB57.DBD28 := DB16.Slurry_Chamber_bar;  // 8. Áp lực buồng đào bùn mặt gương (bar)
    END_IF;
    Last_Clock_1Hz := M0.5;
END_FUNCTION
```

---

## 7. BẢNG TỔNG KẾT TRA CỨU NHANH CHO HỌC VIÊN

| Tên Khối Hàm | Địa Chỉ | Khối Dữ Liệu (DB) | Chức Năng Cốt Lõi Trong Hệ Thống TBM |
|:---|:---|:---|:---|
| `OB100_Restart` | `OB100` | Toàn cục | Khởi tạo mặc định và reset cờ lỗi khi bật nguồn |
| `OB86_FaultHandler` | `OB86` | Toàn cục | Chống sập CPU khi mất kết nối trạm I/O Profibus-DP |
| `FB100_SpeedFilter` | `FB100` | `DB101, DB102` | Lọc số trung bình trượt 10 điểm làm mịn vận tốc kích |
| `OB32_CyclicInt` | `OB32` | `DB101` | Ngắt thời gian thực 100ms gọi FB100 |
| `OB1_MainScan` | `OB1` | Toàn cục | Vòng quét chính thực thi tuần tự 14 Networks |
| `FC61_Freigaben` | `FC61` | `DB58` | Mạch liên động kiểm tra an toàn tổng & Dừng khẩn cấp |
| `FC71_FaultManager` | `FC71, 72`| `DB58` | Quản lý còi báo sự cố và nút ấn xóa lỗi |
| `FC13_MainMotor` | `FC13, 30`| `DB31` | Điều khiển động cơ bơm thủy lực chính 132kW |
| `FC33_FilterMonitor`| `FC33` | `DB58` | Giám sát độ chênh áp cảnh báo nghẹt lọc dầu |
| `FC25_CutterDir` | `FC25` | `DB21` | Điều khiển đảo chiều mâm cắt Thuận (CW) / Nghịch (CCW) |
| `FC26_CutterSpeed` | `FC26` | `PQW 268` | Chuẩn hóa van tỷ lệ đĩa bơm & giảm tốc chống kẹt |
| `FC5_Characteristic`| `FC5, 140`| `DB21` | Đường đặc tính lưu lượng bơm Rexroth A4VG 250cc |
| `FC20_MainJacks` | `FC20` | `PQW 264, 266` | Điều khiển van kích tiến/lùi và xuất áp van tỷ lệ |
| `FC22_BarToTonnage` | `FC22` | `DB16, DB17` | Quy đổi áp suất Bar ra Tổng lực kích đẩy Tấn |
| `FC55_DehnerSync` | `FC55..57`| `DB16` | Thuật toán đồng bộ chu trình 5 trạm kích trung gian |
| `FC40_Steering` | `FC40..42`| `DB59` | Điều khiển 6 van thủy lực thò/thụt 3 xi lanh lái 120° |
| `FC43_AntiRollWing` | `FC43` | `Q 51.4` | Tự động bung cánh thép chống xoay khiên khi Roll > $\pm 1.5^\circ$ |
| `FC81_Inclinometer` | `FC81` | `DB16, DB59` | Đọc cảm biến đo góc dốc Pitch và góc xoay Roll |
| `FC80_TACS_Laser` | `FC80` | `DB19` | Giải mã tọa độ quang học tâm gương laser TACS |
| `FC10_SlurryCharge` | `FC10, 11`| `DB112` | Điều khiển bơm cấp/xả bùn và biến tần Schneider |
| `FC90_HPPump` | `FC90, 91`| `Q 48.0, Q 51.2..7`| Bơm cao áp và mở 4 cụm béc phun tia nước 400 bar |
| `FC35_DistancePulse`| `FC35` | `DB1` | Đếm xung bánh xe đo chiều dài hầm Längenvortrieb |
| `FC110_SmartWire` | `FC110, 111`| `DB31, DB33` | Đọc dòng điện tải từ rơ le động cơ điện tử Eaton PKE |
| `FC57_SCADA_Log` | `FC57` | `DB57` | Ghi snapshot thông số khoan định kỳ 1 giây |

---

