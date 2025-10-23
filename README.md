# -*- coding: utf-8 -*-
import sys
import os
import time

# ------------------- KHỞI TẠO CÁC THÀNH PHẦN CẦN THIẾT -------------------

# Lớp chứa mã màu ANSI để làm đẹp chữ trên terminal
class Mau:
    RESET = '\033[0m'
    HONG_NHAT = '\033[38;2;255;182;193m'
    XANH_NHAT = '\033[38;2;173;216;230m'
    VANG_NHAT = '\033[38;2;255;255;224m'
    TIM_NHAT = '\033[38;2;221;160;221m'
    OAI_HUONG = '\033[38;2;230;230;250m'
    BAC_HA = '\033[38;2;152;251;152m'
    XANH_LUC_NHAT = '\033[38;2;175;238;238m'

# ------------------- CÁC HÀM CHỨC NĂNG -------------------

def xoa_man_hinh():
    """Hàm này xóa sạch màn hình terminal."""
    os.system('cls' if os.name == 'nt' else 'clear')

def hieu_ung_chu(lyrics_list, char_speed_list, pause_list):
    """
    Hàm hiển thị từng dòng lời bài hát với hiệu ứng gõ chữ và đổi màu.
    """
    mau_sac = [
        Mau.XANH_NHAT, Mau.TIM_NHAT, Mau.OAI_HUONG, Mau.BAC_HA,
        Mau.HONG_NHAT, Mau.XANH_LUC_NHAT, Mau.VANG_NHAT, Mau.HONG_NHAT, # Add more colors if needed
        Mau.XANH_NHAT, Mau.TIM_NHAT, Mau.OAI_HUONG, Mau.BAC_HA, Mau.HONG_NHAT, Mau.XANH_LUC_NHAT, Mau.VANG_NHAT
    ]

    for index, line in enumerate(lyrics_list):
        mau_hien_tai = mau_sac[index % len(mau_sac)]
        dong_hien_tai = ""

        try:
            current_speed = char_speed_list[index]
        except IndexError:
            current_speed = 0.05 # Default fast speed

        for i, char in enumerate(line):
            wave = (i / len(line)) * 0.7 + 0.3
            if mau_hien_tai == Mau.XANH_NHAT: r,g,b = int(173*wave), int(216*wave), int(230*wave)
            elif mau_hien_tai == Mau.TIM_NHAT: r,g,b = int(221*wave), int(160*wave), int(221*wave)
            elif mau_hien_tai == Mau.OAI_HUONG: r,g,b = int(230*wave), int(230*wave), int(250*wave)
            elif mau_hien_tai == Mau.BAC_HA: r,g,b = int(152*wave), int(251*wave), int(152*wave)
            elif mau_hien_tai == Mau.HONG_NHAT: r,g,b = int(255*wave), int(182*wave), int(193*wave)
            elif mau_hien_tai == Mau.XANH_LUC_NHAT: r,g,b = int(175*wave), int(238*wave), int(238*wave)
            else: r,g,b = int(255*wave), int(255*wave), int(224*wave)

            dong_hien_tai += f"\033[38;2;{r};{g};{b}m{char}{Mau.RESET}"
            sys.stdout.write(f"\r{dong_hien_tai}")
            sys.stdout.flush()
            time.sleep(current_speed)

        try:
            current_pause = pause_list[index]
        except IndexError:
            current_pause = 0.5 # Default short pause

        time.sleep(current_pause)
        print()

# ------------------- HÀM CHÍNH ĐỂ CHẠY CHƯƠNG TRÌNH -------------------

def chay_loi_bai_hat():
    """
    Hàm chính chứa lời bài hát, timing và điều phối mọi thứ.
    """

    # Lời bài hát bạn cung cấp
    lyrics = [
        "lạc vào khu rừng hoa này",
        "Khu rừng ngập mùi hương người ta",
        "Dành tặng em bài ca",
        "Kỉ niệm tháng năm ta đã qua",
        "Ở bên anh được không?", 
        "Lòng này cứ trống rỗng",
        "Đầu vẫn mãi lưu luyến đến nụ cười tươi",
        "Anh ôm đôi vai này đêm ngày",
        "Không thoát ra đâu",
        "Ôm theo bao giấc mơ trong",
        "Từng tháng năm kia nhiệm màu",
        "Em ơi xin đừng xa chẳng",
        "Đông vẫn luôn lạnh giá",
        "Đừng để lòng tan nát",
        "Đến bao ngày tháng ta không chung lối"
    ]

    # ========== TỐC ĐỘ GÕ CHỮ (NHANH) ==========
    # Số nhỏ = nhanh
    char_speeds = [
        0.05, 0.04, 0.05, 0.04, 0.05, 0.05, 0.04, # Fast intro
        0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05 # Main part
    ]

    # ========== THỜI GIAN NGHỈ SAU MỖI CÂU (NGẮN) ==========
    # Số nhỏ = nghỉ ít
    pauses = [
        0.5, 0.6, 0.5, 0.6, 0.5, 0.6, 0.8, # Short pauses for intro
        0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 1.5 # Short pauses for main part
    ]
    # ===============================================

    # --- Bắt đầu chạy chương trình ---
    xoa_man_hinh()
    print("Bài hát: Lạc Vào Khu Rừng (Version 2)")
    time.sleep(1.5) # Chờ 1.5 giây
    xoa_man_hinh()

    # Gọi hàm để hiển thị lời bài hát
    hieu_ung_chu(lyrics, char_speeds, pauses)

    print("\n--- Kết thúc ---")
    time.sleep(5)

# ------------------- ĐIỂM BẮT ĐẦU CỦA CHƯƠNG TRÌNH -------------------
if __name__ == "__main__":
    chay_loi_bai_hat()
