Hi mọi người, nhân dịp mới release phiên bản chatbot Gen AI đầu tiên cho khách hàng ngoài và n chatbot Gen AI nội bộ cho các khối vận hành khác nhau của 1 bank V giấu tên chỉ trong vòng 1,5 tháng, mình có rút ra được một số bài học và kinh nghiệm. Mình mong rằng những kinh nghiệm này của mình sẽ giúp các anh chị em nắm được tổng quan các giai đoạn, và các điểm cần lưu ý trong quá trình triển khai một chatbot Gen AI cho doanh nghiệp của mọi người. 

# Appetizer - Một số giả định
Trước khi vào nội dung chính thì mình muốn làm rõ một số giả định đầu vào cho bài viết này để chúng mình có cùng quan điểm nhé.
## Đầu tiên
mình giả định là công ty của anh chị em đã có một hệ thống đủ một số tính năng cơ bản để tạo ra một chatbot Gen AI và mục tiêu của mọi người là làm sao để triển khai dự án chatbot của mọi người nhanh, và hiệu quả, nên bài viết này sẽ không bàn tới việc hệ thống đó là gì, có tính năng như nào,...
## Thứ 2,
mình giả định là mọi người hiểu chatbot Gen AI có điểm gì khác biệt so với chatbot kịch bản truyền thống, và mọi người hiểu về một số khái niệm xung quanh generative AI nên bài viết này cũng sẽ không giải thích sâu về các khái niệm, thuật ngữ nếu có được nhắc tới. Tuy nhiên, cái mà mọi người muốn là tìm hiểu về suggestion, về best practice thay vì đi sâu về mặt kĩ thuật công nghệ.
## Thứ 3,
chatbot Gen AI ở đây đơn giản là chatbot có thể hiểu và trả lời các câu hỏi liên quan đến nghiệp vụ, quy trình, và thông tin dựa trên cơ sở dữ liệu là các tài liệu và văn bản (nghiệp vụ) của doanh nghiệp đã được cung cấp, nên bài viết này cũng sẽ không cover các chủ đề vĩ mô hơn khi ứng dụng gen ai vào các bài toán “vĩ mô" và phức tạp hơn khác của doanh nghiệp.  

# Main course - Kinh nghiệm cá nhân
Theo quan điểm cá nhân thì mình sẽ chia quá trình phát triển một chatbot Gen AI thành 3 giai đoạn chính. Mỗi giai đoạn lại có những lưu ý riêng. Tổng quan mọi người xem hình dưới đây nhé:

![image](https://github.com/user-attachments/assets/f35f0924-5891-4d7f-bd16-97955332a867)



## Phase Chuẩn bị - Chuẩn bị những gì?

### 1. Dữ liệu
Mình tin rằng mỗi bên sẽ có thể có một tool chunking có cơ chế hoạt động khác nhau và việc rà soát kĩ càng để đảm bảo tài liệu văn bản của mọi người đúng với template và format phù hợp với tool đó là vô cùng quan trọng.
Ví dụ như bên mình, sau khi document được đẩy lên hệ thống thì bên mình sẽ cắt doc đó ra lần lượt theo 
```
<Title>-<Heading 1>-<Heading n>-<content>
``` 
Do đó, nếu document không được đánh title, thì nôị dung doc phía dưới sẽ hoàn toàn bị cắt đi và không được lưu trữ. Hoặc tool bên mình với những heading mà có 1 dòng bị cách thì content sau heading đó cũng bị cắt đi. Mình biết là văn bản nội bộ quy định quy trình thường rất dài, có những văn bản có thể lên tới mấy chục trang. Đau đầu hơn là có những dòng nhìn thì tưởng là Heading, nhưng thực ra là normal text được format cho giống với Heading 🙁nên nhìn chung công việc xử lý dữ liệu đòi hỏi sự kiên nhẫn và tỉ mẩn rất cao. 
Bài học của mình là hãy check kĩ menu document outline bên cạnh xem đã đầy đủ các nội dung Heading như trong văn bản chưa. 
Đối với dữ liệu dạng excel, hãy cực kì cẩn thận với các sheet ẩn. Bộ phận nghiệp vụ trong quá trình xây dữ liệu có thể ẩn sheet này sheet kia và sau đó quên không hiển thị lại. Và những sheet ẩn này hoàn toàn có thể là những sheet sai format, làm ảnh hưởng tới bộ dữ liệu sạch sẽ cho chatbot. 
Mọi người cũng cần loại bỏ các thông tin thừa trong các file dữ liệu trước khi thực hiện lưu trữ. Ví dụ, các thông tin như STT, PIC của câu hỏi,.. như trường hợp bên mình thì hoàn toàn là những nội dung không phục vụ cho việc trả lời câu hỏi của end user mà chỉ để sử dụng để bên nghiệp vụ dễ dàng phân chia nhiệm vụ. Những cột thông tin thừa đó cần được loại bỏ trước khi đưa vào lưu trữ.
Dữ liệu của mọi người cũng nên cần hạn chế viết tắt, đặc biệt là các từ thuật ngữ nên được diễn giải cẩn thận. Đối với dữ liệu dạng QnA, phần Answer nên thật ngắn gọn, súc tích, đi vào trọng tâm các nội dung chính để trả lời được Question tương ứng. Ví dụ: 

```
Question: Làm sao để mở thẻ tín dụng?
Correct Answer: Có x cách để mở thẻ tín dụng. Cách 1:... Cách 2:...
Incorrect Answer: Dạ thưa Anh Chị, Em xin phép được trả lời như sau ạ. Hiện ngân hàng chúng em hỗ trợ hai cách để mở thẻ tín dụng. Cách 1: Anh Chị … Cách 2: Anh Chị …
```
**→ Văn phong của Chatbot hoàn toàn có thể được hướng dẫn bởi system prompt thay vì là để luôn thành câu trả lời trong file QnA. Việc để nhiều từ ngữ, câu văn rườm rà có thể ảnh hưởng tới mức độ chính xác của việc truy xuất ra dữ liệu đúng để chatbot tạo ra câu trả lời.** 
