# Bài tập tuần 1
# Đề bài:
- Tạo một image docker bằng docker-composer, cài đặt vnc-server và một DE - desktop environment (Xfce/GNOME/LXDE/LXQt/KDE/...) cho image đó. Thử remote thông qua trình vnc-viewer trên máy.
- Lưu ý không dùng các image có sẵn các gói nêu trên.
- Được phép cài các package sau khi đã truy cập được máy ảo.
- Gợi ý sử dụng SSH để truy cập máy ảo rồi mới cài DE.

## Yêu cầu

- Docker Desktop đã được cài đặt và đang chạy.

## Hướng dẫn Cài đặt và Sử dụng

### Bước 1: Build và Chạy Container

Mở terminal, di chuyển vào thư mục dự án và chạy lệnh sau:

```bash
docker-compose up --build -d
```

Lệnh này sẽ build image từ `Dockerfile` và khởi chạy container ở chế độ nền.

### Bước 2: Truy cập vào Container qua SSH

Kết nối vào container bằng SSH với tài khoản người dùng được tạo sẵn.

```bash
ssh user@localhost -p 2222
```

- **Mật khẩu:** `1234`
- **Lưu ý:** Nếu bạn đã build lại container và gặp lỗi `REMOTE HOST IDENTIFICATION HAS CHANGED`, hãy chạy lệnh sau trước khi kết nối lại:
  ```bash
  ssh-keygen -R "[localhost]:2222"
  ```

### Bước 3: Cấu hình VNC bên trong Container

Sau khi đã SSH vào container, bạn hãy thực hiện tuần tự các lệnh sau:

1.  **Đặt mật khẩu cho VNC:** (Lệnh này sẽ yêu cầu bạn nhập và xác nhận mật khẩu)
    ```bash
    vncpasswd
    ```

2.  **Tạo file cấu hình `xstartup`:** (Copy và paste cả khối lệnh)
    ```bash
    cat > ~/.vnc/xstartup << EOF
    #!/bin/bash
    unset SESSION_MANAGER
    unset DBUS_SESSION_BUS_ADDRESS
    xfce4-session &
    EOF
    ```

3.  **Cấp quyền thực thi cho file `xstartup`:**
    ```bash
    chmod +x ~/.vnc/xstartup
    ```

4.  **Khởi động VNC Server:**
    ```bash
    tightvncserver :1 -geometry 1280x800 -depth 24
    ```

### Bước 4: Kết nối bằng VNC Viewer

1.  Mở một ứng dụng VNC Viewer bất kỳ trên máy tính của bạn (ví dụ: RealVNC Viewer).
2.  Kết nối đến địa chỉ: `localhost:5901`
3.  Nhập mật khẩu VNC bạn đã tạo ở Bước 3.
4.  Bạn sẽ thấy màn hình desktop XFCE hiện ra.
