
## Thông tin sinh viên

| Trường          | Giá trị                               |
|-----------------|---------------------------------------|
| Họ tên          | Nguyễn Đăng Dương                      |
| Linux username  | `nguyendanggduong-k24cntt2`               |
| Lớp             | K24 CNTT2                             |
| Trường          | PTIT                                  |
| Repository      | `devops-hackathon-de003-nguyendangduong` |
| Nhánh           | `main`                                |
| PORT cá nhân    | **8082**                              |

---

## Cấu trúc repository

```
devops-hackathon-de003-nguyenđanguong/
├── src/
│   └── index.html                  ← Trang web thông tin cá nhân
├── nginx/
│   └── nguyendanggduong-k24cntt2.conf  ← Server block Nginx
├── screenshots/
│   ├── 01-user.png
│   ├── 02-software.png
│   ├── 03-git-clone.png
│   ├── 04-nginx-config.png
│   ├── 05-ufw.png
│   └── 06-browser.png
├── .gitignore
└── README.md
```

---

## Thông số triển khai

| Thông số            | Giá trị                                                       |
|---------------------|---------------------------------------------------------------|
| Hệ điều hành        | Ubuntu 24.04                                                  |
| Thư mục triển khai  | `/var/www/devops-hackathon-de003/`                |
| Web root (Nginx)    | `/var/www/devops-hackathon-de003/src`            |
| Cổng Nginx (PORT)   | `8082`                                                        |
| `server_name`       | Địa chỉ IP máy chủ (xem `hostname -I`)                       |
| File server block   | `nguyendanggduong-k24cntt2.conf`                                  |
| UFW rule            | `sudo ufw allow 8082/tcp`                                     |
| URL truy cập        | `http://<IP_máy_chủ>:8082`                                    |

---

## Hướng dẫn triển khai

### Phần 1 – Chuẩn bị môi trường

#### 1.1 Tạo tài khoản người dùng

```bash
# Tạo user với home dir và shell /bin/bash
sudo useradd -m -s /bin/bash nguyendanggduong-k24cntt2

# Đặt mật khẩu
sudo passwd nguyendanggduong-k24cntt2

# Thêm vào group sudo (secondary group)
sudo usermod -aG sudo nguyendanggduong-k24cntt2

# Kiểm tra
id nguyendanggduong-k24cntt2
```

#### 1.2 Cài đặt phần mềm

```bash
sudo apt update && sudo apt install -y nginx git ufw curl

# Bật và khởi động Nginx
sudo systemctl enable nginx
sudo systemctl start nginx

# Cấu hình Git
git config --global user.name "pedguedes090)"
git config --global user.email "pedguedes090@outlook.com "
```

---

### Phần 2 – Quản lý mã nguồn với Git/GitHub

```bash
# Clone repository về máy chủ
cd /var/www/devops-hackathon-de003
git clone https://github.com/pedguedes090/devops-hackathon-de003-nguyendangduong.git .

# Hoặc pull khi đã clone rồi
git pull origin main
```

---

### Phần 3 – Cấu hình Nginx

```bash
# Copy server block vào sites-available
sudo cp nginx/nguyendanggduong-k24cntt2.conf /etc/nginx/sites-available/nguyendanggduong-k24cntt2.conf

# Tạo symlink sang sites-enabled
sudo ln -s /etc/nginx/sites-available/nguyendanggduong-k24cntt2.conf \
           /etc/nginx/sites-enabled/nguyendanggduong-k24cntt2.conf

# Xoá site default để tránh xung đột cổng 80
sudo rm -f /etc/nginx/sites-enabled/default

# Kiểm tra cú pháp
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx
```

---

### Phần 4 – Cấu hình UFW Firewall

```bash
# Cho phép SSH (giữ kết nối)
sudo ufw allow OpenSSH

# Cho phép cổng cá nhân
sudo ufw allow 8082/tcp

# Bật UFW
sudo ufw enable

# Kiểm tra
sudo ufw status verbose
```

---

### Kết quả

Truy cập website tại: `http://<IP_máy_chủ>:8082`

---

## Ảnh minh chứng

| File            | Nội dung                                        |
|-----------------|-------------------------------------------------|
| `01-user.png`   | Kết quả lệnh `id` và `whoami`                   |
| `02-software.png` | Nginx active + enabled, git/ufw/curl đã cài   |
| `03-git-clone.png` | Kết quả `git clone` hoặc `git pull`          |
| `04-nginx-config.png` | Nội dung file `.conf` và `nginx -t` OK    |
| `05-ufw.png`    | Kết quả `ufw status verbose`                    |
| `06-browser.png`| Trang web hiển thị đúng trên trình duyệt        |