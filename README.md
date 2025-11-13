# 🎯 6h4T9pTpR0 CTF Team Website

Website chuyên nghiệp cho team CTF **6h4T9pTpR0** từ **Đại học HUTECH** - Được thiết kế với giao diện hiện đại, hiệu ứng hacker aesthetic, axolotl mascot cute, và background matrix đầy sắc màu! 🌈

---

## 📋 Cấu Trúc Project

```
CTFTeamSite/
├── app.py                  # Flask backend - routes & dữ liệu team
├── requirements.txt        # Python dependencies
├── templates/              # HTML templates (Jinja2)
│   ├── index.html         # Trang chủ (hero + club + team preview)
│   ├── team.html          # Danh sách thành viên
│   ├── writeups.html      # Danh sách writeups
│   ├── 404.html           # Error page
│   └── 500.html           # Error page
├── static/
│   ├── css/style.css      # All animations & responsive design (1356+ lines)
│   ├── js/main.js         # Axolotl + cursor effects + matrix background
│   └── img/               # Logos & avatars
└── README.md
```

---

## 🚀 Chạy Cục Bộ (Windows)

### 1️⃣ Cài Đặt PHP & Web Server

**Option A: XAMPP** (Easy)
```
1. Download XAMPP: https://www.apachefriends.org/
2. Cài đặt (chọn Apache + PHP)
3. Copy project vào C:\xampp\htdocs\ctf-website\
4. Start Apache từ XAMPP Control Panel
5. Mở http://localhost/ctf-website/
```

**Option B: Built-in PHP Server** (Quick)
```powershell
cd d:\VSCode\src\CTFTeamSite
php -S 127.0.0.1:8000
```
Mở http://127.0.0.1:8000 trong browser 🌐

### 2️⃣ Yêu Cầu

- **PHP 7.4+**
- **Apache** (với mod_rewrite) hoặc **PHP Built-in Server**
- **.htaccess** support (nếu dùng Apache)

---

## 🎨 Features

✨ **Modern Design**
- Rainbow gradient animations trên tất cả headings
- Matrix background code với hex codes (0xdeadbeef, 0xcafebabe, etc.)
- Responsive design (desktop, tablet, mobile)

🐙 **Cute Mascot**
- Pink axolotl cursor follower với blink & gill animations
- Smooth mouse tracking với easing effect

🕸️ **Cursor Effects**
- Trail particles tạo web connections
- Click burst effect với 12 particles
- Dense hacker aesthetic

🎭 **20+ CSS Animations**
- rainbow-text, rainbow-border, neon-glow, glow-pulse
- matrix-flow, slide-in, float-y, float-particle, reveal
- color-shift, pulse-glow, hero-fade-in, stat-counter, trail-fade

---

## 📝 Chỉnh Sửa Dữ Liệu

Tất cả dữ liệu team được lưu trong `app.py`:

### Team Info
```python
TEAM = {
    "name": "6h4T9pTpR0",
    "tagline": "Security Through Capture The Flag",
    "ctftime": "https://ctftime.org/team/412747/",
    "members": [
        {"name": "BaoZ", "role": "Team Lead/Forensics", "handle": "BaoZ", "avatar": "/static/img/BaoZ.jpg"},
        # Thêm thành viên ở đây
    ],
    "socials": {
        "twitter": "https://twitter.com/...",
        "github": "https://github.com/...",
    }
}
```

### Club & University Info
```python
CLUB = {
    "name": "Câu lạc bộ An Ninh Mạng HUTECH",
    "description": "...",
    "achievements": [...]
}

UNIVERSITY = {
    "name": "Đại học HUTECH",
    "website": "https://www.hutech.edu.vn",
    # ...
}
```

---

## 🌍 Deploy Lên Host (VPS/Web Hosting PHP)

### 📤 Option 1: Upload via FTP (Hosting chia sẻ)

#### Lần Đầu Setup

```
1. Download FileZilla hoặc WinSCP
2. FTP vào host (host cung cấp credentials)
3. Upload toàn bộ files:
   - index.php
   - .htaccess
   - templates/ (tất cả .php files)
   - static/ (css + js + img)
4. Set permissions: chmod 755 trên folders, 644 trên files
5. Visit http://your-domain.com
```

#### Update Code

```
1. Sửa file locally
2. Upload lại via FTP
   - Chỉ upload file changed, hoặc
   - Upload entire project nếu muốn an toàn

3. Refresh browser (Ctrl+Shift+R để clear cache)
```

---

### 🔧 Option 2: SSH + Git (VPS riêng)

#### Lần Đầu Setup

```bash
# SSH vào VPS
ssh user@your-vps.com

# Clone repository
cd /home/user/public_html  # hoặc /var/www/html
git clone <repo-url> ctf-website
cd ctf-website

# Verify PHP enabled (should see version)
php -v

# Set permissions
chmod 755 .
chmod 644 index.php .htaccess templates/*.php
chmod 755 static static/css static/js static/img
```

#### Update Code

```bash
# Trên local machine
git add .
git commit -m "Fix: [description]"
git push origin main

# Trên VPS
cd /home/user/public_html/ctf-website
git pull origin main
# Done! No restart needed (PHP is stateless)
```

#### Auto-Update (Webhook)

```bash
# Cài webhook listener (optional, tùy hosting)
# Hoặc dùng cron job:
*/10 * * * * cd /home/user/public_html/ctf-website && git pull origin main > /dev/null 2>&1
```

---

### 🌐 Option 3: Apache Configuration

#### Verify mod_rewrite

```bash
# SSH vào VPS
sudo a2enmod rewrite
sudo systemctl restart apache2
```

#### .htaccess Setup

File `.htaccess` đã có sẵn - nó sẽ:
- Route `/team` → `?page=team`
- Route `/writeups` → `?page=writeups`
- Route `/api/team.json` → `?page=api-team`

#### Virtual Host (Optional)

```bash
sudo nano /etc/apache2/sites-available/ctf-website.conf
```

Copy & paste:
```apache
<VirtualHost *:80>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /home/user/public_html/ctf-website

    <Directory /home/user/public_html/ctf-website>
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php-fpm.sock|fcgi://localhost"
    </FilesMatch>
</VirtualHost>
```

Enable & test:
```bash
sudo a2ensite ctf-website
sudo apache2ctl configtest
sudo systemctl restart apache2
```

---

### 🔐 SSL/HTTPS với Let's Encrypt

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
sudo systemctl reload nginx
```

Certbot sẽ tự động config SSL! ✅

---

## ⚡ Update Code Workflow

### PHP Advantage: No Restart Needed! 🚀

Khác với Python/Flask, PHP không cần restart - cứ push code lên là hoạt động ngay!

**Mỗi lần sửa code:**

```powershell
# Local machine
1. Sửa file (.php, CSS, JS)
2. Test locally: php -S 127.0.0.1:8000
3. Push lên git:
   git add .
   git commit -m "Fix: [description]"
   git push origin main
```

**Trên host:**

```bash
# SSH vào VPS
cd /home/user/public_html/ctf-website
git pull origin main
# DONE! Không cần restart Apache
```

Hoặc chỉ dùng **FTP upload** nếu không có Git:
- Sửa file → Upload via FileZilla → Refresh browser ✅

---

## 🐛 Troubleshooting

**Port conflict?**
```powershell
# Find what's using port 5000
netstat -ano | findstr :5000
# Kill it
taskkill /PID <PID> /F
```

**Module not found?**
```bash
pip install -r requirements.txt --upgrade
```

**Static files broken?**
- Verify `/static/` path in templates
- Check `static/css/`, `static/js/`, `static/img/` exist

**Animations not working?**
- Open DevTools (F12) → Console
- Check for JavaScript errors
- Verify `main.js` loads

---

## 📚 Technology Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Flask (Python) |
| **Frontend** | HTML5, CSS3 (1356+ lines), JavaScript ES6+ |
| **Server** | Gunicorn (app), Nginx (reverse proxy) |
| **Animations** | 20+ CSS keyframes + RequestAnimationFrame |
| **SSL** | Let's Encrypt (Free HTTPS) |
| **Deployment** | Linux VPS / Web Hosting |

---

## 📞 Team Info

- **Team Name:** 6h4T9pTpR0
- **Organization:** Câu lạc bộ An Ninh Mạng HUTECH
- **University:** Đại học HUTECH, TP.HCM
- **CTFtime:** https://ctftime.org/team/412747/
- **Founded:** 2023

---

**Last Updated:** November 13, 2025 ✨
**Made with 💚 for cybersecurity enthusiasts**
