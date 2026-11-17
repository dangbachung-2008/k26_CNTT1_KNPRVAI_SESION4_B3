# [Vận dụng chuyên sâu] Thiết kế giải pháp Prompting ứng dụng thực tế cho Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)
### **Bước 1: Thiết kế giải pháp (I/O)**

-   **Input (Đầu vào):**
    
      Dữ liệu phản hồi/đơn hàng thô từ khách hàng qua kênh chat (có thể chứa thông tin trống, thiếu dữ liệu hoặc lỗi định dạng).
        
     Yêu cầu nghiệp vụ: Phân loại ý định, trích xuất thông tin đơn hàng và tính toán tổng tiền tự động.
        
-   **Output (Đầu ra mong muốn):**
    
     Báo cáo dạng cấu trúc chuẩn (Markdown / Bảng) gồm: Trạng thái xử lý (Thành công / Cảnh báo lỗi), thông tin đã trích xuất, và các bước logic giải quyết vấn đề.
     ### **Bước 2: Viết prompt và chạy thử**

#### **1. Prompt thực thi (Áp dụng Chain-of-Thought & Xử lý lỗi):**

> "Bạn là một trợ lý AI tự động hóa quy trình vận hành cho cửa hàng bán lẻ.
> 
> **Nhiệm vụ:** Hãy xử lý dữ liệu đơn hàng đầu vào từ khách hàng theo từng bước logic sau:
> 
> 1.  **Kiểm tra dữ liệu (Validation):** Kiểm tra xem khách hàng đã cung cấp đủ 3 thông tin cốt lõi chưa: `[TÊN_KHÁCH_HÀNG]`, `[SỐ_ĐIỆN_THOẠI]`, `[SẢN_PHẨM_MUA]`. Nếu thiếu bất kỳ trường nào, chuyển sang Bước 3 (Xử lý lỗi).
>     
> 2.  **Tính toán logic (Chain-of-Thought):** Nếu đủ dữ liệu, hãy suy luận từng bước: Trích xuất tên sản phẩm →\rightarrow→ Xác định đơn giá →\rightarrow→ Tính tổng tiền thanh toán (áp dụng mã giảm giá nếu có).
>     
> 3.  **Xử lý lỗi (Exception Handling):** Nếu phát hiện dữ liệu bị trống hoặc sai lệch, hãy dừng lại, xuất ra cảnh báo dạng `[LỖI: THIẾU DỮ LIỆU]` và soạn sẵn một câu phản hồi lịch sự yêu cầu khách bổ sung.
>     
> 
> **Định dạng đầu ra:** Trình bày rõ ràng bằng Markdown kèm bảng tóm tắt chi tiết."

#### **2. Kết quả chạy thử thực tế với AI:**

-   **Dữ liệu giả định đầu vào:** Khách hàng nhắn tin:  _"Cho anh đặt 2 áo khoác Bomber màu đen, sđt của anh là 0987654321"_ (Quên chưa cung cấp Tên khách hàng).
    
-   **Phản hồi thực tế từ AI:**
    
    > **[LỖI: THIẾU DỮ LIỆU]**
    > 
    > -   **Chi tiết kiểm tra:** Hệ thống ghi nhận thiếu trường thông tin quan trọng `[TÊN_KHÁCH_HÀNG]` trong tin nhắn của người mua.
    >     
    > -   **Tiến trình xử lý:** Dừng quy trình tính toán tự động để tránh sai sót đơn hàng.
    >     
    > -   **Tin nhắn phản hồi đề xuất gửi cho khách hàng:**
    >     
    >      _"Dạ chào anh/chị, cửa hàng đã ghi nhận đơn đặt 2 áo khoác Bomber màu đen và số điện thoại 0987654321 của mình ạ. Anh/chị vui lòng cho cửa hàng xin thêm **Họ tên đầy đủ** để hoàn tất việc tạo đơn nhé ạ. Em cảm ơn anh/chị!"_
    >     
    


