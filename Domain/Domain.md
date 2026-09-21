# Domain

```mermaid
classDiagram
    %% Định nghĩa bảng màu Pastel phân loại theo DDD giống thiết kế chuẩn
    classDef cGreen fill:#E8F8F0,stroke:#27AE60,stroke-width:1.5px,color:#1E3A2F;
    classDef cPink fill:#FDEDEC,stroke:#E74C3C,stroke-width:1.5px,color:#4A1515;
    classDef cBlue fill:#EBF5FB,stroke:#3498DB,stroke-width:1.5px,color:#1B3854;
    classDef cYellow fill:#FEFDE8,stroke:#F1C40F,stroke-width:1.5px,color:#4D3E02;
    classDef cPurple fill:#F4ECF7,stroke:#8E44AD,stroke-width:1.5px,color:#3D184E;
    classDef cOrange fill:#FDF2E9,stroke:#E67E22,stroke-width:1.5px,color:#4E2606;
    classDef cCyan fill:#E0F7FA,stroke:#00ACC1,stroke-width:1.5px,color:#00363A;

    namespace DinhDanh_PhanQuyen {
        class TaiKhoan:::cGreen {
            <<AggregateRoot>>
            +maTaiKhoan
            +tenDangNhap
            +matKhauBam
            +loaiNguoiDung
            +trangThai
        }
        class VaiTro:::cGreen {
            <<Entity>>
            +tenVaiTro
        }
        class Quyen:::cGreen {
            <<ValueObject>>
            +chucNang
            +thaoTac
        }
        class NhatKyThaoTac:::cPurple {
            <<Entity>>
            +maTaiKhoan
            +hanhDong
            +thoiDiem
            +ketQua
        }
    }
    TaiKhoan "1" --> "0..*" VaiTro : có
    VaiTro "1" *-- "1..*" Quyen : gồm
    TaiKhoan ..> NhatKyThaoTac : ghi vết

    namespace QuanLyKhachHang {
        class KhachHang:::cBlue {
            <<AggregateRoot>>
            +maKhachHang
            +maTaiKhoan
            +hoTen
            +soDienThoai
            +email
            +trangThai
        }
        class PhuongThucDaLuu:::cBlue {
            <<ValueObject>>
            +loai
            +maThamChieu
        }
    }
    KhachHang "1" *-- "0..*" PhuongThucDaLuu
    KhachHang ..> TaiKhoan : maTaiKhoan

    namespace TaiXe_PhuongTien {
        class TaiXe:::cYellow {
            <<AggregateRoot>>
            +maTaiXe
            +maTaiKhoan
            +hoSo
            +trangThai
        }
        class PhuongTien:::cOrange {
            <<Entity>>
            +maPhuongTien
            +bienSo
            +hangXe
            +maLoaiXe
        }
        class LoaiXe:::cOrange {
            <<Entity>>
            +maLoaiXe
            +tenLoaiXe
            +soChoNgoi
        }
    }
    TaiXe "1" *-- "1..*" PhuongTien : sở hữu
    PhuongTien "*" --> "1" LoaiXe : thuộc loại
    TaiXe ..> TaiKhoan : maTaiKhoan

    namespace DinhViTaiXe {
        class ViTriTaiXe:::cBlue {
            <<AggregateRoot>>
            +maTaiXe
            +toaDo
            +doChinhXac
            +thoiDiemCapNhat
        }
    }
    ViTriTaiXe ..> TaiXe : maTaiXe (tham chiếu)

    namespace DatXe {
        class YeuCauDatXe:::cPink {
            <<AggregateRoot>>
            +maYeuCau
            +maKhachHang
            +diemDon
            +diemDen
            +maLoaiXe
            +trangThai
            +thoiGianTao
        }
    }
    YeuCauDatXe ..> KhachHang : maKhachHang

    namespace PhanCongTaiXe {
        class QuaTrinhPhanCong:::cPink {
            <<AggregateRoot>>
            +maPhanCong
            +maYeuCau
            +diemDon
            +maLoaiXe
            +trangThai
        }
        class LoiMoi:::cPurple {
            <<Entity>>
            +maLoiMoi
            +maTaiXe
            +trangThai
            +moiLuc
            +hanPhanHoi
        }
    }
    QuaTrinhPhanCong "1" *-- "0..*" LoiMoi
    QuaTrinhPhanCong ..> YeuCauDatXe : maYeuCau
    LoiMoi ..> TaiXe : maTaiXe

    namespace QuanLyChuyenDi {
        class ChuyenDi:::cPink {
            <<AggregateRoot>>
            +maChuyen
            +maYeuCau
            +maKhachHang
            +maTaiXe
            +maPhuongTien
            +trangThai
        }
        class DiaDiem:::cYellow {
            <<ValueObject>>
            +diaChi
            +toaDo
        }
    }
    ChuyenDi *-- DiaDiem : điểm đón / đến
    ChuyenDi ..> QuaTrinhPhanCong : tạo sau ĐãPhânCôngTàiXế
    ChuyenDi ..> KhachHang : maKhachHang
    ChuyenDi ..> TaiXe : maTaiXe
    ChuyenDi ..> PhuongTien : maPhuongTien

    namespace TinhCuoc {
        class CuocPhi:::cCyan {
            <<AggregateRoot>>
            +maCuoc
            +maChuyen
            +tongTien
        }
    }
    CuocPhi ..> ChuyenDi : maChuyen

    namespace XuLyThanhToan {
        class ThanhToan:::cGreen {
            <<AggregateRoot>>
            +maThanhToan
            +maChuyen
            +maKhachHang
            +phuongThuc
            +trangThai
            +soLanThu
        }
        class GiaoDich:::cPurple {
            <<Entity>>
            +maGiaoDich
            +maThamChieuBenThuBa
            +ketQua
        }
    }
    ThanhToan "1" *-- "0..*" GiaoDich
    ThanhToan ..> CuocPhi : maCuoc
    ThanhToan ..> KhachHang : maKhachHang

    namespace DichVuThongBao {
        class ThongBao:::cPurple {
            <<AggregateRoot>>
            +maThongBao
            +nguoiNhan
            +loaiSuKien
            +noiDung
            +trangThai
        }
    }
    ThongBao ..> KhachHang : maNguoiNhan
    ThongBao ..> TaiXe : maNguoiNhan

    namespace PhanHoiDanhGia {
        class DanhGia:::cCyan {
            <<AggregateRoot>>
            +maDanhGia
            +maChuyen
            +maKhachHang
            +maTaiXe
            +diem
            +nhanXet
        }
    }
    DanhGia ..> ChuyenDi : maChuyen

    namespace VanHanh {
        class SuCo:::cPink {
            <<AggregateRoot>>
            +maSuCo
            +maChuyen
            +trangThai
            +nguoiXuLy
        }
    }
    SuCo ..> ChuyenDi : maChuyen
    SuCo ..> TaiKhoan : maNhanVienXuLy

    namespace BaoCao {
        class TongHopChuyenTheoNgay:::cBlue {
            <<ReadModel>>
            +ngay
            +soChuyen
            +soHoanThanh
            +soHuy
        }
        class DoanhThuTheoNgay:::cGreen {
            <<ReadModel>>
            +ngay
            +doanhThu
        }
        class HieuQuaTaiXe:::cYellow {
            <<ReadModel>>
            +maTaiXe
            +tyLeTuChoi
            +diemDanhGiaTrungBinh
        }
    }
