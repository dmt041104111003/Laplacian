# Phát hiện vết nứt (Crack) trên bề mặt kim loại bằng Laplacian + PCA

Dự án này thực hiện phát hiện **vết nứt (crack)** hoặc **biến dạng** trên bề mặt kim loại (đường hàn, thép, chi tiết cơ khí…) bằng cách kết hợp:

- ✔ **Toán tử Laplacian (∇²f)** – tăng cường chi tiết theo đúng lý thuyết trong giáo trình  
- ✔ **Phát hiện vùng tối** (crack luôn tối hơn nền)  
- ✔ **Connected Components**  
- ✔ **Lọc hình dạng bằng PCA** – chỉ giữ lại crack thật (dài, mảnh), loại bỏ nhiễu  
---

# Giới thiệu
Vết nứt trên kim loại không phải là “biên mảnh” như trong lý thuyết Sobel hoặc Laplacian.  
Chúng thường là:

- vùng **tối**,  
- dạng **rãnh dài – hẹp**,  
- tương phản thấp,  
- nằm trong vùng có nhiều texture kim loại.

Do đó:

### ❌ Laplacian đơn thuần KHÔNG phát hiện được crack  
### ✔ Nhưng Laplacian dùng để tăng cường chi tiết + PCA để lọc hình dạng → phát hiện được crack thật
---

# Pipeline
## 1️⃣ Làm mượt Gaussian  
Giảm nhiễu nhưng giữ đặc trưng hình dạng.
## 2️⃣ Laplacian (8 hướng – đúng theo giáo trình)
Mặt nạ Laplacian:
1 1 1
1 -8 1
1 1 1
Dùng để:
- tăng cường biên,
- làm nổi bật chi tiết,
- đúng vai trò “lọc thông cao” trong giáo trình.
## 3️⃣ Lấy vùng tối (crack mask)  
Crack luôn có cường độ thấp → dùng Otsu để lấy vùng tối.
## 4️⃣ Lấy vùng Laplacian mạnh  
Lấy vùng có thay đổi cường độ mạnh (high-pass).
## 5️⃣ Kết hợp 2 mặt nạ  
CrackMask = DarkMask ∧ LaplacianMask
## 6️⃣ Morphology  
Nối các đoạn crack bị đứt, loại nhiễu nhỏ.
## 7️⃣ PCA Shape Filtering  
Đặc trưng crack:
- Dài – mảnh → λ1 ≫ λ2  
- Nhiễu (blob tròn) → λ1 ≈ λ2
→ PCA loại nhiễu, giữ lại crack.
---
# Laplacian
- Là **đạo hàm bậc 2** của ảnh  
- Là bộ lọc **thông cao (high-pass)**  
- Nhạy với:
  - biên,
  - thay đổi cường độ đột ngột,
  - chi tiết nhỏ  
Laplacian được dùng để:
- cải thiện ảnh (image enhancement),
- làm sắc nét (sharpening),
- làm nổi biên.
---
author: dmt
---
