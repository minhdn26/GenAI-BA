# Mục tiêu
Xây dựng luồng tự động lựa chọn được LLM phù hợp nhất đối với mỗi query, nhằm
- Tối ưu chi phí token khi sử dụng LLMs: Sử dụng các model rẻ hơn đối với các queries đơn giản, đặc biệt là có thể tận dụng các model open source khi các bên cung cấp model ngày càng nhiều.
- Tăng trải nghiệm người dùng: Các model rẻ hơn thường có tốc độ xử lý nhanh hơn, từ đó giảm thời gian nhận được câu trả lời của người dùng đối với các câu hỏi đơn giản.
- Tăng độ chính xác: Mỗi một model sẽ phù hợp với các tác vụ và loại câu hỏi khác nhau. Việc lựa chọn được model phù hợp sẽ tăng mức độ chính xác trong việc đáp ứng các câu trả lời khác nhau của người dùng.
- Giảm phụ thuộc và dễ mở rộng: Bên cung cấp model có thể giới hạn request trong 1 khoảng thời gian, do đó cần có cơ chế phân bổ request tới các model khác nhau để vẫn đảm bảo performance khi số lượng request tăng.
# Phạm vi
## Luồng routing cần đảm bảo:
- Tích hợp được với nhiều mô hình LLM trong hệ thống sẵn có
- Phân loại query đầu vào và lựa chọn model tối ưu nhất về tỉ lệ cost và accuracy.
- Báo cáo tracking chi phí và các LLM đã sử dụng cho từng nhóm user.
- Dễ dàng thay đổi cơ chế lưạ chọn model khi yêu cầu business thay đổi.
## LLMs
Các LLMs sẽ được phân thành hai nhóm, **Strong** và **Weak**, cụ thể:

- **Strong**: Các closed-source model đắt tiền và thông minh nhất hiện nay như: GPT 4o, Claude 3.5 Sonnet, Mistral Large 24.11, Pixtral Large,...
- **Weak**: Các closed-source model nhỏ gọn, hoặc open-source model như: GPT 4o-mini, Claude 3.5 Haiku, Mistral Small 24.09, Pixtral 12B,...

 
