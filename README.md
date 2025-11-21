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

## 🛠️ Yêu cầu cài đặt

### 1. Python
Yêu cầu Python 3.6 trở lên. Kiểm tra version:
```bash
python --version
# hoặc
python3 --version
```

### 2. Cài đặt thư viện Pillow

**Windows:**
```bash
pip install Pillow
```

**macOS/Linux:**
```bash
pip3 install Pillow
```

**Kiểm tra đã cài đặt thành công:**
```bash
python -c "from PIL import Image; print('Pillow installed successfully!')"
```

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

## 🚀 Hướng dẫn chạy chương trình

### Bước 1: Khởi động Server

Mở terminal/cmd tại thư mục `python_rtp/`:

```bash
python Server.py 5454
```

**Giải thích:**
- `5454`: Port server lắng nghe RTSP requests (có thể dùng port > 1024)
- Server sẽ hiển thị: `Server is ready to receive requests...`

### Bước 2: Khởi động Client

Mở terminal/cmd **MỚI** (giữ server chạy), tại cùng thư mục:

```bash
python ClientLauncher.py localhost 5454 25000 movie.Mjpeg
```

**Giải thích:**
- `localhost`: Địa chỉ server (dùng `localhost` nếu chạy cùng máy)
- `5454`: Port RTSP server
- `25000`: Port RTP nhận dữ liệu video (client)
- `movie.Mjpeg`: Tên file video cần stream

### Bước 3: Sử dụng Client GUI

Cửa sổ client sẽ hiển thị với 4 buttons:

```
┌─────────────────────────────────────────┐
│         [Video Display Area]            │
│                                         │
└─────────────────────────────────────────┘
│ Setup │ Play │ Pause │ Teardown │
└──────────────────────────────────────────┘
```

**Thứ tự thao tác:**
1. Click **Setup** → Thiết lập kết nối với server
2. Click **Play** → Bắt đầu streaming video
3. Click **Pause** → Tạm dừng video (có thể Play lại)
4. Click **Teardown** → Kết thúc phiên và đóng kết nối

## 📊 Kiểm tra hoạt động

### Console Server sẽ hiển thị:
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
```

### Console Client sẽ hiển thị:
```
Data sent:
SETUP movie.Mjpeg RTSP/1.0
CSeq: 1
Transport: RTP/UDP; client_port= 25000

Current Seq Num: 1
Current Seq Num: 2
Current Seq Num: 3
...
```

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

## 🐛 Troubleshooting

### Lỗi: `ModuleNotFoundError: No module named 'PIL'`
**Giải pháp:** Cài đặt Pillow
```bash
pip install Pillow
```

### Lỗi: `[WinError 10048] Only one usage of each socket address`
**Nguyên nhân:** Port đang được sử dụng  
**Giải pháp:** 
- Đổi port khác (vd: 5455, 5456...)
- Hoặc tắt chương trình đang dùng port đó

### Lỗi: `Connection to 'localhost' failed`
**Giải pháp:**
- Kiểm tra server đã chạy chưa
- Kiểm tra port server có đúng không

### Video không hiển thị
**Giải pháp:**
- Kiểm tra file `movie.Mjpeg` có trong thư mục không
- Kiểm tra RTP port (25000) chưa bị chiếm dụng

## 📝 Ghi chú

### RTSP Commands Format

**SETUP:**
```
SETUP movie.Mjpeg RTSP/1.0
CSeq: 1
Transport: RTP/UDP; client_port= 25000
```

**PLAY:**
```
PLAY movie.Mjpeg RTSP/1.0
CSeq: 2
Session: 123456
```

**PAUSE:**
```
PAUSE movie.Mjpeg RTSP/1.0
CSeq: 3
Session: 123456
```

**TEARDOWN:**
```
TEARDOWN movie.Mjpeg RTSP/1.0
CSeq: 4
Session: 123456
```

### RTP Packet Header (12 bytes)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|V=2|P|X|  CC   |M|     PT      |       sequence number         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           timestamp                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           synchronization source (SSRC) identifier            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## 🎯 Phần đã hoàn thành (4/10 điểm)

✅ RTSP Protocol Implementation (Client)  
✅ RTP Packetization (Server)  
✅ Basic GUI với tkinter  
✅ State management (INIT, READY, PLAYING)

## 🔜 Phần nâng cao (sẽ thực hiện)

- [ ] HD Video Streaming (720p/1080p) - 3 điểm
- [ ] Client-Side Caching & Buffering - 2.5 điểm  
- [ ] Report & Documentation - 0.5 điểm

## 📚 Tài liệu tham khảo

- [RFC 2326 - RTSP](https://datatracker.ietf.org/doc/html/rfc2326)
- [RFC 1889 - RTP](https://datatracker.ietf.org/doc/html/rfc1889)
- [Pillow Documentation](https://pillow.readthedocs.io/)

## 📧 Liên hệ

Nếu có vấn đề hoặc câu hỏi, liên hệ qua:
- GitHub Issues: [Link repo issues]
- Email nhóm: [email]

---

**Ghi chú:** Đây là phiên bản cơ bản (4 điểm). Phần nâng cao đang được phát triển.
