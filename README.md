# Video Streaming with RTSP and RTP

Đồ án Mạng Máy Tính - Video Streaming sử dụng giao thức RTSP và RTP

## 👥 Thành viên nhóm

- MSSV1: [Tên thành viên 1]
- MSSV2: [Tên thành viên 2]  
- MSSV3: [Tên thành viên 3]

## 📋 Mô tả

Dự án implement hệ thống streaming video client-server sử dụng:
- **RTSP (Real-Time Streaming Protocol)** - TCP port 554 - Điều khiển phiên streaming
- **RTP (Real-time Transport Protocol)** - UDP - Truyền dữ liệu video

---

## 🛠️ Yêu cầu hệ thống & Cài đặt

### Bước 1: Kiểm tra Python

Project yêu cầu **Python 3.6 trở lên**.

**Kiểm tra version Python:**

**Windows (Command Prompt hoặc PowerShell):**
```bash
python --version
```

**macOS/Linux (Terminal):**
```bash
python3 --version
```

**Kết quả mong đợi:**
```
Python 3.9.7
```
hoặc version 3.6+ bất kỳ

> ⚠️ **Nếu chưa có Python:** Tải tại [python.org/downloads](https://www.python.org/downloads/)

---

### Bước 2: Cài đặt thư viện Pillow (BẮT BUỘC)

**Pillow** là thư viện xử lý ảnh của Python, dùng để hiển thị video frames.

#### 🪟 **Windows**

**Cách 1: Dùng Command Prompt (CMD)**
1. Nhấn `Windows + R`
2. Gõ `cmd` → Enter
3. Chạy lệnh:
```bash
pip install Pillow
```

**Cách 2: Dùng PowerShell**
1. Nhấn `Windows + X` → chọn "Windows PowerShell"
2. Chạy lệnh:
```bash
pip install Pillow
```

**Nếu gặp lỗi "pip is not recognized":**
```bash
python -m pip install Pillow
```

---

#### 🍎 **macOS**

**Mở Terminal** (Applications → Utilities → Terminal):
```bash
pip3 install Pillow
```

**Nếu gặp lỗi permission:**
```bash
sudo pip3 install Pillow
# Nhập password macOS khi được yêu cầu
```

---

#### 🐧 **Linux (Ubuntu/Debian)**

**Mở Terminal:**
```bash
sudo apt-get update
sudo apt-get install python3-pip
pip3 install Pillow
```

**Fedora/CentOS/RHEL:**
```bash
sudo dnf install python3-pip
pip3 install Pillow
```

---

### Bước 3: Kiểm tra Pillow đã cài đặt thành công

Chạy lệnh sau để kiểm tra:

```bash
python -c "from PIL import Image; print('✅ Pillow installed successfully!')"
```

**Hoặc trên macOS/Linux:**
```bash
python3 -c "from PIL import Image; print('✅ Pillow installed successfully!')"
```

**Kết quả mong đợi:**
```
✅ Pillow installed successfully!
```

**Nếu thấy lỗi `ModuleNotFoundError`:** Quay lại Bước 2 và cài đặt lại Pillow

---

### Bước 4: Clone Repository

```bash
git clone https://github.com/24127592-UcNguyenAnhVo/Mang_may_tinh.git
cd Mang_may_tinh/python_rtp
```

**Hoặc tải ZIP:**
1. Vào https://github.com/24127592-UcNguyenAnhVo/Mang_may_tinh
2. Click nút **Code** → **Download ZIP**
3. Giải nén → Vào thư mục `Mang_may_tinh/python_rtp`

---

## 📁 Cấu trúc thư mục

```
python_rtp/
│
├── Server.py              # Server streaming video
├── ServerWorker.py        # Xử lý requests từ client
├── Client.py              # Client nhận và hiển thị video
├── ClientLauncher.py      # Khởi động client GUI
├── RtpPacket.py          # Xử lý RTP packets
├── VideoStream.py        # Đọc video từ file
└── movie.Mjpeg           # File video mẫu
```

---

## 🚀 Hướng dẫn chạy chương trình

### Bước 1: Khởi động Server

**Mở Terminal/CMD** tại thư mục `python_rtp/`:

**Windows:**
```bash
cd Mang_may_tinh\python_rtp
python Server.py 5454
```

**macOS/Linux:**
```bash
cd Mang_may_tinh/python_rtp
python3 Server.py 5454
```

**Giải thích:**
- `5454`: Port server lắng nghe RTSP requests (có thể dùng port > 1024)

**Kết quả mong đợi:**
```
Server is ready to receive requests...
```

> ⚠️ **LƯU Ý:** Giữ cửa sổ này mở, KHÔNG tắt Server!

---

### Bước 2: Khởi động Client

**Mở Terminal/CMD MỚI** (giữ Server chạy ở terminal cũ):

**Windows:**
```bash
cd Mang_may_tinh\python_rtp
python ClientLauncher.py localhost 5454 25000 movie.Mjpeg
```

**macOS/Linux:**
```bash
cd Mang_may_tinh/python_rtp
python3 ClientLauncher.py localhost 5454 25000 movie.Mjpeg
```

**Giải thích tham số:**
- `localhost`: Địa chỉ server (dùng `localhost` nếu chạy cùng máy)
- `5454`: Port RTSP server
- `25000`: Port RTP nhận dữ liệu video (client)
- `movie.Mjpeg`: Tên file video cần stream

**Kết quả:** Cửa sổ GUI sẽ hiện ra như sau:

```
┌─────────────────────────────────────────┐
│         [Video Display Area]            │
│                                         │
│                                         │
└─────────────────────────────────────────┘
│ Setup │ Play │ Pause │ Teardown │
└──────────────────────────────────────────┘
```

---

### Bước 3: Sử dụng Client GUI

**Thứ tự thao tác ĐÚNG:**

1. **Click "Setup"** 
   - Thiết lập kết nối với server
   - Chờ message "200 OK" ở console

2. **Click "Play"** 
   - Bắt đầu streaming video
   - Video sẽ hiển thị trên GUI

3. **Click "Pause"** (optional)
   - Tạm dừng video
   - Click "Play" lại để tiếp tục

4. **Click "Teardown"**
   - Kết thúc phiên streaming
   - Đóng kết nối

---

## 📊 Kiểm tra hoạt động

### ✅ Console Server sẽ hiển thị:
```
Server is ready to receive requests...
Connected from ('127.0.0.1', 50234)

Data received:
SETUP movie.Mjpeg RTSP/1.0
CSeq: 1
Transport: RTP/UDP; client_port= 25000

Data sent:
RTSP/1.0 200 OK
CSeq: 1
Session: 123456

processing PLAY
```

### ✅ Console Client sẽ hiển thị:
```
Data sent:
SETUP movie.Mjpeg RTSP/1.0
CSeq: 1
Transport: RTP/UDP; client_port= 25000

Current Seq Num: 1
Current Seq Num: 2
Current Seq Num: 3
Current Seq Num: 4
...
```

---

## 🐛 Troubleshooting (Khắc phục lỗi)

### ❌ Lỗi 1: `ModuleNotFoundError: No module named 'PIL'`

**Nguyên nhân:** Chưa cài đặt Pillow

**Giải pháp:**
```bash
# Windows
pip install Pillow

# macOS/Linux
pip3 install Pillow
```

---

### ❌ Lỗi 2: `No module named 'tkinter'`

**Nguyên nhân:** Thiếu tkinter (GUI library)

**Giải pháp:**

**Ubuntu/Debian:**
```bash
sudo apt-get install python3-tk
```

**macOS:**
```bash
brew install python-tk
```

**Windows:** Tkinter đã có sẵn, nếu thiếu thì cài lại Python từ [python.org](https://www.python.org)

---

### ❌ Lỗi 3: `[WinError 10048] Only one usage of each socket address`

**Nguyên nhân:** Port đang được sử dụng

**Giải pháp:**

**Cách 1:** Đổi port khác
```bash
# Thay vì dùng 5454, dùng 5455
python Server.py 5455
python ClientLauncher.py localhost 5455 25000 movie.Mjpeg
```

**Cách 2:** Tìm và tắt tiến trình đang dùng port

**Windows:**
```bash
netstat -ano | findstr :5454
taskkill /PID <PID_number> /F
```

**macOS/Linux:**
```bash
lsof -i :5454
kill -9 <PID>
```

---

### ❌ Lỗi 4: `Connection to 'localhost' failed`

**Nguyên nhân:** Server chưa chạy hoặc port sai

**Giải pháp:**
1. Kiểm tra Server đã chạy chưa
2. Kiểm tra port Server và Client có khớp không
3. Tắt Firewall tạm thời để test

---

### ❌ Lỗi 5: Video không hiển thị

**Nguyên nhân:** Thiếu file `movie.Mjpeg`

**Giải pháp:**
1. Kiểm tra file `movie.Mjpeg` có trong thư mục `python_rtp/` không
2. Download file video từ repo
3. Đảm bảo tên file đúng (phân biệt hoa/thường)

---

### ❌ Lỗi 6: `[Usage: Server.py Server_port]`

**Nguyên nhân:** Thiếu tham số khi chạy

**Giải pháp:**
```bash
# SAI - Thiếu port
python Server.py

# ĐÚNG
python Server.py 5454
```

---

## 🔍 RTSP State Machine

```
        SETUP          PLAY
[INIT] ──────> [READY] ──────> [PLAYING]
                  ↑               │
                  │     PAUSE     │
                  └───────────────┘
                        
                  TEARDOWN
        ────────────────────────> [INIT]
```

---

## 📝 RTSP Commands Format

### SETUP Request:
```
SETUP movie.Mjpeg RTSP/1.0
CSeq: 1
Transport: RTP/UDP; client_port= 25000
```

### PLAY Request:
```
PLAY movie.Mjpeg RTSP/1.0
CSeq: 2
Session: 123456
```

### PAUSE Request:
```
PAUSE movie.Mjpeg RTSP/1.0
CSeq: 3
Session: 123456
```

### TEARDOWN Request:
```
TEARDOWN movie.Mjpeg RTSP/1.0
CSeq: 4
Session: 123456
```

---

## 🎯 Phần đã hoàn thành (4/10 điểm)

✅ RTSP Protocol Implementation (Client)  
✅ RTP Packetization (Server)  
✅ Basic GUI với tkinter  
✅ State management (INIT, READY, PLAYING)  
✅ Video streaming cơ bản

---

## 🔜 Phần nâng cao (sẽ thực hiện)

- [ ] HD Video Streaming (720p/1080p) với fragmentation - 3 điểm
- [ ] Client-Side Caching & Frame Buffering - 2.5 điểm  
- [ ] Report & Documentation - 0.5 điểm

---

## 📚 Tài liệu tham khảo

- [RFC 2326 - RTSP](https://datatracker.ietf.org/doc/html/rfc2326)
- [RFC 1889 - RTP](https://datatracker.ietf.org/doc/html/rfc1889)
- [Pillow Documentation](https://pillow.readthedocs.io/)
- [Python Socket Programming](https://docs.python.org/3/library/socket.html)

---

## ⚠️ Lưu ý quan trọng

1. **Chạy Server TRƯỚC**, sau đó mới chạy Client
2. **Không tắt Server** khi Client đang streaming
3. **Thứ tự click buttons:** Setup → Play → Pause/Teardown
4. **Port phải > 1024** (các port < 1024 cần quyền admin)
5. **File video** phải ở cùng thư mục với các file .py

---

**Ghi chú:** Đây là phiên bản cơ bản (4/10 điểm). Phần nâng cao (HD streaming, caching) đang được phát triển.

**Last updated:** 2025-11-21
