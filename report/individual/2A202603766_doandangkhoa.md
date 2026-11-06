# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202603766
- **Họ và tên:** Đoàn Đăng Khoa
- **SEED** (in ở ô đầu notebook): 4495

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☐ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

## 2. Kết quả chính

Bảng so sánh kết quả các mô hình ở Phần 5:

| Mô hình               | F1 test | F1 SUB 40 | p95 ms/tin | USD/1k tin |
| --------------------- | ------- | --------- | ---------- | ---------- |
| 1. BoW + LogReg       | 0.872   | 0.919     | 0.152      | 0.0        |
| 2. BoW + MLP          | 0.941   | 0.947     | 3.632      | 0.0        |
| 3a. MiniLM + kNN      | 0.903   | 0.947     | 67.330     | 0.0        |
| 3b. MiniLM + LogReg   | 0.948   | 0.974     | 42.043     | 0.0        |
| 3c. MiniLM + MLP head | 0.952   | 0.974     | 50.105     | 0.0        |
| 4a. LLM nhỏ zero-shot | 0.884   | 0.884     | 3443.305   | 0.0        |
| 4b. LLM lớn zero-shot | 0.905   | 0.905     | 2415.324   | 0.0        |
| 4c. LLM nhỏ few-shot  | 0.927   | 0.927     | 2870.702   | 0.0        |

## 3. Dự đoán và thực tế

*Chọn 3 dự đoán `predict(...)` lệch nhiều nhất so với kết quả. Mỗi cái: bạn đoán gì, ra gì, vì sao lệch.*

## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
