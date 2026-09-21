# Domain

```mermaid
classDiagram
    namespace DinhDanh_PhanQuyen {
        class TaiKhoan {
            <<AggregateRoot>>
            +maTaiKhoan
            +tenDangNhap
            +matKhauBam
            +loaiNguoiDung
            +trangThai
        }
        class VaiTro {
            <<Entity>>
            +tenVaiTro
        }
        class Quyen {
            <<ValueObject>>
            +chucNang
            +thaoTac
        }
        class NhatKyThaoTac {
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
        class KhachHang {
            <<AggregateRoot>>
            +maKhachHang
            +maTaiKhoan
            +hoTen
            +soDienThoai
            +email
            +trangThai
        }
        class PhuongThucDaLuu {
            <<ValueObject>>
            +loai
            +maThamChieu
        }
    }
    KhachHang "1" *-- "0..*" PhuongThucDaLuu
    KhachHang ..> TaiKhoan : maTaiKhoan

    namespace TaiXe_PhuongTien {
        class TaiXe {
            <<AggregateRoot>>
            +maTaiXe
            +maTaiKhoan
            +hoSo
            +trangThai
        }
        class PhuongTien {
            <<Entity>>
            +maPhuongTien
            +bienSo
            +hangXe
            +maLoaiXe
        }
        class LoaiXe {
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
        class ViTriTaiXe {
            <<AggregateRoot>>
            +maTaiXe
            +toaDo
            +doChinhXac
            +thoiDiemCapNhat
        }
    }
    ViTriTaiXe ..> TaiXe : maTaiXe (tham chiếu, KHÔNG composition)

    namespace DatXe {
        class YeuCauDatXe {
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
        class QuaTrinhPhanCong {
            <<AggregateRoot>>
            +maPhanCong
            +maYeuCau
            +diemDon
            +maLoaiXe
            +trangThai
        }
        class LoiMoi {
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
        class ChuyenDi {
            <<AggregateRoot>>
            +maChuyen
            +maYeuCau
            +maKhachHang
            +maTaiXe
            +maPhuongTien
            +trangThai
        }
        class DiaDiem {
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
        class CuocPhi {
            <<AggregateRoot>>
            +maCuoc
            +maChuyen
            +tongTien
        }
    }
    CuocPhi ..> ChuyenDi : maChuyen (qua sự kiện TomTatChuyen)

    namespace XuLyThanhToan {
        class ThanhToan {
            <<AggregateRoot>>
            +maThanhToan
            +maChuyen
            +maKhachHang
            +phuongThuc
            +trangThai
            +soLanThu
        }
        class GiaoDich {
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
        class ThongBao {
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
        class DanhGia {
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
        class SuCo {
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
        class TongHopChuyenTheoNgay {
            <<ReadModel>>
            +ngay
            +soChuyen
            +soHoanThanh
            +soHuy
        }
        class DoanhThuTheoNgay {
            <<ReadModel>>
            +ngay
            +doanhThu
        }
        class HieuQuaTaiXe {
            <<ReadModel>>
            +maTaiXe
            +tyLeTuChoi
            +diemDanhGiaTrungBinh
        }
    }
