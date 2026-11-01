# CSN-DA24TTA - Nguyễn Minh Khang - Quản Lý Nhà Trọ Sinh Viên (SQL Server)

## Đề tài: Xây dựng cơ sở dữ liệu quản lý Nhà trọ sinh viên và viết các câu truy vấn minh họa
![Badge](https://img.shields.io/badge/Status-In%20Progress%20(Tu%E1%BA%A7n%201)-yellow) ![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white) ![T-SQL](https://img.shields.io/badge/T--SQL-0078D4?style=flat&logo=databricks&logoColor=white)

## 1. THÔNG TIN TÁC GIẢ
- **Giảng viên hướng dẫn:** ThS. Võ Thành C

- **Sinh viên thực hiện:** Nguyễn Minh Khang
- **Mã số sinh viên:** 110124083
- **Mã lớp:** DA24TTA
- **Liên hệ:** 110124083@st.tvu.edu.vn

---

## 2. GIỚI THIỆU ĐỒ ÁN & CHỨC NĂNG CHÍNH
### **Mô tả ngắn:**

Đồ án xây dựng cơ sở dữ liệu quan hệ phục vụ quản lý nhà trọ sinh viên trên địa bàn tỉnh Vĩnh Long: chủ trọ, nhà trọ, phòng trọ, sinh viên thuê trọ, hợp đồng thuê, hóa đơn điện nước hằng tháng và công nợ. Kèm theo là bộ câu truy vấn SQL minh họa cho tra cứu, xử lý nghiệp vụ và thống kê.

### **Các chức năng chính (theo yêu cầu quản lý đã phân tích ở tuần 1):**
- Quản lý chủ trọ và nhà trọ: một chủ trọ có nhiều nhà trọ, theo dõi trạng thái hoạt động, bảng giá điện/nước có lịch sử thay đổi.

- Quản lý phòng trọ: loại phòng (có gác, có máy lạnh), diện tích, sức chứa, giá thuê, trạng thái (Còn trống / Đang được thuê / Đang sửa chữa).
- Quản lý sinh viên thuê trọ: thông tin cá nhân, học tập (lớp/khoa/trường), trạng thái đăng ký tạm trú.
- Hợp đồng thuê: người đại diện đứng tên, ở ghép (không vượt sức chứa), gia hạn, chuyển phòng, thanh lý và hoàn cọc, lưu lịch sử thuê.
- Hóa đơn hằng tháng: chốt chỉ số điện/nước, tính tiền phòng + điện + nước + dịch vụ (rác, giữ xe, ...).
- Thanh toán & công nợ: thanh toán nhiều lần, trạng thái Chưa thanh toán / Đã thanh toán / Thanh toán một phần.
- Tra cứu: phòng trống theo khu vực, khoảng giá, diện tích, loại phòng; sinh viên đang ở đâu; hợp đồng sắp hết hạn.
- Thống kê: số phòng theo trạng thái, số sinh viên theo trường/quê quán, điện nước tiêu thụ, doanh thu theo tháng/năm, danh sách nợ quá hạn.

### **Thiết kế dữ liệu (đã xác định ở tuần 1):**
| # | Bảng | Ý nghĩa |
|---|------|---------|
| 1 | `Chu_nha_tro` | Chủ nhà trọ |
| 2 | `Phuong_xa` | Danh mục phường/xã tại Vĩnh Long |
| 3 | `Nha_tro` | Nhà trọ (khu trọ) |
| 4 | `Bang_gia_dien_nuoc` | Đơn giá điện/nước theo thời gian của từng nhà trọ |
| 5 | `Loai_phong` | Loại phòng (có gác, có máy lạnh) |
| 6 | `Phong_tro` | Phòng trọ |
| 7 | `Sinh_vien` | Sinh viên (khách thuê) |
| 8 | `Hop_dong_thue` | Hợp đồng thuê phòng |
| 9 | `Chi_tiet_o_ghep` | Sinh viên lưu trú theo hợp đồng (đại diện / ở ghép) |
| 10 | `Hoa_don` | Hóa đơn theo kỳ |
| 11 | `Thanh_toan` | Các lần thanh toán của hóa đơn |
| 12 | `Dich_vu` | Dịch vụ đi kèm |
| 13 | `Chi_tiet_dich_vu` | Dịch vụ sử dụng trong từng hóa đơn |

Chi tiết thuộc tính, kiểu dữ liệu, 9 mối quan hệ và các ràng buộc toàn vẹn: xem `docs/Chi_Tiet_Thuc_The_Rang_buoc_Quan_he.docx`.

---

## 3. CÔNG NGHỆ SỬ DỤNG
- Microsoft SQL Server (2019 trở lên) + SQL Server Management Studio (SSMS)

- T-SQL: bảng, khóa chính/ngoại, CHECK, UNIQUE, view, stored procedure, trigger
- Công cụ vẽ ERD: draw.io / StarUML (tuần 2)

**Cấu trúc thư mục chính (dự kiến, cập nhật dần theo tiến độ):**
```
.
├── docs/                          # Tài liệu đồ án
│   ├── DeCuong_CSN.docx           # Đề cương chi tiết
│   ├── BaoCaoTienDo_TuanX.docx    # Báo cáo tiến độ từng tuần
│   ├── Yeu_cau_quan_ly.docx       # Phân tích yêu cầu quản lý (tuần 1)
│   ├── Chi_Tiet_Thuc_The_...docx  # Thực thể, ràng buộc, quan hệ (tuần 1)
│   └── ERD.png                    # Sơ đồ thực thể - liên kết (tuần 2)
├── database/                      # Script SQL (tuần 3)
│   ├── 01_create_database.sql     # Tạo CSDL
│   ├── 02_create_tables.sql       # Tạo bảng, khóa chính, khóa ngoại
│   ├── 03_constraints.sql         # CHECK, UNIQUE, trigger ràng buộc nghiệp vụ
│   ├── 04_seed_data.sql           # Dữ liệu mẫu
│   └── 05_views_procedures.sql    # View, stored procedure nghiệp vụ
├── queries/                       # Bộ câu truy vấn minh họa (tuần 3)
│   ├── 01_tra_cuu.sql             # Tra cứu nhà trọ, phòng, sinh viên, hợp đồng
│   ├── 02_nghiep_vu.sql           # Tìm phòng trống, lập hóa đơn, thanh toán
│   └── 03_thong_ke.sql            # Doanh thu, công nợ, điện nước
└── README.md                      # Giới thiệu đồ án
```

---

## 4. TIẾN ĐỘ THỰC HIỆN 📅
| Tuần | Thời gian | Nội dung | Trạng thái |
|------|-----------|----------|------------|
| 1 | 12/10/2026 – 18/10/2026 | Tìm hiểu, phân tích nghiệp vụ quản lý nhà trọ; xác định thực thể, thuộc tính và mối quan hệ | ✅ Hoàn thành |
| 2 | 02/11/2026 – 08/11/2026 | Xây dựng ERD và mô hình quan hệ; chuẩn hóa và hoàn thiện thiết kế các bảng | ⏳ Đang thực hiện |
| 3 | 23/11/2026 – 29/11/2026 | Tạo CSDL, bảng, ràng buộc, nạp dữ liệu mẫu; viết truy vấn cơ bản, nghiệp vụ, thống kê; kiểm thử | ⬜ Chưa bắt đầu |
| 4 | 14/12/2026 – 20/12/2026 | Hoàn thiện báo cáo và chuẩn bị trình bày | ⬜ Chưa bắt đầu |

### Kết quả tuần 1
- Phân tích xong yêu cầu quản lý: nhà trọ, phòng trọ, sinh viên, hợp đồng thuê, hóa đơn – thanh toán, tra cứu và thống kê.
- Xác định 13 bảng (10 thực thể chính và 3 bảng trung gian/chi tiết) cùng thuộc tính, kiểu dữ liệu, khóa.
- Xác định 9 mối quan hệ (2 quan hệ n–n tách thành bảng trung gian `Chi_tiet_o_ghep`, `Chi_tiet_dich_vu`).
- Xác định các ràng buộc toàn vẹn: khóa, tham chiếu, miền giá trị, ràng buộc liên thuộc tính (chu kỳ hóa đơn, chỉ số điện nước, thanh toán, vòng đời hợp đồng).

---

## 5. HƯỚNG DẪN CÀI ĐẶT (Installation Guide) ⚙️
> Phần này sẽ dùng được từ tuần 3, khi các script SQL hoàn thành.

### Yêu cầu
- Microsoft SQL Server 2019+ (Express hoặc Developer)
- SQL Server Management Studio (SSMS) hoặc Azure Data Studio

### Các bước cài đặt
1. Clone repository:
```bash
git clone <repo-url>
cd <project-folder>
```

2. Mở SSMS, kết nối tới SQL Server và chạy lần lượt các script trong thư mục `database/`:
```
01_create_database.sql
02_create_tables.sql
03_constraints.sql
04_seed_data.sql
05_views_procedures.sql
```

3. Chạy các câu truy vấn minh họa trong thư mục `queries/` để xem kết quả tra cứu và thống kê.
