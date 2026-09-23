# BÀN GIAO DỰ ÁN: Deploy Tutorial lên simulation2.s4vnrc.org

## 1. TỔNG QUAN DỰ ÁN

### Mục tiêu
Deploy nội dung tutorial series đang chạy thử nghiệm tại:
- **Test site:** https://quangphamsteamforvietnam.github.io/webots-tutorial/
- **Production site:** https://simulation2.s4vnrc.org/Tutorial

Trên navigation bar của simulation2.s4vnrc.org, thêm mục **"Thiết kế mô phỏng"** trỏ đến `/Tutorial`.

### Lưu ý về tên gọi
Trong toàn bộ nội dung hiển thị cho người dùng, từ **"Webots"** đã được thay bằng **"Robotics Simulation"** ở trang chủ (index.html). Tuy nhiên các trang tutorial con (Tutorial 1-4) vẫn giữ nguyên "Webots" vì đó là tên kỹ thuật của phần mềm gốc. Nếu cần đổi tên trong các trang con, cần chỉnh sửa thêm.

---

## 2. SOURCE CODE & REPOSITORY

### GitHub Repository
```
https://github.com/QuangPhamSTEAMforVietNam/webots-tutorial.git
Branch: main
```

### Clone
```bash
git clone https://github.com/QuangPhamSTEAMforVietNam/webots-tutorial.git
```

### Commit history (mới nhất)
```
7c16ebf Remove email and STEAM for Vietnam subtitle
6362f7b Rename Webots to Robotics Simulation on homepage, add download button
0ed8cd6 Add Tutorial 4 card to homepage
0d6697d Add Webots Tutorial 4 with multi-language code tabs, interactive feedback loop, and FlyonUI template
55c96f9 Fix: restore with correct UTF-8 encoding (git checkout)
ebda44a Revert Tutorial 1 to original custom CSS design (pre-TailwindCSS)
```

---

## 3. CẤU TRÚC THƯ MỤC

```
webots-tutorial/
├── index.html                          ← Trang chủ Tutorial Series (deploy tại /Tutorial)
├── Phan 1/                             ← Tutorial 1 assets + HTML
│   ├── Phần 1 Khởi tạo môi trường mô phỏng và robot.html
│   ├── logo_steam.png                  ← Logo STEAM (dùng chung cho header tất cả trang)
│   ├── image1.png ... image17.png      ← Ảnh minh họa
│   ├── images/                         ← Bản sao ảnh (cùng nội dung)
│   ├── thao tac khoi tao mo phong.mp4  ← Video (66MB)
│   ├── thao tac tao controller.mp4     ← Video (38MB)
│   └── *.docx, *.pdf                   ← Tài liệu gốc (không cần deploy)
│
├── Webots Tutorial 2/                  ← Tutorial 2 assets + HTML
│   ├── Phần 2 Chỉnh sửa môi trường mô phỏng.html
│   ├── *.mp4                           ← 6 video (13-67MB mỗi file)
│   └── *.docx                          ← Tài liệu gốc
│
├── Webots Tutorial 3/                  ← Tutorial 3 assets + HTML
│   ├── Phần 3 Thiết kế giao diện môi trường mô phỏng.html
│   ├── *.mp4                           ← 7 video (9-81MB, 1 file 244MB bị gitignore)
│   └── *.docx                          ← Tài liệu gốc
│
├── Webots Tutorial 4/                  ← Tutorial 4 assets + HTML
│   ├── Phần 4 Lập trình Robot Controller cùng Webots.html
│   ├── Webots Tutorial 4/              ← Thư mục con chứa video
│   │   ├── Thực hành 1_tạo file.mp4              (32MB)
│   │   └── Thực hành 1_liên kết epuck_controller.mp4  (28MB)
│   └── *.docx                          ← Tài liệu gốc
│
├── flyonui-free-landing-page/          ← Template FlyonUI (KHÔNG cần deploy)
├── luu/                                ← Backup files (KHÔNG cần deploy)
├── Phan 1 gui/                         ← Backup cũ (KHÔNG cần deploy)
├── .gitignore
└── .gitattributes
```

### Các file/thư mục KHÔNG cần deploy lên production
- `flyonui-free-landing-page/` — template gốc, chỉ dùng để tham khảo
- `luu/` — backup files cũ
- `Phan 1 gui/` — bản backup Tutorial 1
- `*.docx`, `*.pdf`, `*.rar` — tài liệu gốc
- `.git/`, `.gitignore`, `.gitattributes`, `.kilo/`

### Các file CẦN deploy
- `index.html` → mount tại `/Tutorial` hoặc `/Tutorial/index.html`
- `Phan 1/` (toàn bộ thư mục, trừ .docx/.pdf/.rar)
- `Webots Tutorial 2/` (toàn bộ, trừ .docx)
- `Webots Tutorial 3/` (toàn bộ, trừ .docx; lưu ý file `Giải bài tập cũ.mp4` 244MB bị gitignore)
- `Webots Tutorial 4/` (toàn bộ, trừ .docx)

---

## 4. KIẾN TRÚC KỸ THUẬT

### Công nghệ sử dụng
- **100% Static HTML/CSS/JS** — không cần build, không framework, không server-side
- **CSS:** Custom CSS thuần (không dùng Tailwind/FlyonUI ở phiên bản production)
- **Fonts:** Google Fonts (Inter, Quicksand, Fira Code) — load từ CDN
- **Video:** Embedded `<video>` tags, files nằm cùng thư mục
- **JS features:**
  - Scroll reveal animation (IntersectionObserver)
  - Language tab switcher (C/C++/Python/Java/MATLAB)
  - Interactive feedback loop flowchart (SVG animation)
  - Copy code button

### Cách các trang liên kết
```
index.html
  ├── href="Phan 1/Phần 1 ... .html"              → Tutorial 1
  ├── href="Webots Tutorial 2/Phần 2 ... .html"    → Tutorial 2
  ├── href="Webots Tutorial 3/Phần 3 ... .html"    → Tutorial 3
  └── href="Webots Tutorial 4/Phần 4 ... .html"    → Tutorial 4

Mỗi tutorial HTML dùng relative paths cho assets:
  - Ảnh: cùng thư mục (image1.png, image2.png, ...)
  - Video: cùng thư mục hoặc thư mục con (*.mp4)
  - Logo: "../Phan 1/logo_steam.png" (relative path từ tutorial folder)
```

### Nút tải phần mềm
Trên trang chủ có nút "Tải phần mềm" trỏ đến:
```
https://drive.google.com/file/d/1n0wAIRPy7Qj6j12bidEwNIWLm5ERUFMJ/view?usp=sharing
```

---

## 5. TRANG ĐÍCH: simulation2.s4vnrc.org

### Hiện trạng Navigation Bar
Dựa trên HTML fetch được từ simulation2.s4vnrc.org, navigation hiện có:
```
Trang chủ | Khám phá ▾ | Thư viện Robot | Robot Lab | Mô phỏng | Lập trình kéo thả | Digital Twin | Cuộc thi ▾ | Kết nối phần cứng ▾
```

### Yêu cầu thay đổi
1. **Thêm mục "Thiết kế mô phỏng"** vào navigation bar, trỏ đến `/Tutorial`
2. **Deploy static files** tại đường dẫn `/Tutorial/`

### URL mapping sau khi deploy
```
https://simulation2.s4vnrc.org/Tutorial                                    → index.html
https://simulation2.s4vnrc.org/Tutorial/Phan 1/Phần 1 ... .html           → Tutorial 1
https://simulation2.s4vnrc.org/Tutorial/Webots Tutorial 2/Phần 2 ... .html → Tutorial 2
https://simulation2.s4vnrc.org/Tutorial/Webots Tutorial 3/Phần 3 ... .html → Tutorial 3
https://simulation2.s4vnrc.org/Tutorial/Webots Tutorial 4/Phần 4 ... .html → Tutorial 4
```

---

## 6. HƯỚNG DẪN DEPLOY

### Phương án A: Static file serving (Recommended)
Nếu simulation2.s4vnrc.org chạy Next.js/Express/Nginx:

1. Clone repo:
   ```bash
   git clone https://github.com/QuangPhamSTEAMforVietNam/webots-tutorial.git
   ```

2. Copy các file cần thiết vào thư mục `/public/Tutorial/` (hoặc tương đương):
   ```bash
   # Chỉ copy các file production
   cp index.html /public/Tutorial/
   cp -r "Phan 1" /public/Tutorial/
   cp -r "Webots Tutorial 2" /public/Tutorial/
   cp -r "Webots Tutorial 3" /public/Tutorial/
   cp -r "Webots Tutorial 4" /public/Tutorial/
   
   # Xóa các file không cần
   find /public/Tutorial -name "*.docx" -delete
   find /public/Tutorial -name "*.pdf" -delete
   find /public/Tutorial -name "*.rar" -delete
   ```

3. Thêm nav item "Thiết kế mô phỏng" với `href="/Tutorial"` vào layout/header component

### Phương án B: iframe embed
Nếu không thể serve static files trực tiếp:
```html
<iframe src="https://quangphamsteamforvietnam.github.io/webots-tutorial/" 
        style="width:100%; height:100vh; border:none;">
</iframe>
```

### Phương án C: Reverse proxy (Nginx)
```nginx
location /Tutorial/ {
    alias /path/to/webots-tutorial/;
    try_files $uri $uri/ $uri/index.html;
}
```

---

## 7. LƯU Ý QUAN TRỌNG

### Encoding
- Tất cả file HTML dùng **UTF-8** encoding
- Tên file và thư mục chứa **tiếng Việt có dấu** (ví dụ: `Phần 4 Lập trình Robot Controller cùng Webots.html`)
- Server phải hỗ trợ UTF-8 trong URL (percent-encoded)
- Git config đã có `.gitattributes` để xử lý line endings

### Video files
- Tổng dung lượng video: **~900MB**
- File lớn nhất: `Giải bài tập cũ.mp4` (244MB) — đã bị gitignore, cần copy thủ công nếu cần
- Nếu server giới hạn dung lượng, cân nhắc host video trên Google Drive/YouTube và nhúng link

### Relative paths
- Các tutorial HTML dùng **relative paths** cho assets
- Logo header tham chiếu `../Phan 1/logo_steam.png` — nếu thay đổi cấu trúc thư mục, cần update paths
- Video paths cũng relative — không hardcode domain

### Không có build step
- Toàn bộ là static HTML, không cần `npm install`, `npm build`, hay bất kỳ build step nào
- Chỉ cần serve files as-is

---

## 8. DANH MỤC NỘI DUNG 4 TUTORIALS

| # | Tên | File HTML | Chủ đề |
|---|-----|-----------|--------|
| 1 | Bài Mô Phỏng Đầu Tiên | `Phan 1/Phần 1 Khởi tạo...html` | Giao diện, tạo world, E-puck, Controller C |
| 2 | Chỉnh Sửa Môi Trường | `Webots Tutorial 2/Phần 2...html` | Solid Node, DEF-USE, bóng, tường |
| 3 | Thiết Kế Giao Diện | `Webots Tutorial 3/Phần 3...html` | Appearance, PBR, Texturing, ánh sáng |
| 4 | Lập Trình Robot Controller | `Webots Tutorial 4/Phần 4...html` | Controller API, cảm biến, motor, collision avoidance |

### Tính năng đặc biệt của Tutorial 4
- **Tab chuyển đổi đa ngôn ngữ:** C, C++, Python, Java, MATLAB (5 tabs cho mỗi code block)
- **Interactive Feedback Loop:** SVG flowchart animation với Start/Pause/Restart
- **Code copy button:** Sao chép code 1 click

---

## 9. LIÊN HỆ & TÀI KHOẢN

- **GitHub repo owner:** QuangPhamSTEAMforVietNam
- **Repo URL:** https://github.com/QuangPhamSTEAMforVietNam/webots-tutorial
- **Test site (GitHub Pages):** https://quangphamsteamforvietnam.github.io/webots-tutorial/
- **Production target:** https://simulation2.s4vnrc.org/Tutorial
- **Team contact:** hoanghuy.pham@steamforvietnam.org (Phạm Hoàng Huy - Team Simulation & Robotics)
