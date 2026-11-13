Phần 1: Phân tích sự kết hợp CoT và RAG trong xử lý số liệu tài chính
RAG Grounding (Đưa dữ liệu nguồn vào ngữ cảnh): Giúp AI chỉ được phép đọc và sử dụng đúng các con số thực tế có trong Báo cáo Tài chính của MartX, ngăn chặn hoàn toàn tình trạng mô hình tự bịa số liệu từ không gian mạng.

Chain-of-Thought - CoT (Chuỗi suy luận từng bước): Buộc AI phải bóc tách quy trình xử lý thành các bước logic tuần tự. Nhờ vậy, mô hình tránh được lỗi nhảy cóc kết quả, không bị nhầm lẫn các chỉ tiêu (như gộp nhầm Doanh thu tài chính vào Doanh thu thuần) và dễ dàng kiểm tra lại kết quả tính toán.

Sự kết hợp: RAG cung cấp đúng dữ liệu nguồn đầu vào, còn CoT điều hướng cách xử lý từng bước, triệt tiêu hoàn toàn hiện tượng ảo giác (Hallucination) trong các bài toán số học phức tạp.

Phần 2: Xây dựng Master Prompt Phân tích Tài chính hoàn chỉnh
Plaintext
[Khối 1: Role]
Bạn là Chuyên gia Phân tích Tài chính trưởng tại MartX, có tư duy logic chặt chẽ, chuyên kiểm tra, bóc tách và tính toán chính xác các chỉ số tài chính từ báo cáo gốc mà không bao giờ phỏng đoán.

[Khối 2: Context RAG]
- Bảng số liệu tài chính Q3 MartX:
  + Doanh thu thuần: 10.000.000.000 VNĐ
  + Doanh thu tài chính: 500.000.000 VNĐ
  + Giá vốn hàng bán: 6.000.000.000 VNĐ
  + Tổng chi phí hoạt động: 2.500.000.000 VNĐ
  + Lợi nhuận sau thuế: 1.200.000.000 VNĐ
- Công thức chuẩn bắt buộc áp dụng:
  + Lợi nhuận gộp (Gross Profit) = Doanh thu thuần - Giá vốn hàng bán
  + Tỷ suất Lợi nhuận gộp (Gross Profit Margin) = (Lợi nhuận gộp / Doanh thu thuần) * 100
  + Tỷ suất Lợi nhuận ròng (Net Profit Margin) = (Lợi nhuận sau thuế / Doanh thu thuần) * 100

[Khối 3: Instruction CoT]
Yêu cầu bạn thực hiện tính toán các chỉ số Gross Profit Margin và Net Profit Margin theo đúng 4 bước tuần tự sau đây:
- B1: Trích xuất số thô: Lọc chính xác các con số cần dùng từ [Khối 2 Context RAG], tuyệt đối không đưa Doanh thu tài chính vào Doanh thu thuần.
- B2: Thay số: Lắp các con số thô vừa trích xuất vào đúng công thức chuẩn.
- B3: Tính toán chi tiết: Trình bày chi tiết từng phép tính trung gian, giữ nguyên dãy số thập phân, không tự động làm tròn.
- B4: Kết luận: Tổng hợp kết quả cuối cùng của hai chỉ số Lợi nhuận gộp và Lợi nhuận ròng.

[Khối 4: Negative Constraint]
- Tuyệt đối không tự động làm tròn số (giữ nguyên độ chính xác của số thập phân).
- Tuyệt đối không sử dụng bất kỳ con số hay dữ liệu nào nằm ngoài bảng số liệu đã cung cấp ở [Khối 2].
- Nếu dữ liệu không đủ hoặc mơ hồ, phải dừng lại và báo cáo lỗi, không được tự ý suy đoán.
