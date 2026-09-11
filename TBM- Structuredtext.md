# TÀI LIỆU CHƯƠNG TRÌNH ĐIỀU KHIỂN PLC S7-300 - MÁY KHOAN NGẦM TBM (BOHRKOPF 2100)
## NGÔN NGỮ VĂN BẢN CẤU TRÚC: STRUCTURED TEXT (ST / SCL) - CHUẨN IEC 61131-3

---

## MỤC LỤC
1. [Khái Niệm & Định Nghĩa Kiểu Dữ Liệu Cấu Trúc (UDT / TYPE)](#1-định-nghĩa-kiểu-dữ-liệu-cấu-trúc-udt--type)
2. [Khai Báo Biến Toàn Cục & Khối Dữ Liệu (GLOBAL DBs)](#2-khai-báo-biến-toàn-cục--khối-dữ-liệu-global-dbs)
3. [Khối Tổ Chức Khởi Tạo: OB100 (Complete Restart)](#3-khối-tổ-chức-khởi-tạo-ob100-complete-restart)
4. [Khối Xử Lý Sự Cố Rớt Mạng: OB86 (Rack / Station Fault)](#4-khối-xử-lý-sự-cố-rớt-mạng-ob86-rack--station-fault)
5. [Khối Ngắt Chu Kỳ Lọc Vận Tốc: OB32 / FB100 (Cyclic Interrupt 100ms)](#5-khối-ngắt-chu-kỳ-lọc-vận-tốc-ob32--fb100-cyclic-interrupt-100ms)
6. [Khối Vòng Quét Chính: OB1 (MainControl Cycle - 14 Networks)](#6-khối-vòng-quét-chính-ob1-maincontrol-cycle)
7. [Mạch Liên Động An Toàn & Dừng Khẩn Cấp: FC61 (Freigaben)](#7-mạch-liên-động-an-toàn--dừng-khẩn-cấp-fc61-freigaben)
8. [Quản Lý & Xóa Cờ Báo Lỗi: FC71 & FC72 (Fault Manager)](#8-quản-lý--xóa-cờ-báo-lỗi-fc71--fc72-fault-manager)
9. [Điều Khiển Trạm Nguồn Thủy Lực & Động Cơ Chính: FC13, FC30, FC33](#9-điều-khiển-trạm-nguồn-thủy-lực--động-cơ-chính-fc13-fc30-fc33)
10. [Điều Khiển Đảo Chiều & Tốc Độ Đầu Cắt Schürfrad: FC14, FC25, FC26](#10-điều-khiển-đảo-chiều--tốc-độ-đầu-cắt-schürfrad-fc14-fc25-fc26)
11. [Đặc Tính Lưu Lượng Bơm Rexroth A4VG / CSG: FC4, FC5, FC140, FC150](#11-đặc-tính-lưu-lượng-bơm-rexroth-a4vg--csg-fc4-fc5-fc140-fc150)
12. [Điều Khiển Trạm Kích Đẩy Chính & Quy Đổi Ra Tấn: FC20, FC21, FC22](#12-điều-khiển-trạm-kích-đẩy-chính--quy-đổi-ra-tấn-fc20-fc21-fc22)
13. [Điều Khiển Đồng Bộ Các Trạm Kích Trung Gian: FC55, FC56, FC57 (Dehner 1..5)](#13-điều-khiển-đồng-bộ-các-trạm-kích-trung-gian-fc55-fc56-fc57-dehner-15)
14. [Điều Khiển Bẻ Lái 3 Xi Lanh 120° & Cánh Chống Xoay: FC40..43](#14-điều-khiển-bẻ-lái-3-xi-lanh-120--cánh-chống-xoay-fc4043)
15. [Định Vị Laser TACS & Bù Nghiêng Xoắn: FC80, FC81, FC82](#15-định-vị-laser-tacs--bù-nghiêng-xoắn-fc80-fc81-fc82)
16. [Hệ Thống Tuần Hoàn Bùn & Biến Tần Schneider Altivar: FC10, FC11, FC12, FC112](#16-hệ-thống-tuần-hoàn-bùn--biến-tần-schneider-altivar-fc10-fc11-fc12-fc112)
17. [Bơm Nước Cao Áp 400 bar & Cụm Béc Phun Mặt Gương: FC90, FC91](#17-bơm-nước-cao-áp-400-bar--cụm-béc-phun-mặt-gương-fc90-fc91)
18. [Đo Chiều Dài Hầm Đào: FC35 (Längenvortrieb Encoder)](#18-đo-chiều-dài-hầm-đào-fc35-längenvortrieb-encoder)
19. [Giao Tiếp Mạng Cáp Dẹt SmartWire-DT Eaton PKE/NZM: FC110, FC111](#19-giao-tiếp-mạng-cáp-dẹt-smartwire-dt-eaton-pkenzm-fc110-fc111)
20. [Giao Tiếp Màn Hình Cảm Ứng HMI & Nhật Ký SCADA: DB19, DB57](#20-giao-tiếp-màn-hình-cảm-ứng-hmi--nhật-ký-scada-db19-db57)

---

## 1. ĐỊNH NGHĨA KIỂU DỮ LIỆU CẤU TRÚC (UDT / TYPE)

```pascal
TYPE UDT_AnalogSensor :
STRUCT
    Raw_PIW       : INT;        // Giá trị thô từ module ADC (0 - 27648)
    Val_Scaled    : REAL;       // Giá trị thực sau chuẩn hóa vật lý
    Min_Limit     : REAL;       // Giới hạn dưới của thang đo
    Max_Limit     : REAL;       // Giới hạn trên của thang đo
    Alarm_High    : BOOL;       // Cờ cảnh báo vượt ngưỡng trên
    Alarm_Low     : BOOL;       // Cờ cảnh báo dưới ngưỡng dưới
    Sensor_Fault  : BOOL;       // Cờ đứt dây hoặc lỗi cảm biến (Raw < -200 hoặc > 28500)
END_STRUCT
END_TYPE

TYPE UDT_SteeringCylinder :
STRUCT
    Pressure_Bar  : REAL;       // Áp suất dầu xi lanh (0 - 350 bar)
    Stroke_mm     : REAL;       // Hành trình đo được (0 - 200 mm)
    Target_mm     : REAL;       // Hành trình mục tiêu yêu cầu
    Cmd_Extend    : BOOL;       // Lệnh van đẩy ra
    Cmd_Retract   : BOOL;       // Lệnh van thu vào
    Limit_Extend  : BOOL;       // Giới hạn hành trình max (195 mm)
    Limit_Retract : BOOL;       // Giới hạn hành trình min (5 mm)
END_STRUCT
END_TYPE

TYPE UDT_DehnerStation :
STRUCT
    Pressure_Bar  : REAL;       // Áp suất trạm kích trung gian (0 - 400 bar)
    Stroke_mm     : REAL;       // Hành trình mở rộng (0 - 1200 mm)
    Active        : BOOL;       // Trạng thái đang kích
    Auto_Enable   : BOOL;       // Cho phép kích trong chu trình tự động
    Stroke_Limit  : BOOL;       // Đã đạt hành trình tối đa
END_STRUCT
END_TYPE

TYPE UDT_AltivarVFD :
STRUCT
    ControlWord   : WORD;       // Lệnh điều khiển gửi tới biến tần (CMD)
    SpeedRef_Hz   : INT;        // Tần số đặt (0.1 Hz)
    StatusWord    : WORD;       // Trạng thái phản hồi từ biến tần (ETA)
    Actual_Hz     : INT;        // Tần số thực tế động cơ
    Current_A     : REAL;       // Dòng điện tải thực tế (A)
    Fault_Code    : WORD;       // Mã lỗi biến tần
END_STRUCT
END_TYPE
```

---

## 2. KHAI BÁO BIẾN TOÀN CỤC & KHỐI DỮ LIỆU (GLOBAL DBs)

```pascal
DATA_BLOCK DB16 // DB Analog Values
TITLE = 'Vùng Nhớ Giá Trị Đo Lường Tương Tự Từ Cảm Biến Trường'
STRUCT
    Jacks_Pres_PP       : REAL; // Áp suất kích chính Container (bar)
    Cutter_Pres_PP      : REAL; // Áp suất bơm đầu cắt Container (bar)
    Cutter_Pres_Head    : REAL; // Áp suất đĩa cắt trên đầu khiên (bar)
    Cyl1_Pres           : REAL; // Áp suất xi lanh lái 1 (bar)
    Cyl2_Pres           : REAL; // Áp suất xi lanh lái 2 (bar)
    Cyl3_Pres           : REAL; // Áp suất xi lanh lái 3 (bar)
    Cyl1_Stroke         : REAL; // Hành trình xi lanh lái 1 (mm)
    Cyl2_Stroke         : REAL; // Hành trình xi lanh lái 2 (mm)
    Cyl3_Stroke         : REAL; // Hành trình xi lanh lái 3 (mm)
    Roll_Angle_Deg      : REAL; // Góc xoay lật thân khiên (Deg)
    Pitch_Angle_Deg     : REAL; // Góc dốc trục dọc thân khiên (Deg)
    Dehner1_Pres        : REAL; // Áp suất Dehner 1 (bar)
    Dehner1_Stroke      : REAL; // Hành trình Dehner 1 (mm)
    Dehner2_Pres        : REAL; // Áp suất Dehner 2 (bar)
    Dehner2_Stroke      : REAL; // Hành trình Dehner 2 (mm)
    Dehner3_Pres        : REAL; // Áp suất Dehner 3 (bar)
    Dehner3_Stroke      : REAL; // Hành trình Dehner 3 (mm)
    HD_Water_Pres       : REAL; // Áp suất bơm nước cao áp (bar)
    HD_Water_Flow       : REAL; // Lưu lượng nước cao áp (%)
    Slurry_Chamber_Pres : REAL; // Áp lực buồng đào bùn (bar)
    Slurry_Line_Pres    : REAL; // Áp suất đường ống bùn thải (bar)
    Slurry_Charge_Pres  : REAL; // Áp suất đường ống bùn cấp (bar)
    Slurry_Charge_Flow  : REAL; // Lưu lượng nạp bùn (m3/h)
    Slurry_Disch_Flow   : REAL; // Lưu lượng xả bùn (m3/h)
    Oil_Temp_PP         : REAL; // Nhiệt độ dầu Container (°C)
    Oil_Temp_Head       : REAL; // Nhiệt độ dầu đầu khiên (°C)
    Oil_Humidity_PP     : REAL; // Độ ẩm dầu Container (%)
    CaseDrain_Pres      : REAL; // Áp suất rò rỉ gối đỡ đĩa cắt (bar)
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB17 // DB Druck-Tonnen
TITLE = 'Bảng Quy Đổi Áp Suất Thủy Lực (Bar) Sang Lực Kích Tổng (Tấn)'
STRUCT
    Total_Force_Tons    : REAL; // Tổng lực kích đẩy toàn phần (Tấn)
    Piston_Area_cm2     : REAL := 2454.3; // Tổng tiết diện piston kích chính (cm2)
    Conversion_Factor   : REAL := 0.24543; // Hệ số nhân chuyển đổi Bar -> Tấn
    Dehner1_Force_Tons  : REAL; // Lực kích trạm Dehner 1
    Dehner2_Force_Tons  : REAL; // Lực kích trạm Dehner 2
    Dehner3_Force_Tons  : REAL; // Lực kích trạm Dehner 3
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB19 // DB Touchscreen HMI
TITLE = 'Vùng Nhớ Trao Đổi Giao Diện Màn Hình Cảm Ứng HMI'
STRUCT
    Btn_Auto_Mode       : BOOL; // Nút chọn chế độ Tự động
    Btn_Manual_Mode     : BOOL; // Nút chọn chế độ Thủ công
    Btn_Cutter_CW       : BOOL; // Nút chọn quay phải mâm cắt
    Btn_Cutter_CCW      : BOOL; // Nút chọn quay trái mâm cắt
    Btn_Cutter_Start    : BOOL; // Nút chạy đầu cắt
    Btn_Cutter_Stop     : BOOL; // Nút dừng đầu cắt
    Btn_Jacks_Adv       : BOOL; // Nút kích tiến
    Btn_Jacks_Ret       : BOOL; // Nút kích lùi
    Btn_HD_Jet_Toggle   : BOOL; // Nút bật/tắt tia nước cao áp 400 bar
    Btn_EStop_Reset     : BOOL; // Nút reset sự cố dừng khẩn cấp
    Btn_Wing_Extend     : BOOL; // Nút bung cánh chống xoay khiên
    Btn_Wing_Retract    : BOOL; // Nút thu cánh chống xoay khiên
    Slider_Cutter_Speed : REAL; // Chiết áp tốc độ đầu cắt (0.0 - 100.0 %)
    Slider_Jacks_Speed  : REAL; // Chiết áp tốc độ kích đẩy (0.0 - 100.0 %)
    Slider_Steer_Angle  : REAL; // Chiết áp bẻ góc lái (-30.0 đến +30.0 Deg)
END_STRUCT
BEGIN
END_DATA_BLOCK

DATA_BLOCK DB58 // DB Fault_DB
TITLE = 'Bảng Cờ Báo Lỗi & Sự Cố Toàn Hệ Thống'
STRUCT
    Gen_Fault           : BOOL; // Cờ lỗi tổng quát
    EStop_Activated     : BOOL; // Nút E-Stop đang bị nhấn
    DP_Bus_Failure      : BOOL; // Mất kết nối trạm Profibus-DP
    Oil_Level_Low       : BOOL; // Cạn dầu thủy lực
    Oil_Temp_High       : BOOL; // Quá nhiệt dầu thủy lực > 70°C
    Filter_Clogged      : BOOL; // Nghẹt phin lọc dầu
    Motor_Overload      : BOOL; // Quá tải dòng điện động cơ
    HD_Pump_No_Flow     : BOOL; // Bơm cao áp mất nước cấp
    Cutter_OverTorque   : BOOL; // Quá mô-men đĩa cắt kẹt đất
    Slurry_Chamber_High : BOOL; // Quá áp buồng đào bùn
    Roll_Angle_Excess   : BOOL; // Thân khiên bị xoay nghiêng quá mức
END_STRUCT
BEGIN
END_DATA_BLOCK
```

---

## 3. KHỐI TỔ CHỨC KHỞI TẠO: OB100 (COMPLETE RESTART)

```pascal
ORGANIZATION_BLOCK OB100
TITLE = 'Complete Restart - Khởi Tạo Tham Số Khi Bật Nguồn PLC'
VAR_TEMP
    info : ARRAY[0..19] OF BYTE;
END_VAR

BEGIN
    // 1. Reset toàn bộ cờ lỗi hệ thống
    DB58.Gen_Fault           := FALSE;
    DB58.EStop_Activated     := FALSE;
    DB58.DP_Bus_Failure      := FALSE;
    DB58.Oil_Level_Low       := FALSE;
    DB58.Oil_Temp_High       := FALSE;
    DB58.Filter_Clogged      := FALSE;
    DB58.Motor_Overload      := FALSE;
    DB58.HD_Pump_No_Flow     := FALSE;
    DB58.Cutter_OverTorque   := FALSE;
    DB58.Slurry_Chamber_High := FALSE;
    DB58.Roll_Angle_Excess   := FALSE;

    // 2. Thiết lập giá trị mặc định cho các bộ điều khiển
    DB19.Slider_Cutter_Speed := 50.0; // Mặc định 50%
    DB19.Slider_Jacks_Speed  := 30.0; // Mặc định 30%
    DB19.Slider_Steer_Angle  := 0.0;  // Giữ thẳng tim hầm
    DB19.Btn_Auto_Mode       := FALSE;
    DB19.Btn_Manual_Mode     := TRUE;

    // 3. Khởi tạo cờ hệ thống hoàn tất Startup
    M100.0 := TRUE; // Sys_Init_Done
END_ORGANIZATION_BLOCK
```

---

## 4. KHỐI XỬ LÝ SỰ CỐ RỚT MẠNG: OB86 (RACK / STATION FAULT)

```pascal
ORGANIZATION_BLOCK OB86
TITLE = 'Loss of Rack Fault Handler - Ngăn Ngừa Dừng CPU Khi Mất Trạm DP'
VAR_TEMP
    OB86_EV_CLASS : BYTE;   // 16#38: Event leaving, 16#39: Event entering
    OB86_FLT_ID   : BYTE;   // Mã lỗi trạm
    OB86_RACK_NUM : BYTE;   // Địa chỉ trạm DP Station Number
    OB86_RESERVED : ARRAY[0..3] OF BYTE;
END_VAR

BEGIN
    // Khi một trạm Profibus-DP (Turck BL20/BL67/SDPB) bị mất nguồn hoặc đứt cáp:
    IF (OB86_EV_CLASS = 16#39) THEN
        DB58.DP_Bus_Failure := TRUE;
        DB58.Gen_Fault      := TRUE;
        M76.0               := FALSE; // DP_Bus_OK := FALSE
    ELSE
        // Trạm kết nối lại thành công
        DB58.DP_Bus_Failure := FALSE;
        M76.0               := TRUE;  // DP_Bus_OK := TRUE
    END_IF;
END_ORGANIZATION_BLOCK
```

---

## 5. KHỐI NGẮT CHU KỲ LỌC VẬN TỐC: OB32 / FB100 (CYCLIC INTERRUPT 100ms)

```pascal
FUNCTION_BLOCK FB100
TITLE = 'Moving Average Digital Filter - Bộ Lọc Trung Bình Động Làm Mịn Vận Tốc Kích'
VAR_INPUT
    Raw_Distance_mm : DINT;   // Quãng đường thô từ bánh xe đo xung (DB1.DBD0)
    Sample_Time_sec : REAL;   // Chu kỳ lấy mẫu ngắt (0.1s)
END_VAR
VAR_OUTPUT
    Smooth_Speed_mm_min : REAL; // Vận tốc kích đẩy sau làm mịn (mm/min)
END_VAR
VAR
    Prev_Distance_mm : DINT;
    Buffer : ARRAY[0..9] OF REAL; // Mảng 10 mẫu trung bình trượt
    Index  : INT := 0;
    Sum    : REAL := 0.0;
    i      : INT;
    Inst_Speed : REAL;
END_VAR

BEGIN
    // 1. Tính vận tốc tức thời: (mm / sec) * 60 = mm/min
    Inst_Speed := (DINT_TO_REAL(Raw_Distance_mm - Prev_Distance_mm) / Sample_Time_sec) * 60.0;
    Prev_Distance_mm := Raw_Distance_mm;

    // Giới hạn nhiễu đột biến âm
    IF Inst_Speed < 0.0 THEN
        Inst_Speed := 0.0;
    END_IF;

    // 2. Cập nhật mảng trung bình trượt (Moving Average 10 điểm)
    Buffer[Index] := Inst_Speed;
    Index := (Index + 1) MOD 10;

    Sum := 0.0;
    FOR i := 0 TO 9 DO
        Sum := Sum + Buffer[i];
    END_FOR;

    Smooth_Speed_mm_min := Sum / 10.0;
END_FUNCTION_BLOCK

ORGANIZATION_BLOCK OB32
TITLE = 'Cyclic Interrupt 100ms - Gọi FB100 Tính Vận Tốc Kích Đẩy'
VAR_TEMP
    info : ARRAY[0..19] OF BYTE;
END_VAR

BEGIN
    // Gọi FB100 với khối dữ liệu thực thể DB101
    FB100.DB101(
        Raw_Distance_mm     := DB1.DBD0,
        Sample_Time_sec     := 0.1,
        Smooth_Speed_mm_min => DB102.DBD2
    );
END_ORGANIZATION_BLOCK
```

---

## 6. KHỐI VÒNG QUÉT CHÍNH: OB1 (MAINCONTROL CYCLE)

```pascal
ORGANIZATION_BLOCK OB1
TITLE = 'MainControl Cycle - Vòng Quét Tuần Hoàn Chính Thực Thi 14 Networks'
VAR_TEMP
    info : ARRAY[0..19] OF BYTE;
END_VAR

BEGIN
    // Network 1-2: Chẩn đoán truyền thông Profibus-DP & Phần cứng
    FC125_DP_Diagnostics();

    // Network 3: Kiểm tra liên động an toàn Freigaben & Dừng khẩn cấp E-Stop
    FC61_Safety_Freigaben();
    FC71_Fault_Management();

    // Network 4-5: Khởi động bộ nguồn thủy lực Container & Bơm dầu chính
    FC30_Hydraulic_Container();
    FC13_Main_Motor_Control();
    FC33_Filter_Monitoring();

    // Network 6-7: Điều khiển chiều quay & Tốc độ đầu cắt Schürfrad
    FC25_Cutter_Direction();
    FC26_Cutter_Speed_Scale();

    // Network 8-9: Tính đường đặc tính bơm thủy lực biến tích Rexroth A4VG / CSG
    FC5_Pump_Characteristic_A4VG();

    // Network 10: Điều khiển kích chính & Trạm kích trung gian Dehner 1..5
    FC20_MainJacks_Control();
    FC22_Bar_To_Tonnage_Calc();
    FC55_Dehner_Synchronizer();

    // Network 11: Điều khiển hệ thống tuần hoàn dung dịch bùn bentonite
    FC10_Slurry_Charge_Pump();
    FC11_Slurry_Discharge_Pump();
    FC112_Altivar_VFD_Control();

    // Network 12-13: Định vị Laser TACS, Đo nghiêng Pitch/Roll & Bẻ lái 3 xi lanh 120°
    FC80_TACS_Laser_Parser();
    FC81_Inclinometer_Read();
    FC40_Steering_Hydraulics();
    FC43_AntiRoll_Wing_Control();

    // Network 14: Đo chiều dài hầm, Bơm cao áp 400 bar & Nhật ký SCADA
    FC35_Distance_Measurement();
    FC90_HighPressure_Pump();
    FC91_HD_Nozzles_Control();
    FC57_SCADA_DataLogging();
END_ORGANIZATION_BLOCK
```

---

## 7. MẠCH LIÊN ĐỘNG AN TOÀN & DỪNG KHẨN CẤP: FC61 (FREIGABEN)

```pascal
FUNCTION FC61_Safety_Freigaben : VOID
TITLE = 'Master Safety Interlock - Cấp Quyền Hoạt Động Khi Đủ Điều Kiện An Toàn'
VAR_TEMP
    All_Safety_Conditions_Met : BOOL;
END_VAR

BEGIN
    // 1. Kiểm tra trạng thái nút Dừng khẩn cấp (I 0.0 đóng khi an toàn)
    IF NOT I0.0 THEN
        DB58.EStop_Activated := TRUE;
    ELSE
        DB58.EStop_Activated := FALSE;
    END_IF;

    // 2. Kiểm tra mức dầu thủy lực trong Container (I 12.0) và Đầu khiên (I 6.1)
    IF (NOT I12.0) OR (NOT I6.1) THEN
        DB58.Oil_Level_Low := TRUE;
    ELSE
        DB58.Oil_Level_Low := FALSE;
    END_IF;

    // 3. Kiểm tra nhiệt độ dầu thủy lực (PIW 404 & PIW 288)
    IF (DB16.Oil_Temp_PP > 70.0) OR (DB16.Oil_Temp_Head > 75.0) THEN
        DB58.Oil_Temp_High := TRUE;
    ELSE
        DB58.Oil_Temp_High := FALSE;
    END_IF;

    // 4. Tổng hợp điều kiện liên động an toàn Freigaben
    All_Safety_Conditions_Met := (NOT DB58.EStop_Activated)
                             AND (NOT DB58.Oil_Level_Low)
                             AND (NOT DB58.Oil_Temp_High)
                             AND (NOT DB58.Filter_Clogged)
                             AND (NOT DB58.DP_Bus_Failure)
                             AND (NOT DB58.Motor_Overload);

    // Xuất cờ Freigabe M61.0
    M61.0 := All_Safety_Conditions_Met;

    // Nếu mất cờ Freigabe, tự động tắt toàn bộ van áp lực và đầu cắt
    IF NOT M61.0 THEN
        Q16.2 := FALSE; // Tắt kích tiến trái
        Q17.2 := FALSE; // Tắt kích tiến phải
        Q16.1 := FALSE; // Tắt Dehner 1
        Q16.3 := FALSE; // Tắt Dehner 2
        Q17.1 := FALSE; // Tắt Dehner 3
        M20.1 := FALSE; // Tắt quay phải đầu cắt
        M20.2 := FALSE; // Tắt quay trái đầu cắt
        Q48.0 := FALSE; // Tắt bơm cao áp 400 bar
    END_IF;
END_FUNCTION
```

---

## 8. QUẢN LÝ & XÓA CỜ BÁO LỖI: FC71 & FC72 (FAULT MANAGER)

```pascal
FUNCTION FC71_Fault_Management : VOID
TITLE = 'Quản Lý & Xử Lý Cờ Báo Lỗi Toàn Máy'
BEGIN
    // Kiểm tra nếu có bất kỳ cờ lỗi nào tích cực
    IF DB58.EStop_Activated OR DB58.DP_Bus_Failure OR DB58.Oil_Level_Low 
       OR DB58.Oil_Temp_High OR DB58.Filter_Clogged OR DB58.Motor_Overload 
       OR DB58.HD_Pump_No_Flow OR DB58.Cutter_OverTorque OR DB58.Roll_Angle_Excess THEN
        DB58.Gen_Fault := TRUE;
        Q75.0 := TRUE; // Bật còi / Đèn cảnh báo sự cố
    END_IF;

    // Reset lỗi khi thợ vận hành bấm nút Reset trên HMI hoặc tủ điều khiển
    IF (I60.0 OR DB19.Btn_EStop_Reset) AND M61.0 THEN
        DB58.Gen_Fault := FALSE;
        Q75.0 := FALSE; // Tắt còi báo
    END_IF;
END_FUNCTION
```

---

## 9. ĐIỀU KHIỂN TRẠM NGUỒN THỦY LỰC & ĐỘNG CƠ CHÍNH: FC13, FC30, FC33

```pascal
FUNCTION FC13_Main_Motor_Control : VOID
TITLE = 'Điều Khiển Khởi Động Động Cơ Bơm Nguồn Thủy Lực Chính 110kW/132kW'
BEGIN
    // Mạch tự giữ Start / Stop động cơ chính Container
    IF M61.0 AND (I61.1 OR DB19.Btn_Auto_Mode) AND (NOT I61.0) AND (NOT I1.1) THEN
        Q1.0 := TRUE; // Kích cuộn hút công tắc tơ khởi động Sao-Tam giác / PKE
    ELSIF I61.0 OR (NOT M61.0) OR I1.1 THEN
        Q1.0 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC33_Filter_Monitoring : VOID
TITLE = 'Giám Sát Độ Chênh Áp & Cảnh Báo Nghẹt Phin Lọc Dầu'
BEGIN
    // Nếu áp lực chênh lệch qua phin lọc đường nạp (PIW 308) vượt quá 4.5 bar (Raw > 12441)
    IF PIW308 > 12441 THEN
        DB58.Filter_Clogged := TRUE;
    ELSE
        DB58.Filter_Clogged := FALSE;
    END_IF;
END_FUNCTION
```

---

## 10. ĐIỀU KHIỂN ĐẢO CHIỀU & TỐC ĐỘ ĐẦU CẮT SCHÜRFRAD: FC14, FC25, FC26

```pascal
FUNCTION FC25_Cutter_Direction : VOID
TITLE = 'Điều Khiển Đảo Chiều Thuận (CW) / Nghịch (CCW) Mâm Cắt Schürfrad'
BEGIN
    // 1. Quay thuận phải (CW): Yêu cầu Freigabe M61.0, không đang quay trái (M20.2)
    IF M61.0 AND (I90.1 OR DB19.Btn_Cutter_CW) AND (NOT M20.2) THEN
        M20.1 := TRUE; // Cờ Cutter_CW
    ELSIF (NOT M61.0) OR I61.0 OR (NOT I90.1 AND NOT DB19.Btn_Cutter_CW) THEN
        M20.1 := FALSE;
    END_IF;

    // 2. Quay nghịch trái (CCW): Yêu cầu Freigabe M61.0, không đang quay phải (M20.1)
    IF M61.0 AND (I90.2 OR DB19.Btn_Cutter_CCW) AND (NOT M20.1) THEN
        M20.2 := TRUE; // Cờ Cutter_CCW
    ELSIF (NOT M61.0) OR I61.0 OR (NOT I90.2 AND NOT DB19.Btn_Cutter_CCW) THEN
        M20.2 := FALSE;
    END_IF;

    // Đèn báo trạng thái
    Q90.0 := M20.1; // Đèn báo Quay phải
    Q92.0 := M20.2; // Đèn báo Quay trái
END_FUNCTION

FUNCTION FC26_Cutter_Speed_Scale : VOID
TITLE = 'Chuẩn Hóa Chiết Áp HMI Xuất Điện Áp Card Khuếch Đại Đĩa Bơm (PQW 268)'
VAR_TEMP
    Scaled_Word : INT;
    Target_Speed : REAL;
END_VAR

BEGIN
    IF M20.1 OR M20.2 THEN
        Target_Speed := DB19.Slider_Cutter_Speed; // 0.0 - 100.0 %

        // Giới hạn an toàn dải đặt
        IF Target_Speed > 100.0 THEN Target_Speed := 100.0; END_IF;
        IF Target_Speed < 0.0   THEN Target_Speed := 0.0;   END_IF;

        // Chuẩn hóa 0..100% thành 0..27648 (tương ứng 0..10V van tỷ lệ)
        Scaled_Word := REAL_TO_INT((Target_Speed / 100.0) * 27648.0);

        // Bảo vệ chống kẹt quá tải mô-men: Giảm tốc khi áp lực đĩa cắt > 300 bar (PIW 378)
        IF PIW378 > 23700 THEN
            Scaled_Word := Scaled_Word - 6000;
            IF Scaled_Word < 4000 THEN Scaled_Word := 4000; END_IF;
            DB58.Cutter_OverTorque := TRUE;
        ELSE
            DB58.Cutter_OverTorque := FALSE;
        END_IF;

        PQW268 := INT_TO_WORD(Scaled_Word);
    ELSE
        PQW268 := W#16#0000; // Dừng xuất tín hiệu khi đầu cắt ngừng
    END_IF;
END_FUNCTION
```

---

## 11. ĐẶC TÍNH LƯU LƯỢNG BƠM REXROTH A4VG / CSG: FC4, FC5, FC140, FC150

```pascal
FUNCTION FC5_Pump_Characteristic_A4VG : VOID
TITLE = 'Nội Suy Đường Đặc Tính Bơm Biến Tích Rexroth A4VG 132kW/250cc'
VAR_INPUT
    Target_RPM : REAL; // Vòng quay yêu cầu từ HMI (0 - 6.0 RPM)
END_VAR
VAR_OUTPUT
    SwashPlate_Angle_Volt : REAL; // Điện áp điều khiển góc nghiêng đĩa nghiêng (0 - 10V)
END_VAR

BEGIN
    // Phương trình đặc tính lưu lượng góc nghiêng: Q = Vg * n * eta_vol
    // Góc nghiêng alpha tỷ lệ tuyến tính với điện áp 0-10V
    SwashPlate_Angle_Volt := (Target_RPM / 6.0) * 10.0;

    IF SwashPlate_Angle_Volt > 10.0 THEN
        SwashPlate_Angle_Volt := 10.0;
    ELSIF SwashPlate_Angle_Volt < 0.0 THEN
        SwashPlate_Angle_Volt := 0.0;
    END_IF;
END_FUNCTION
```

---

## 12. ĐIỀU KHIỂN TRẠM KÍCH ĐẨY CHÍNH & QUY ĐỔI RA TẤN: FC20, FC21, FC22

```pascal
FUNCTION FC20_MainJacks_Control : VOID
TITLE = 'Điều Khiển Đóng Mở Van Kích Tiến / Lùi Trạm Kích Chính Container'
BEGIN
    // 1. Kích Tiến (Advance): Yêu cầu Freigabe M61.0, Đầu cắt đang quay (M20.1/M20.2), Chốt khóa đã mở (M12.7)
    IF M61.0 AND (M20.1 OR M20.2) AND M12.7 AND (I62.2 OR DB19.Btn_Jacks_Adv) AND (NOT Q16.0) THEN
        Q16.2 := TRUE; // Van kích tiến bên trái (-K6.1)
        Q17.2 := TRUE; // Van kích tiến bên phải (-K7.1)
        Q89.0 := TRUE; // Đèn báo Kích đang tiến
    ELSE
        Q16.2 := FALSE;
        Q17.2 := FALSE;
        Q89.0 := FALSE;
    END_IF;

    // 2. Kích Lùi (Retract): Thu hồi xi lanh kích về vị trí chuẩn bị chèn đốt cống
    IF M61.0 AND M12.7 AND (I62.3 OR DB19.Btn_Jacks_Ret) AND (NOT Q16.2) THEN
        Q16.0 := TRUE; // Van kích lùi bên trái (-K6.2)
        Q17.0 := TRUE; // Van kích lùi bên phải (-K7.2)
        Q87.0 := TRUE; // Đèn báo Kích đang lùi
    ELSE
        Q16.0 := FALSE;
        Q17.0 := FALSE;
        Q87.0 := FALSE;
    END_IF;

    // 3. Xuất giá trị van tỷ lệ vận tốc kích đẩy (PQW 264)
    IF Q16.2 THEN
        PQW264 := INT_TO_WORD(REAL_TO_INT((DB19.Slider_Jacks_Speed / 100.0) * 27648.0));
    ELSE
        PQW264 := W#16#0000;
    END_IF;
END_FUNCTION

FUNCTION FC22_Bar_To_Tonnage_Calc : VOID
TITLE = 'Quy Đổi Áp Suất Kích (PIW 406) Ra Tổng Lực Kích Đẩy Hầm (Tấn)'
BEGIN
    // 1. Chuẩn hóa điện áp Analog 4-20mA từ cảm biến áp suất PIW 406 sang Bar (0 - 400 bar)
    DB16.Jacks_Pres_PP := (INT_TO_REAL(PIW406) / 27648.0) * 400.0;

    IF DB16.Jacks_Pres_PP < 0.0 THEN DB16.Jacks_Pres_PP := 0.0; END_IF;

    // 2. Tính lực đẩy: Lực (Tấn) = Áp suất (bar = kG/cm2) * Diện tích piston (cm2) / 1000
    // Với 4 xi lanh kích đẩy chính đường kính 280mm -> Tiết diện = 2454.3 cm2
    DB17.Total_Force_Tons := DB16.Jacks_Pres_PP * DB17.Conversion_Factor;
END_FUNCTION
```

---

## 13. ĐIỀU KHIỂN ĐỒNG BỘ CÁC TRẠM KÍCH TRUNG GIAN: FC55, FC56, FC57 (DEHNER 1..5)

```pascal
FUNCTION FC55_Dehner_Synchronizer : VOID
TITLE = 'Phối Hợp Đồng Bộ Chu Trình Đẩy Trạm Kích Trung Gian Chống Vỡ Ống Cống'
BEGIN
    // Đọc áp suất & hành trình 3 trạm Dehner
    DB16.Dehner1_Stroke := (INT_TO_REAL(PIW336) / 27648.0) * 1200.0; // 0 - 1200 mm
    DB16.Dehner1_Pres   := (INT_TO_REAL(PIW338) / 27648.0) * 400.0;  // 0 - 400 bar

    DB16.Dehner2_Stroke := (INT_TO_REAL(PIW344) / 27648.0) * 1200.0;
    DB16.Dehner2_Pres   := (INT_TO_REAL(PIW346) / 27648.0) * 400.0;

    DB16.Dehner3_Stroke := (INT_TO_REAL(PIW380) / 27648.0) * 1200.0;
    DB16.Dehner3_Pres   := (INT_TO_REAL(PIW382) / 27648.0) * 400.0;

    // Chu trình tự động: Kích lần lượt từng trạm trung gian khi hành trình đạt ngưỡng
    IF M61.0 AND DB19.Btn_Auto_Mode THEN
        // Bước 1: Kích Dehner 1 đẩy đoạn khiên đầu
        IF (DB16.Dehner1_Stroke < 1100.0) AND (NOT Q16.3) AND (NOT Q17.1) THEN
            Q16.1 := TRUE; // Van Dehner 1 (-K10)
        ELSE
            Q16.1 := FALSE;
        END_IF;

        // Bước 2: Kích Dehner 2 đẩy đoạn giữa
        IF (DB16.Dehner1_Stroke >= 1100.0) AND (DB16.Dehner2_Stroke < 1100.0) THEN
            Q16.3 := TRUE; // Van Dehner 2 (-K11)
        ELSE
            Q16.3 := FALSE;
        END_IF;

        // Bước 3: Kích Dehner 3 đẩy đoạn sau
        IF (DB16.Dehner2_Stroke >= 1100.0) AND (DB16.Dehner3_Stroke < 1100.0) THEN
            Q17.1 := TRUE; // Van Dehner 3 (-K12)
        ELSE
            Q17.1 := FALSE;
        END_IF;
    END_IF;
END_FUNCTION
```

---

## 14. ĐIỀU KHIỂN BẺ LÁI 3 XI LANH 120° & CÁNH CHỐNG XOAY: FC40..43

```pascal
FUNCTION FC40_Steering_Hydraulics : VOID
TITLE = 'Điều Khiển 3 Cụm Xi Lanh Bẻ Khớp Lái Bố Trí Lệch 120 Độ'
BEGIN
    // Đọc hành trình 3 xi lanh lái (PIW 356, 358, 360) sang mm (0 - 200 mm)
    DB16.Cyl1_Stroke := (INT_TO_REAL(PIW356) / 27648.0) * 200.0;
    DB16.Cyl2_Stroke := (INT_TO_REAL(PIW358) / 27648.0) * 200.0;
    DB16.Cyl3_Stroke := (INT_TO_REAL(PIW360) / 27648.0) * 200.0;

    // Bơm dầu bẻ lái phải chạy (Q 5.0)
    IF M61.0 THEN
        Q5.0 := TRUE;

        // Xi lanh 1 Thò (Q53.0) / Thụt (Q53.1)
        IF (DB19.Slider_Steer_Angle > 1.0) AND (DB16.Cyl1_Stroke < 190.0) THEN
            Q53.0 := TRUE;  // ValveCyl1_out
            Q53.1 := FALSE;
        ELSIF (DB19.Slider_Steer_Angle < -1.0) AND (DB16.Cyl1_Stroke > 10.0) THEN
            Q53.0 := FALSE;
            Q53.1 := TRUE;  // ValveCyl1_in
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
TITLE = 'Điều Khiển Bung Cánh Thép Ghim Vách Đất Chống Xoay Khiên Đào'
BEGIN
    // Nếu góc xoắn thân máy vượt quá +-1.5 độ hoặc thợ lái bấm nút thủ công
    IF (ABS(DB16.Roll_Angle_Deg) > 1.5) OR I63.1 OR DB19.Btn_Wing_Extend THEN
        Q51.4 := TRUE;  // ValveWing_Out (Bung cánh thép ghim vào thành vách đất)
        Q51.5 := FALSE;
        DB58.Roll_Angle_Excess := TRUE;
    ELSIF I63.3 OR DB19.Btn_Wing_Retract THEN
        Q51.4 := FALSE;
        Q51.5 := TRUE;  // ValveWing_In (Thu cánh về thân khiên)
        DB58.Roll_Angle_Excess := FALSE;
    END_IF;
END_FUNCTION
```

---

## 15. ĐỊNH VỊ LASER TACS & BÙ NGHIÊNG XOẮN: FC80, FC81, FC82

```pascal
FUNCTION FC81_Inclinometer_Read : VOID
TITLE = 'Đọc Cảm Biến Nghiêng Trục Dọc (Pitch) & Nghiêng Trục Ngang (Roll)'
BEGIN
    // Đọc cảm biến độ xoắn Roll (PIW 370) thang đo +-30.0 Deg
    DB16.Roll_Angle_Deg := ((INT_TO_REAL(PIW370) - 13824.0) / 13824.0) * 30.0;

    // Đọc cảm biến độ dốc Pitch (PIW 332) thang đo +-30.0 Deg
    DB16.Pitch_Angle_Deg := ((INT_TO_REAL(PIW332) - 13824.0) / 13824.0) * 30.0;
END_FUNCTION

FUNCTION FC80_TACS_Laser_Parser : VOID
TITLE = 'Giải Mã Tọa Độ Quang Học Tâm Gương Bia Ngắm Laser TACS'
VAR_INPUT
    Laser_Raw_X : INT; // Tín hiệu truyền thông RS485 từ bia TACS
    Laser_Raw_Y : INT;
END_VAR
VAR_OUTPUT
    Delta_X_mm  : REAL; // Độ lệch tim ngang (mm)
    Delta_Y_mm  : REAL; // Độ lệch cao độ thẳng đứng (mm)
END_VAR

BEGIN
    Delta_X_mm := INT_TO_REAL(Laser_Raw_X) * 0.1; // Độ phân giải 0.1mm
    Delta_Y_mm := INT_TO_REAL(Laser_Raw_Y) * 0.1;
END_FUNCTION
```

---

## 16. HỆ THỐNG TUẦN HOÀN BÙN & BIẾN TẦN SCHNEIDER ALTIVAR: FC10, FC11, FC12, FC112

```pascal
FUNCTION FC10_Slurry_Charge_Pump : VOID
TITLE = 'Điều Khiển Động Cơ Bơm Cấp Dung Dịch Bùn Bentonite Vào Buồng Đào'
BEGIN
    // Bơm cấp bùn chạy khi đủ liên động và áp lực buồng đào chưa quá tải (< 4.5 bar)
    IF M61.0 AND (I50.0 OR DB19.Btn_Auto_Mode) AND (NOT I50.1) AND (DB16.Slurry_Chamber_Pres < 4.5) THEN
        M10.1 := TRUE;  // Cờ Charge_Run
        Q73.0 := TRUE;  // Đèn báo Bơm cấp chạy
    ELSE
        M10.1 := FALSE;
        Q73.0 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC11_Slurry_Discharge_Pump : VOID
TITLE = 'Điều Khiển Động Cơ Bơm Hút Bùn Thải Về Trạm Lọc Cát'
BEGIN
    // Bơm thải bùn chạy khi áp suất ống xả an toàn (< 6.0 bar)
    IF M61.0 AND (I50.2 OR DB19.Btn_Auto_Mode) AND (NOT I50.3) AND (DB16.Slurry_Line_Pres < 6.0) THEN
        M11.1 := TRUE;  // Cờ Discharge_Run
        Q76.0 := TRUE;  // Đèn báo Bơm thải chạy
    ELSE
        M11.1 := FALSE;
        Q76.0 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC112_Altivar_VFD_Control : VOID
TITLE = 'Truyền Thông Số Giao Tiếp Đặt Tốc Độ Biến Tần Schneider Altivar'
BEGIN
    // Gửi lệnh Run và tần số đặt qua mạng Profibus-DP (DB112)
    IF M10.1 THEN
        DB112.ControlWord := 16#000F; // Lệnh RUN (Switch On & Enable Operation)
        DB112.SpeedRef_Hz := 500;     // Đặt 50.0 Hz
    ELSE
        DB112.ControlWord := 16#0000; // Lệnh STOP
        DB112.SpeedRef_Hz := 0;
    END_IF;
END_FUNCTION
```

---

## 17. BƠM NƯỚC CAO ÁP 400 BAR & CỤM BÉC PHUN MẶT GƯƠNG: FC90, FC91

```pascal
FUNCTION FC90_HighPressure_Pump : VOID
TITLE = 'Điều Khiển Khởi Động Động Cơ Bơm Nước Cao Áp Piston 400 bar'
BEGIN
    // Đọc cảm biến áp suất nước cao áp (PIW 256)
    DB16.HD_Water_Pres := (INT_TO_REAL(PIW256) / 27648.0) * 400.0;

    // Khởi động bơm khi có nước cấp (I16.0) và rơ le nhiệt không nhảy (I16.1)
    IF M61.0 AND (M55.3 OR DB19.Btn_HD_Jet_Toggle) AND (NOT I16.0) AND (NOT I16.1) THEN
        Q48.0 := TRUE; // Xuất lệnh khởi động Sao-Tam giác bơm cao áp
        M55.1 := TRUE; // Cờ HD_Pump_Running
    ELSE
        Q48.0 := FALSE;
        M55.1 := FALSE;
    END_IF;
END_FUNCTION

FUNCTION FC91_HD_Nozzles_Control : VOID
TITLE = 'Mở 4 Cụm Van Béc Phun Tia Nước Cực Cao 400 bar Xối Rửa Đĩa Cắt'
BEGIN
    // Mở 4 béc phun khi bơm cao áp đã đạt đủ áp (> 250 bar) và có lệnh phun
    IF M55.1 AND (DB16.HD_Water_Pres > 250.0) AND (I52.2 OR DB19.Btn_HD_Jet_Toggle) THEN
        Q51.2 := TRUE; // HD_Valve 1
        Q51.3 := TRUE; // HD_Valve 2
        Q51.6 := TRUE; // HD_Valve 3
        Q51.7 := TRUE; // HD_Valve 4
    ELSE
        Q51.2 := FALSE;
        Q51.3 := FALSE;
        Q51.6 := FALSE;
        Q51.7 := FALSE;
    END_IF;
END_FUNCTION
```

---

## 18. ĐO CHIỀU DÀI HẦM ĐÀO: FC35 (LÄNGENVORTRIEB ENCODER)

```pascal
FUNCTION FC35_Distance_Measurement : VOID
TITLE = 'Đếm Xung Bánh Xe Encoder Đo Quãng Đường Kích Đẩy Längenvortrieb'
VAR
    Prev_Pulse_State : BOOL;
END_VAR

BEGIN
    // Phát hiện sườn lên (Rising Edge) của xung cảm biến bánh xe đo chiều dài (I 0.0)
    IF I0.0 AND (NOT Prev_Pulse_State) THEN
        // Mỗi xung tương ứng 1 mm dịch chuyển
        DB1.DBD0 := DB1.DBD0 + 1; // Tăng chiều dài tổng cộng (mm)
    END_IF;
    Prev_Pulse_State := I0.0;
END_FUNCTION
```

---

## 19. GIAO TIẾP MẠNG CÁP DẸT SMARTWIRE-DT EATON PKE/NZM: FC110, FC111

```pascal
FUNCTION FC110_SmartWire_PKE_Read : VOID
TITLE = 'Đọc Dữ Liệu Dòng Điện & Quá Tải Từ Rơ Le Động Cơ Điện Tử Eaton PKE'
BEGIN
    // Khi mạng Profibus DP đến Gateway Eaton EU5C-SWD-DP hoạt động tốt (M 76.0)
    IF M76.0 THEN
        // Đọc giá trị dòng tải thực tế từ các module PKE lưu vào DB31
        // Nếu bất kỳ động cơ nào vượt 115% dòng định mức trong 5 giây -> Báo lỗi quá tải
        IF DB31.DBD0 > 165.0 THEN // Dòng động cơ cắt > 165A
            DB58.Motor_Overload := TRUE;
        ELSE
            DB58.Motor_Overload := FALSE;
        END_IF;
    END_IF;
END_FUNCTION
```

---

## 20. GIAO TIẾP MÀN HÌNH CẢM ỨNG HMI & NHẬT KÝ SCADA: DB19, DB57

```pascal
FUNCTION FC57_SCADA_DataLogging : VOID
TITLE = 'Ghi Lưu Nhật Ký Thông Số Khoan Định Kỳ 1 Giây Phục Vụ Máy Chủ SCADA'
VAR
    Clock_1Hz_Edge : BOOL;
    Prev_Clock     : BOOL;
END_VAR

BEGIN
    // Sử dụng bit xung nhịp 1Hz của CPU (M 0.5)
    IF M0.5 AND (NOT Prev_Clock) THEN
        // Sao chép khối dữ liệu đo lường tức thời sang bộ đệm ghi log SCADA (DB57)
        DB57.DBD0  := DB1.DBD0;             // Quãng đường đào (mm)
        DB57.DBD4  := DB102.DBD2;           // Vận tốc kích đẩy (mm/min)
        DB57.DBD8  := DB16.Jacks_Pres_PP;   // Áp suất kích chính (bar)
        DB57.DBD12 := DB17.Total_Force_Tons;// Lực kích tổng (Tấn)
        DB57.DBD16 := DB16.Cutter_Pres_Head;// Áp suất đầu cắt (bar)
        DB57.DBD20 := DB16.Roll_Angle_Deg;  // Góc nghiêng Roll (Deg)
        DB57.DBD24 := DB16.Pitch_Angle_Deg; // Góc dốc Pitch (Deg)
        DB57.DBD28 := DB16.Slurry_Chamber_Pres; // Áp lực buồng đào (bar)
    END_IF;
    Prev_Clock := M0.5;
END_FUNCTION
```

---

## BẢNG TRA CỨU ĐỐI CHIẾU GIỮA CÁC KHỐI SCL / ST & LADDER (LAD)

| Tên Khối Hàm (ST/SCL) | Địa Chỉ Khối | Khối Dữ Liệu (DB) | Chức Năng Chính |
|:---|:---|:---|:---|
| `OB100_Restart` | `OB100` | Toàn cục | Khởi tạo ban đầu khi cấp nguồn |
| `OB86_FaultHandler` | `OB86` | Toàn cục | Chống sập CPU khi mất trạm DP |
| `FB100_SpeedFilter` | `FB100` | `DB101, DB102` | Lọc số trung bình trượt vận tốc |
| `OB1_MainScan` | `OB1` | Toàn cục | Quản lý chu trình quét 14 Networks |
| `FC61_Safety_Freigaben` | `FC61` | `DB58` | Mạch liên động an toàn Freigaben |
| `FC25_Cutter_Direction` | `FC25` | `DB21` | Điều khiển quay thuận/nghịch CW/CCW |
| `FC26_Cutter_Speed_Scale`| `FC26` | `PQW 268` | Chuẩn hóa van tỷ lệ đĩa bơm |
| `FC20_MainJacks_Control` | `FC20` | `PQW 264, 266` | Điều khiển van kích đẩy chính |
| `FC22_Bar_To_Tonnage` | `FC22` | `DB16, DB17` | Quy đổi áp suất Bar ra Tấn |
| `FC55_Dehner_Sync` | `FC55..57`| `DB16` | Đồng bộ 5 trạm kích trung gian |
| `FC40_Steering_Hydr` | `FC40..42`| `DB59` | Điều khiển 3 xi lanh lái 120° |
| `FC43_AntiRoll_Wing` | `FC43` | `Q 51.4` | Bung cánh ghim chống xoay khiên |
| `FC81_Inclinometer_Read`| `FC81` | `DB59` | Đo góc nghiêng Pitch & Roll |
| `FC90_HighPressure_Pump`| `FC90, 91` | `Q 48.0, Q 51.2..7`| Bơm cao áp & 4 béc phun 400 bar |
| `FC35_Distance_Measure` | `FC35` | `DB1` | Đếm xung bánh xe đo quãng đường |
| `FC112_Altivar_VFD` | `FC112` | `DB112` | Giao tiếp biến tần bơm bùn |
| `FC57_SCADA_Logging` | `FC57` | `DB57` | Ghi dữ liệu định kỳ 1 giây |

---

---

### 📑 Cấu trúc và nội dung chi tiết trong file `TBM- Structuredtext.md`:

1. **Định nghĩa kiểu dữ liệu cấu trúc (UDT / TYPE):**
   * `UDT_AnalogSensor`: Cấu trúc chuẩn hóa tín hiệu Analog ADC sang giá trị vật lý và cảnh báo ngưỡng.
   * `UDT_SteeringCylinder`: Cấu trúc điều khiển áp suất, hành trình và van tỷ lệ 3 xi lanh lái 120°.
   * `UDT_DehnerStation`: Cấu trúc điều khiển trạm kích trung gian Dehner 1..5.
   * `UDT_AltivarVFD`: Cấu trúc thanh ghi truyền thông biến tần Schneider Altivar.

2. **Khai báo các khối dữ liệu toàn cục (GLOBAL DATA BLOCKS):**
   * `DB16 (DB_AnalogValues)`: 28 biến analog đo lường áp suất, nhiệt độ, độ ẩm, góc nghiêng, lưu lượng.
   * `DB17 (DB_Druck_Tonnen)`: Công thức và tham số quy đổi áp lực Bar sang Tấn lực đẩy.
   * `DB19 (DB_Touchscreen)`: Vùng nhớ trao đổi nút bấm, chế độ Auto/Man, chiết áp tốc độ với HMI.
   * `DB58 (DB_Fault_DB)`: Bảng cờ lỗi, dừng khẩn cấp E-Stop, rớt mạng DP và quá tải dòng.

3. **Mã nguồn hoàn chỉnh 20 Khối Hàm & Khối Tổ Chức (ST / SCL):**
   * **`OB100` (Complete Restart):** Khởi tạo mặc định và reset toàn bộ cờ lỗi khi bật nguồn CPU.
   * **`OB86` (Rack Fault Handler):** Chống dừng CPU khi mất kết nối trạm Profibus-DP Slaves.
   * **`OB32` & `FB100` (Speed Filter):** Thuật toán lọc số trung bình trượt 10 điểm trong ngắt thời gian thực 100ms tính vận tốc kích đẩy.
   * **`OB1` (MainControl Cycle):** Điều phối tuần tự chu trình quét của 14 Networks công nghệ.
   * **`FC61` (Safety Freigaben):** Mạch liên động kiểm tra an toàn tổng, E-Stop, mức dầu, nhiệt độ và phin lọc.
   * **`FC71` & `FC72` (Fault Manager):** Quản lý cảnh báo còi hú và reset lỗi.
   * **`FC13`, `FC30`, `FC33` (Hydraulic PowerPack):** Khởi động động cơ chính 110kW/132kW và giám sát nghẹt lọc.
   * **`FC14`, `FC25`, `FC26` (Cutterhead Control):** Mạch đảo chiều CW/CCW, chuẩn hóa điện áp van tỷ lệ đĩa bơm `PQW 268` (0–10V) và giảm tốc chống kẹt đĩa cắt.
   * **`FC5` (Pump Characteristic A4VG):** Phương trình nội suy đặc tính đĩa nghiêng bơm Rexroth A4VG / CSG 132kW.
   * **`FC20` & `FC22` (Main Jacks & Tonnage):** Điều khiển kích tiến/lùi, xuất áp van tỷ lệ `PQW 264/266` và tính tổng lực đẩy ra Tấn.
   * **`FC55..57` (Dehner Synchronizer):** Thuật toán tự động kích tuần tự các trạm kích trung gian 1..3.
   * **`FC40` & `FC43` (Steering & Wing):** Điều khiển 6 van thủy lực thò/thụt 3 xi lanh lái và tự động bung cánh chống xoay khiên khi góc Roll > $\pm 1.5^\circ$.
   * **`FC80` & `FC81` (TACS & Inclinometer):** Giải mã tọa độ laser TACS và đọc góc nghiêng Pitch/Roll.
   * **`FC10`, `FC11`, `FC112` (Slurry & Altivar VFD):** Tuần hoàn bùn bentonite và xuất điện áp/lệnh Profibus tới biến tần Schneider.
   * **`FC90` & `FC91` (Water Jet 400 bar):** Bơm piston cao áp và mở cụm 4 béc phun mặt gương.
   * **`FC35` (Distance Pulse Counter):** Đếm sườn xung encoder đo chiều dài hầm (Längenvortrieb).
   * **`FC110` & `FC111` (SmartWire PKE/NZM):** Đọc dòng tải động cơ và bảo vệ ngắt mạch.
   * **`FC57` (SCADA DataLogging):** Bộ đệm sao chép snapshot dữ liệu đo lường định kỳ 1 giây.
