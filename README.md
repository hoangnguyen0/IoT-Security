# Mô phỏng Wokwi – Minh hoạ checklist bảo mật IoT

Thư mục này chứa mô phỏng ESP32 + DHT22 dùng để minh hoạ trực quan các tiêu chí
**FW02, FW03, PR01, NW04** trong checklist bảo mật (xem báo cáo chính
`Checklist_Bao_Mat_IoT.docx`, Mục 8).

## Cấu trúc

```
wokwi-demo/
├── diagram.json                 # Sơ đồ phần cứng (ESP32 + DHT22) dùng chung cho cả 2 kịch bản
├── wokwi.toml                   # Cấu hình project Wokwi
├── libraries.txt                # Danh sách thư viện Arduino cần cài
├── scenario_A_insecure/
│   └── sketch.ino               # Kịch bản KHÔNG AN TOÀN (mật khẩu hardcode, MQTT plaintext)
└── scenario_B_secure/
    ├── sketch.ino               # Kịch bản ĐÃ KHẮC PHỤC (TLS, tách secrets, không anonymous)
    └── secrets.h                # File cấu hình nhạy cảm mẫu (KHÔNG commit lên Git khi dùng thật)
```

## Cách chạy trên wokwi.com (không cần cài phần cứng)

1. Truy cập https://wokwi.com/projects/new/esp32 để tạo project ESP32 mới.
2. Mở tab **diagram.json** trong project mới, thay nội dung bằng file `diagram.json` ở đây.
3. Mở tab **sketch.ino**, dán nội dung của `scenario_A_insecure/sketch.ino` (hoặc
   `scenario_B_secure/sketch.ino` + tạo thêm tab `secrets.h` với nội dung tương ứng).
4. Vào **Library Manager** (biểu tượng quyển sách bên trái), thêm 3 thư viện liệt kê
   trong `libraries.txt`.
5. Nhấn nút **Play (▶)** để chạy mô phỏng, theo dõi **Serial Monitor** để xem log
   kết nối WiFi/MQTT.
6. Dùng một MQTT client thật trên máy tính (ví dụ MQTT Explorer hoặc lệnh
   `mosquitto_sub -h test.mosquitto.org -t "iot-checklist-demo/insecure/sensor"`)
   để quan sát dữ liệu — đây chính là minh chứng cần chụp lại cho phiếu kiểm tra.

## Ánh xạ sang checklist

| Kịch bản | Tiêu chí minh hoạ | Trạng thái minh hoạ |
|---|---|---|
| A – Không an toàn | FW02, FW03 | Chưa đạt — user/pass hardcode trong sketch.ino |
| A – Không an toàn | PR01, NW04 | Chưa đạt — MQTT cổng 1883, không TLS, payload đọc được trực tiếp |
| B – Đã khắc phục | FW02, FW03 | Đạt — thông tin nhạy cảm tách sang `secrets.h`, không commit |
| B – Đã khắc phục | PR01, NW04 | Đạt — MQTT cổng 8883 qua `WiFiClientSecure`, có xác thực, không cho anonymous fallback |

## Lưu ý

- Đây là môi trường **mô phỏng phục vụ học tập**, không phải hướng dẫn tấn công hệ thống thật.
  Chỉ kết nối tới broker công cộng dùng cho mục đích thử nghiệm (ví dụ `test.mosquitto.org`),
  không publish dữ liệu thật hoặc gắn với hệ thống sản xuất.
- `secrets.h` trong kịch bản B chỉ là **ví dụ mẫu**; khi áp dụng cho dự án thật, thông tin
  đăng nhập cần được cấp phát qua quy trình provisioning an toàn, không đặt cứng trong file.
