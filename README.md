# Weekly Tracking · Dashboard 3AM & SCA

Dashboard quản trị doanh thu, lợi nhuận và hiệu suất KTV cho hệ thống 3AM D'SPA + SCA Academy.

🔗 **Live**: https://siliciouslee2912.github.io/weekly-tracking/dashboard-tuan.html

## Tính năng

- 📊 **Tổng quan** — doanh thu/khách theo tháng & tuần (4 cơ sở)
- 💰 **Lợi nhuận (P&L)** — drill-down doanh thu, chi phí, margin theo từng cơ sở
- 👥 **Hiệu suất KTV** — số ca, lương, dịch vụ phổ biến, top KH của từng kỹ thuật viên
- 🔐 Đăng nhập password chung (SHA-256 hash, nhớ 30 ngày)
- ☁️ Firebase Firestore làm database (sync real-time giữa nhiều thiết bị)

## Setup Firebase (cần làm 1 lần)

### 1. Bật Firestore Database
Firebase Console → Build → Firestore Database → Create database → Region `asia-southeast1` → Test mode

### 2. Bật Anonymous Authentication
Firebase Console → Build → Authentication → Get started → Sign-in method → **Anonymous** → Enable

### 3. Add domain GitHub Pages vào danh sách trusted
Authentication → Settings → tab "Authorized domains" → Add `siliciouslee2912.github.io`

### 4. Apply Firestore Security Rules
Firestore Database → Rules → copy nội dung từ file `firestore.rules` trong repo này → Publish

## Cấu trúc data trên Firestore

```
/dashboard/main  (single doc, ~60KB)
  payload: {
    branches: [...],     // 4 cơ sở
    weekly: [...],       // weekly summary records
    pnl: {...},          // P&L data theo tháng (T1/2025 → T2/2026)
    ktv: {...},          // KTV aggregates
    sca: {...},
    _seedVersion: "..."
  }
  _updatedAt: serverTimestamp
```

## Đổi password

Password được hash SHA-256 và lưu hardcoded trong `dashboard-tuan.html` (biến `PASSWORD_HASH`).

Để đổi:
```bash
echo -n "MAT_KHAU_MOI" | sha256sum
```
Lấy chuỗi hash xuất ra, paste vào biến `PASSWORD_HASH` trong file HTML, push GitHub.

## Phát triển

File chính: `dashboard-tuan.html` (single-file SPA, không build step).
