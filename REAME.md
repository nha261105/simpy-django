# Simply - Django Project

Ứng dụng web đơn giản được xây dựng bằng Python & Django.

---

## Yêu cầu
- Python 3.12
- Git

---

## 🖥️ Chạy trên Local (Development)

```bash
# 1. Clone project
git clone https://github.com/<username>/simply.git
cd simply

# 2. Tạo và kích hoạt môi trường ảo
python3.12 -m venv myenv
source myenv/bin/activate        # Linux/macOS
# myenv\Scripts\activate         # Windows

# 3. Cài thư viện
pip install -r requirements.txt

# 4. Chạy migrate và khởi động server
cd simply
python3 manage.py migrate
python3 manage.py runserver
```

Truy cập: **http://127.0.0.1:8000**

---

## ☁️ Chạy trên AWS EC2 (Staging)

### Lần đầu setup server

```bash
# 1. SSH vào server (thay bằng IP và tên file .pem của bạn)
ssh -i "simply-key.pem" ubuntu@<EC2_PUBLIC_IP>

# 2. Cập nhật hệ thống
sudo apt update && sudo apt upgrade -y

# 3. Cài các gói cần thiết
sudo apt install python3.12 python3.12-venv git nginx -y
sudo systemctl start nginx && sudo systemctl enable nginx

# 4. Clone project
git clone https://github.com/<username>/simply.git simpy-django
cd simpy-django

# 5. Tạo môi trường ảo và cài thư viện
python3.12 -m venv myenv
source myenv/bin/activate
pip install -r requirements.txt

# 6. Cấu hình ALLOWED_HOSTS
nano simply/simply/settings.py
# Sửa dòng: ALLOWED_HOSTS = ['<EC2_PUBLIC_IP>', 'localhost', '127.0.0.1']

# 7. Chạy server
cd simply
python3 manage.py migrate
python3 manage.py runserver 0.0.0.0:8000
```

Truy cập: **http://\<EC2_PUBLIC_IP\>:8000**

---

### Các lần chạy tiếp theo

```bash
# 1. Bật EC2 trên AWS Console → lấy IP mới

# 2. SSH vào server
ssh -i "simply-key.pem" ubuntu@<IP_MỚI>

# 3. Cập nhật ALLOWED_HOSTS nếu IP thay đổi
nano simpy-django/simply/simply/settings.py

# 4. Kích hoạt môi trường ảo và chạy server
cd simpy-django
source myenv/bin/activate
cd simply
python3 manage.py runserver 0.0.0.0:8000
```

> ⚠️ **Lưu ý:** AWS thay đổi IP public mỗi lần restart instance.
> Dùng **Elastic IP** nếu muốn giữ IP cố định.

---

## 📁 Cấu trúc thư mục

```
simpy-django/
├── myenv/               ← môi trường ảo (không commit lên git)
├── requirements.txt     ← danh sách thư viện
└── simply/              ← thư mục project
    ├── manage.py
    ├── myapp/           ← app chính
    └── simply/
        └── settings.py  ← cấu hình project
```

---

## 🛑 Tắt server
```bash
# Dừng Django
Ctrl+C

# Tắt EC2 (tránh mất phí)
# AWS Console → Instances → Instance state → Stop instance
```
