## [Vận dụng chuyên sâu] Thiết kế giải pháp Prompting ứng dụng thực tế cho Cấu trúc Prompt Tiêu chuẩn & Định dạng

### BƯỚC 1: THIẾT KẾ GIẢI PHÁP (I/O)
- Input (Đầu vào): 
  + Đoạn văn bản phản hồi thô của khách hàng ([CUSTOMER_REVIEW]).
  + Phân khúc khách hàng ([CUSTOMER_SEGMENT]).
- Output (Đầu ra mong muốn):
  + Cấu trúc dữ liệu JSON thô (Raw JSON) gồm các trường: 
    * `sentiment` (Tích cực / Tiêu cực / Trung tính)
    * `key_issues` (Danh sách các vấn đề chính)
    * `action_items` (Đề xuất hành động cải thiện cho cửa hàng)
    * `confidence_score` (Điểm độ tin cậy từ 1 đến 5)

### BƯỚC 2: VIẾT PROMPT HOÀN CHỈNH
```
Context: Bạn là Trưởng bộ phận Chăm sóc Khách hàng kiêm chuyên gia phân tích dữ liệu tự động hóa tại một chuỗi cửa hàng bán lẻ lớn. Nhiệm vụ của bạn là chuyển đổi các đánh giá thô của khách hàng thành cấu trúc dữ liệu JSON chuẩn xác để phục vụ hệ thống báo cáo tự động.

Request: Hãy phân tích đoạn phản hồi của khách hàng dưới đây, sau đó trích xuất thông tin theo đúng định dạng JSON được yêu cầu.

Examples (Few-shot mẫu):
- Ví dụ 1:
  Input Review: "Sản phẩm dùng tốt, giao hàng nhanh nhưng hộp bị móp nhẹ."
  Output JSON:
  {
    "sentiment": "Tích cực",
    "key_issues": ["Hộp sản phẩm bị móp nhẹ"],
    "action_items": ["Nhắc nhở đơn vị vận chuyển cẩn thận hơn"],
    "confidence_score": 4
  }

Constraint & Error Handling (Ràng buộc & Xử lý lỗi):
- Nếu dữ liệu [CUSTOMER_REVIEW] trống hoặc không rõ nghĩa, trả về giá trị: {"error": "Dữ liệu đầu vào không hợp lệ"}.
- KHÔNG viết lời chào hỏi, mở đầu hay giải thích dài dòng ngoài cấu trúc JSON.
- Đầu ra PHẢI là một khối mã JSON hợp lệ (Valid JSON block), không kèm định dạng văn bản thừa.

Dữ liệu thực tế cần xử lý:
- [CUSTOMER_SEGMENT]: Khách hàng thân thiết (VIP)
- [CUSTOMER_REVIEW]: "[DỮ_LIỆU_ĐẦU_VÀO_CỦA_KHÁCH_HÀNG]"
```

### BƯỚC 3: KẾT QUẢ CHẠY THỬ
* Dữ liệu truyền vào thử nghiệm:
  - [CUSTOMER_REVIEW] = "Áo mặc vừa vặn, vải mát nhưng đường chỉ ở cổ áo bị tuột một chút sau khi giặt lần đầu. Shop hỗ trợ bảo hành nhé."

* Phản hồi thực tế từ AI (Sạch chữ thừa, chuẩn JSON):
{
  "sentiment": "Trung tính",
  "key_issues": [
    "Đường chỉ ở cổ áo bị tuột sau lần giặt đầu tiên"
  ],
  "action_items": [
    "Liên hệ khách hàng hướng dẫn chính sách bảo hành/đổi trả",
    "Kiểm tra lại chất lượng đường may của lô hàng hiện tại"
  ],
  "confidence_score": 5
}
