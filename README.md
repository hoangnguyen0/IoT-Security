# Checklist kiểm tra bảo mật thiết bị IoT trước triển khai

Thư mục này chứa mô phỏng ESP32 .

## Cấu trúc

```
Checklist kiểm tra bảo mật thiết bị IoT trước triển khai/
├── README.md
├── report/
│   └── Bao_cao_tieu_luan.docx hoặc .pdf
├── slides/
│   └── Slide_trinh_bay.pptx hoặc .pdf
├── src/
│   └── code_demo.py hoặc file nguồn liên quan
├── configs/
│   └── mosquitto.conf, aclfile, flow.json, manifest.json...
├── data/
│   └── dataset_gia_lap.csv hoặc payload_mau.json
├── results/
│   ├── screenshots/
│   ├── logs/
│   └── output.csv
└── references/
    └── link_nguon.md
```

## Cách chạy trên wokwi.com (không cần cài phần cứng)

1. Cấu trúc bộ mã nguồn cung cấp Bộ mã nguồn kiểm thử được đóng gói trong thư mục gốc wokwi-demo, bao gồm tệp README.md và 2 kịch bản chính:          
Thư mục kichban1-khong-an-toan: Chứa mã nguồn mô phỏng cấu hình mạng mặc định, thiếu bảo mật (giao tiếp MQTT dạng Plaintext, không xác thực).            
Thư mục kichban2-da-khac-phuc: Chứa mã nguồn đã được tinh chỉnh, tuân thủ checklist bảo mật (áp dụng mã hóa TLS và xác thực tài khoản).      
Bên trong mỗi thư mục kịch bản đều chứa 3 tệp tin cấu hình tiêu chuẩn của hệ thống Wokwi:          
sketch.ino: Chứa toàn bộ mã nguồn C/C++ xử lý logic mạng và giao thức MQTT.           
diagram.json: Tệp định dạng JSON chứa sơ đồ đấu nối và khai báo linh kiện ảo (ESP32, cảm biến).           
libraries.txt: Chứa danh sách các thư viện cần thiết để biên dịch dự án.         
3. Các bước triển khai mô phỏng (Áp dụng chung cho cả 2 kịch bản)      
Bước 1: Chuẩn bị môi trường Wokwi                   
Mở trình duyệt web và truy cập vào nền tảng mô phỏng: [https://wokwi.com](https://wokwi.com).                               
Chọn tạo một dự án mới từ trang chủ (New Project ESP32).                                             
Bước 2: Nạp cấu hình môi trường ảo                                                                                  
Trên giao diện soạn thảo của Wokwi, mở thẻ diagram.json.                               
Mở tệp diagram.json tương ứng trong thư mục kịch bản, sao chép toàn bộ nội dung và dán đè vào thẻ vừa mở trên web. Sơ đồ mạch mô phỏng sẽ tự động được cập nhật.                      
Bước 3: Khai báo thư viện mạng                               
Chuyển sang thẻ Library Manager (hoặc mở thẻ libraries.txt trên giao diện Wokwi).                                
Đảm bảo khai báo đầy đủ các thư viện được liệt kê trong tệp libraries.txt của kịch bản (ví dụ: PubSubClient, DHT sensor library...).                                    
Bước 4: Nạp mã nguồn phần mềm                                  
Mở tệp sketch.ino từ thư mục kịch bản bằng bất kỳ trình soạn thảo văn bản nào (Notepad, VSCode).                                          
Sao chép toàn bộ mã nguồn và dán đè vào thẻ sketch.ino trên Wokwi.                                          
Bước 5: Khởi chạy và thu thập Log                                    
Nhấn nút Play (màu xanh lá) ở thanh công cụ để nền tảng tiến hành biên dịch (Compile) và chạy mô phỏng.                                    
Mở cửa sổ Serial Monitor (nằm ở góc dưới cùng hoặc bên phải giao diện) để theo dõi luồng thực thi của mã nguồn.                                               
4. Hướng dẫn đối chiếu kết quả (Log Output)                                          
Kiểm chứng Kịch bản 1 (kichban1-khong-an-toan):                                                      
Quan sát Serial Monitor, bạn sẽ thấy thông báo thiết bị kết nối thành công tới MQTT Broker thông qua cổng mặc định (1883) mà không yêu cầu khởi tạo đường hầm TLS hay khai báo thông tin đăng nhập.            
Đánh giá: Kịch bản tái hiện thành công trạng thái không an toàn. Mọi dữ liệu JSON gửi qua mạng ảo đang ở dạng rõ (Plaintext).                                              
Kiểm chứng Kịch bản 2 (kichban2-da-khac-phuc):                                               
Trong Serial Monitor, log sẽ thể hiện thiết bị mất thêm một khoảng thời gian ngắn để thực hiện quá trình bắt tay TLS (TLS Handshake).                                                         
Hệ thống sẽ ghi nhận thiết lập kết nối tới cổng bảo mật (8883) và bắt buộc truyền thông tin Username/Password.                                      
Đánh giá: Xác nhận mã nguồn đã khắc phục lỗ hổng. Kênh truyền tải đã được mã hóa bảo mật, đảm bảo tính toàn vẹn và ngăn chặn các nguy cơ nghe lén/giả mạo (Sniffing/Spoofing).                               
## cách 2: Vào 2 đường kink dưới dây chạy trực tiếp
kịch bản không an toàn: https://wokwi.com/projects/470482721547396097                                        
kịch bản đã khắc phục: https://wokwi.com/projects/470444602516855809
## Lưu ý

- Đây là môi trường **mô phỏng phục vụ học tập**, không phải hướng dẫn tấn công hệ thống thật.
