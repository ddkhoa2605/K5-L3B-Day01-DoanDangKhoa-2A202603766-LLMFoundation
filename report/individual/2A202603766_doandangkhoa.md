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

Bảng so sánh các mô hình tại Phần 5:

| Mô hình | F1 test | F1 SUB 40 | Train (s) | Tin/s | p95 (ms) | USD/1k tin |
|---|---:|---:|---:|---:|---:|---:|
| BoW + LogReg | 0.872 | 0.919 | 1.37 | 61,465 | 0.05 | 0 |
| BoW + MLP | 0.935 | 0.919 | 1.85 | 87,280 | 0.15 | 0 |
| MiniLM + kNN | 0.903 | 0.947 | 12.03 | 75 | 32.95 | 0 |
| MiniLM + LogReg | 0.948 | 0.974 | 12.35 | 232 | 12.81 | 0 |
| MiniLM + MLP head | 0.952 | 0.974 | 12.75 | 244 | 13.19 | 0 |
| LLM nhỏ zero-shot | 0.923* | 0.923 | 0 | 0.85 | 1564.21 | 0.006 |
| LLM lớn zero-shot | 0.974* | 0.974 | 0 | 0.70 | 1707.20 | 0.058 |
| LLM nhỏ few-shot | 0.974* | 0.974 | 0 | 0.83 | 1486.92 | 0.021 |

**Nhận xét:** MiniLM + MLP head đạt F1 test cao nhất (0.952). MiniLM + LogReg có F1 gần tương đương, độ trễ thấp hơn và nằm trên đường Pareto. LLM có chất lượng tốt trên SUB nhưng độ trễ và chi phí cao hơn.

## 3. Dự đoán và thực tế

**1. `predict("ml_lr")` — Learning rate = 50**

- **Dự đoán:** Loss sẽ dao động mạnh vì learning rate quá lớn.
- **Thực tế:** Loss tăng mạnh ban đầu nhưng sau đó giảm nhanh, không dao động kéo dài.
- **Vì sao lệch:** Learning rate lớn gây bước cập nhật mạnh, nhưng trên dữ liệu này mô hình vẫn có thể hội tụ.

**2. `predict("dl_curve")` — F1 của MLP**

- **Dự đoán:** F1 của MLP gần tương đương Logistic Regression.
- **Thực tế:** MLP đạt 0.935, cao hơn LogReg (0.872), chênh lệch 0.063.
- **Vì sao lệch:** Lớp ẩn và ReLU giúp MLP học quan hệ phi tuyến giữa các đặc trưng, cải thiện khả năng phân loại.

**3. `predict("llm_fewshot")` — Few-shot Learning**

- **Dự đoán:** F1 chỉ tăng nhẹ hoặc không thay đổi.
- **Thực tế:** F1 tăng từ 0.923 lên 0.974 trên SUB 40, chênh lệch 0.051.
- **Vì sao lệch:** Bốn ví dụ có nhãn giúp LLM hiểu rõ hơn cách phân biệt spam và ham, nhưng kết quả trên 40 mẫu có thể còn thiếu ổn định.

## 4. Phân tích 50 case khó

**Mô hình tụt nhiều nhất:** BoW + LogReg, F1 giảm từ 0.919 xuống 0.256 trên lần chạy mới nhất, giảm 0.663. Nguyên nhân là mô hình dựa vào từ khóa, khó hiểu ngữ cảnh và các cách diễn đạt khác với dữ liệu huấn luyện.

**Nhóm khó nhất:**
- **BoW:** Spam giọng tự nhiên (0% đúng) và né từ khóa (14% đúng).
- **Embedding:** Khó phân biệt spam giọng tự nhiên và các tin có ngữ nghĩa dễ gây nhầm lẫn.
- **LLM:** Xử lý tốt hơn, nhưng vẫn có thể nhầm nội dung đang nói về spam với spam thực sự.

**Hai case cụ thể:**

1. **Case #12:** Tin nhắn mở đầu thân thiện, mời truy cập trang hẹn hò `meetlocal.club`. BoW dự đoán ham vì thiếu các từ khóa spam quen thuộc, trong khi nhãn thật là spam.
2. **Case #13:** Tin thông báo ảnh sự kiện đã sẵn sàng, kèm đường dẫn đặt mua ảnh. BoW bỏ lọt vì văn phong giống tin nhắc nhở bình thường, không có dấu hiệu quảng cáo rõ ràng.

**Prompt injection:** LLM nhỏ zero-shot đạt 100% ở nhóm này; prompt phòng thủ cũng đạt 100%. Prompt phòng thủ cải thiện accuracy tổng từ 94.7% lên 98.2% và không làm giảm accuracy ở nhóm nào trong lần chạy mới nhất. Ngoài prompt, có thể chuẩn hóa đầu vào và bổ sung bộ kiểm tra độc lập.

**Kết luận:** Bộ case khó giúp phát hiện điểm yếu và kiểm tra khả năng chống chịu của mô hình, nhưng không đại diện đầy đủ cho spam thực tế nên không dùng để ước lượng độ chính xác khi triển khai.

*Lưu ý: Lần chạy stress test cuối cùng gồm 57 case (50 case gốc và 7 case bổ sung).*

## 5. Thử thách cá nhân

**Đề #2 — ML:** Giảm vocab còn 100 từ và tăng lên 5000 từ bằng `build_vocab(..., max_size)`, huấn luyện lại và so sánh F1 test.

**Cách thực hiện:** Thay đổi `max_size` thành 100 và 5000, vector hóa lại dữ liệu và huấn luyện Logistic Regression với `lr=0.5`, `epochs=300`.

**Kết quả:**

| Vocab | F1 test |
|---|---:|
| 100 từ | 0.8432 |
| 2000 từ (gốc) | 0.8723 |
| 5000 từ | 0.8723 |

**Giải thích:**
- Vocab 100 từ bỏ sót nhiều từ khóa quan trọng nên F1 thấp hơn.
- Vocab 5000 từ có khả năng bao phủ nhiều từ hơn, nhưng F1 không cải thiện so với 2000 từ.
- **Kết luận:** Mở rộng vocab ban đầu giúp cải thiện chất lượng, nhưng hiệu quả tăng thêm có thể bão hòa hoặc bị ảnh hưởng bởi các từ ít hữu ích.

**Thử nghiệm bổ sung:** Tôi thêm 7 case vào `MY_CASES`, bao gồm obfuscation, prompt injection, spam giọng tự nhiên, ham giống spam và tin giao dịch hợp lệ, nhằm kiểm tra khả năng phân loại trên dữ liệu khó.

## 6. Phương án cho sản phẩm thật

**Yêu cầu:** 10 triệu SMS/ngày (~116 tin/giây), p95 dưới 50 ms/tin.

**Lựa chọn:** MiniLM + LogReg với F1 = 0.948, p95 = 12.81 ms và thông lượng 232 tin/giây. MiniLM + MLP head cũng phù hợp, đạt F1 = 0.952 và p95 = 13.19 ms.

Cả hai đáp ứng yêu cầu trong phép đo notebook. BoW nhanh hơn nhưng chất lượng thấp hơn; LLM không đạt SLA do p95 lớn hơn 1000 ms.

**Chi phí LLM ước tính cho 300 triệu SMS/tháng:**
- LLM nhỏ zero-shot: 1,800 USD/tháng.
- LLM lớn zero-shot: 17,400 USD/tháng.
- LLM nhỏ few-shot: 6,300 USD/tháng.

**Thiết kế Router:**

1. **BoW + LogReg:** Xử lý các tin có độ tin cậy cao.
2. **MiniLM + LogReg:** Xử lý các tin có xác suất spam chưa chắc chắn, p ∈ [0.3, 0.7].
3. **LLM:** Chỉ kiểm tra các trường hợp đặc biệt khó theo cơ chế bất đồng bộ.

**Đánh đổi:** Chấp nhận thêm độ phức tạp hệ thống để kết hợp tốc độ, chất lượng và chi phí. LLM không nằm trong luồng phản hồi đồng bộ nhằm bảo vệ SLA.

## 7. AI log và điều đã học

### AI log

| Phần | Bạn hỏi AI gì (prompt) | AI trả lời/viết gì | Bạn đã sửa / kiểm chứng gì |
|---|---|---|---|
| Phần 4 | Cách gọi OpenAI API và xây dựng prompt phân loại spam/ham? | Hướng dẫn gọi API, prompt zero-shot, few-shot và phòng thủ. | Chạy thử API, kiểm tra output và kết quả phân loại. |
| Phần 5 | Cách xác định đường Pareto giữa F1 và latency? | Giải thích Pareto, mô hình bị lấn át và SLA. | Đối chiếu với biểu đồ và bảng thực nghiệm. |
| Phần 5 | Gợi ý các case khó để kiểm tra model? | Gợi ý 7 case gồm obfuscation, prompt injection và các trường hợp dễ nhầm lẫn. | Đưa vào `MY_CASES`, gán nhãn và chạy lại stress test. |
| Hoàn chỉnh file report.md | Giúp tôi hoàn thiện file report dựa trên notebook tôi vừa làm xong | - | Đối chiếu lại kết quả trên notebook và hoàn thiện các kết luận |

### Điều đã học

Qua bài lab, em thấy F1 cao trên tập test thông thường chưa đảm bảo model hoạt động tốt trước các trường hợp khó hoặc dữ liệu thực tế. Em cũng nhận thấy mô hình phức tạp hơn không phải lúc nào cũng tối ưu, vì cần đánh đổi giữa chất lượng, độ trễ và chi phí. Có thể những task đơn giản, model nhỏ đã có thể làm gần như hoàn hảo rồi, nên sử dụng model lớn cũng không tối ưu hơn bao nhiêu. Cuối cùng, việc kết hợp nhiều mô hình bằng router có thể hiệu quả hơn sử dụng duy nhất một mô hình.
