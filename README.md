# MTS 1000 (Bohrkopf 2100) — Siemens SIMATIC S7-300 & VisAM 17" SCADA Digital Twin

[![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/demo-GitHub%20Pages-brightgreen.svg)](https://darktutemi.github.io/tbm/)
[![PLC](https://img.shields.io/badge/PLC-Siemens%20S7--300%20(CPU%20315--2%20PN%2FDP)-00646E.svg)](https://support.industry.siemens.com/)
[![SCADA](https://img.shields.io/badge/SCADA-VisAM%20Touchscreen%2017%22-orange.svg)](https://www.visam.de/)

> **Hệ Thống Mô Phỏng Số (Digital Twin), Bản Vẽ Điện Kỹ Thuật & Giám Sát Thời Gian Thực Toàn Diện Cho Máy Khoan Ngầm Cân Bằng Áp Lực Bùn MTS 1000 (MTS Perforator GmbH — Order 2018221 / DIN 61346).**

---

## 🌟 Trực Tiếp Demo (Live Demo)

Truy cập và trải nghiệm trực tiếp bộ mô phỏng trên trình duyệt (hỗ trợ PC, Tablet iPad và Điện thoại di động):
🔗 **[https://darktutemi.github.io/tbm/](https://darktutemi.github.io/tbm/)**

---

## 📌 Các Phân Hệ Tính Năng Chính

### 1. Màn Hình VisAM 17" Touchscreen SCADA (Photo-Exact Replica)
* Tái hiện chuẩn xác 1:1 theo ảnh thực tế `1000027975.jpg` của máy MTS 1000.
* Khung nền chuẩn công nghiệp Windows Embedded (`#d4d0c8`) với các vát góc 3D.
* **13 Phím mềm điều hướng (Left Softkeys):** `Setup`, `Info's PPH`, `Faults`, `Profibus`, `Pipes`, `Head roll left/right`, `Case drain head`, `Steering`, `HD pump`, `Locking cyl.`, `Pipe hold`, `Crusher`.
* **Cụm thông số đĩa cắt & áp lực bùn:** `Cutter Head Rev`, `Cutter Head P`, `Main Jacking Unit F`, `Slurry Chamber P`, `Slurry Line P`, `RPM Slurry pumps 1..3`, `Slurry circuit V`.
* **Sơ đồ đồ họa mạch bùn tuần hoàn (Slurry Circuit Graphic):** Tuyến ống nạp xanh dương, ống buồng đào xanh lá, nút trung tâm `Unlock Bypass-Jet` (Màu đỏ/xanh) đóng vai trò khóa liên động an toàn.
* **Định vị hình học hầm & logo `mts PERFORATOR`:** Độ lệch tâm đứng/ngang (`Vertical/Horiz Dev`), góc nghiêng `Head roll`, độ dốc `Gradient`, nhiệt độ dầu bồn nguồn và đầu khiên.
* **Hệ thống xilanh bẻ khớp lái 3D (`Steering rams 1..3`):** 3 thanh trượt đứng đo hành trình mm, áp lực bar và tổng lực kích bẻ lái `F [ton]`.

### 2. Mặt Bàn Điều Khiển Cabin Figure 3.5 (=cc Bedienpult)
* Tái hiện nguyên bản từ Hồ sơ Kỹ thuật Pháp lý Tập I - Mục 4.3.1.3 Trang 36.
* Toàn bộ **33 phần tử điều khiển (Item 01 đến 32)** bao gồm nút bấm có đèn SmartWire-DT, núm xoay, chiết áp potentiometer Rexroth và joystick điều khiển xilanh lái.
* **Đồng bộ 2 chiều (Bidirectional Closed-Loop Sync):** Khi bấm nút trên Bàn điều khiển hoặc chạm màn hình VisAM, mọi van điện từ `-K58/-K59`, bơm thủy lực Rexroth A4VG và động cơ đều phản hồi đồng thời.

### 3. Chu Trình Vận Hành 7 Giai Đoạn Khép Kín (IEC 61131-3 SCL/ST)
Hệ thống tích hợp logic tự động theo chu trình thi công thực tế:
1. **GĐ 1:** Khởi tạo & Kiểm tra điều kiện liên động an toàn (Safety Loop FC61 / E-Stop / Phao dầu).
2. **GĐ 2:** Tuần hoàn bùn Bentonite (Mở van Bypass, bật bơm nạp Altivar 50Hz, ổn định buồng đào).
3. **GĐ 3:** Khởi động đĩa cắt & Cơ cấu nón nghiền chống kẹt đất sét (FC14/FC25/FC26).
4. **GĐ 4:** Kích tiến đồng bộ & Đo quãng đường thực tế (FC20/FC22, tính toán lực kích Tấn).
5. **GĐ 5:** Phối hợp trạm kích phụ trung gian Dehner 1..5 (FC55..FC57).
6. **GĐ 6:** Bẻ khớp lái 3 xilanh 120° theo tim bia Laser TACS & Bung cánh chống xoay Wing (FC40/FC80).
7. **GĐ 7:** Xối rửa áp lực cao Water Jet 400 bar (FC90/FC91, béc phun buồng đào).

### 4. Truy Vết Tín Hiệu Thời Gian Thực (Visual Signal Trace Across 10 Hops)
Trực quan hóa luồng tín hiệu từ cabin xuống gương hầm qua 10 chặng:
1. Nút bấm vật lý Bàn điều khiển cabin `=cc`.
2. Mạng truyền thông nội bộ Eaton SmartWire-DT.
3. Gateway SmartWire-DP (`-K1.10`, DP-Adr. 40).
4. Cáp quang / Cáp bọc kim Profibus-DP đường hầm (`-W22`).
5. Bộ điều khiển trung tâm PLC Siemens SIMATIC S7-300 (CPU 315-2 PN/DP).
6. Màn hình SCADA máy tính công nghiệp IPC VisAM.
7. Cụm trạm I/O từ xa Turck IP67/IP68 dưới hầm:
   * **PicoNet Profibus `-K2.2`** (DP-Adr. 24) điều khiển cụm van bùn.
   * **Turck Analog IP68 `-K2.1`** (DP-Adr. 25) điều khiển xilanh lái.
   * **Turck BL67 Modular `-K1.1`** (DP-Adr. 05) trạm nguồn thủy lực container `=pph`.
8. Khối rơ-le cách ly đệm công suất 24VDC.
9. Cuộn hút Solenoid Van thủy lực / Biến tần động cơ 132kW/160kW.
10. Tín hiệu phản hồi hành trình (Feedback limit switch) xác nhận trạng thái ngược về PLC.

### 5. Hỗ Trợ Đầy Đủ iPad & Điện Thoại Di Động
* **Multi-touch Pinch-to-Zoom:** Thu phóng tự do từ $20\%$ đến $500\%$ bằng 2 ngón tay trên màn hình cảm ứng.
* **Bộ điều khiển thu phóng nổi 1 chạm (#mobileZoomBar):** Các nấc zoom nhanh $50\%$, $75\%$, $100\%$, $125\%$, $150\%$, $200\%$ và nút `+` / `-`.
* **Tương thích 100% iPadOS Safari / WebKit:** Loại bỏ tình trạng nuốt click với `touch-action: manipulation`, mở khóa click bubbling.

---

## 📂 Cấu Trúc Mã Nguồn

```
├── tbm_plc_simulator.html    # Ứng dụng mô phỏng chính (Single Page Application)
├── index.html                # Cổng phục vụ GitHub Pages
├── 2018221a.s7p              # File dự án gốc Siemens STEP 7 v5.5
├── s7asrcom/                 # Cấu hình phần cứng Hardware Config HW-Config
├── ombstx/                   # Bảng mã nguồn Blocks (OB, FC, FB, DB)
├── TBM- Structuredtext.md    # Chi tiết mã nguồn SCL/Structured Text của toàn bộ khối hàm
├── TBM-PLC.MD                # Tài liệu phân tích phần cứng PLC và địa chỉ I/O
├── TBM-SCL.MD                # Kiến trúc chương trình PLC và chu trình quét OB1
├── TBM-BOM-Sensors.md        # Bảng kê 118 thiết bị, cảm biến và cuộn hút điện từ
├── TBM-BOM-Sensors.csv       # Dữ liệu BOM định dạng bảng tính CSV
└── README.md                 # Tài liệu giới thiệu dự án
```

---

## 🚀 Hướng Dẫn Chạy Cục Bộ (Run Locally)

Chỉ cần một trình duyệt web hiện đại (Chrome, Edge, Safari, Firefox), không cần cài đặt thêm bất kỳ thư viện hay server phức tạp nào:

```bash
# Cách 1: Mở trực tiếp file HTML
double-click vào file "index.html" hoặc "tbm_plc_simulator.html"

# Cách 2: Chạy qua Python HTTP Server
python -m http.server 8080
# Truy cập: http://localhost:8080/index.html
```

---

## 👨‍💻 Bản Quyền & Tác Giả

* **Hệ thống gốc:** MTS Perforator GmbH — Bohrkopf 2100 / MTS 1000.
* **Tác giả số hóa & Digital Twin:** DarkTutemi (`duyanh033hgt@gmail.com`).
* **Repository:** [https://github.com/DarkTutemi/tbm](https://github.com/DarkTutemi/tbm)
