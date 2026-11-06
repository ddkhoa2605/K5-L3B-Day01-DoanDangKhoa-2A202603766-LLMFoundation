# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202603766
- **Họ và tên:** Đoàn Đăng Khoa
- **SEED** (in ở ô đầu notebook): 4495

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | x |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | x |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | x |

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

**1. `predict("ml_lr")` — Learning rate = 50**
- **Dự đoán:** Loss sẽ dao động mạnh vì learning rate quá lớn.
- **Thực tế:** Loss tăng vọt ban đầu nhưng nhanh chóng giảm xuống 0.005, không dao động kéo dài.
- **Vì sao lệch:** Dù learning rate lớn, mô hình vẫn hội tụ nhanh trên bộ dữ liệu này.

**2. `predict("dl_curve")` — F1 của MLP**
- **Dự đoán:** F1 của MLP gần tương đương Logistic Regression.
- **Thực tế:** MLP đạt 0.941, cao hơn LogReg (0.872), chênh lệch 0.069.
- **Vì sao lệch:** MLP có khả năng học quan hệ phi tuyến giữa các đặc trưng, giúp cải thiện khả năng nhận diện spam.

**3. `predict("llm_fewshot")` — Few-shot Learning**
- **Dự đoán:** F1 chỉ tăng nhẹ hoặc không đổi.
- **Thực tế:** F1 tăng từ 0.884 (zero-shot) lên 0.927 (few-shot), tăng 0.043.
- **Vì sao lệch:** Bốn ví dụ có nhãn trong prompt giúp LLM nhận biết rõ hơn đặc điểm của spam và ham, cải thiện kết quả nhiều hơn dự kiến trên SUB 40.

## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
