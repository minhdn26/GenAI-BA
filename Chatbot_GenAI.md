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

![image](https://github.com/user-attachments/assets/7640997e-edec-4150-9b6c-6db6c2d77686)



## Preparation Phase - Chuẩn bị những gì?

### 1. Data - Dữ liệu sạch là số 1
Mình tin rằng mỗi bên sẽ có thể có một tool chunking có cơ chế hoạt động khác nhau và việc rà soát kĩ càng để đảm bảo tài liệu văn bản của mọi người đúng với template và format phù hợp với tool đó là vô cùng quan trọng.
Ví dụ như bên mình, sau khi document được đẩy lên hệ thống thì bên mình sẽ cắt doc đó ra lần lượt theo 
```
<Title>-<Heading 1>-<Heading n>-<content>
``` 
Do đó, nếu document không được đánh title, thì nội dung doc phía dưới sẽ hoàn toàn bị cắt đi và không được lưu trữ. Hoặc tool bên mình với những heading mà có 1 dòng bị cách thì content sau heading đó cũng bị cắt đi. Mình biết là văn bản nội bộ quy định quy trình thường rất dài, có những văn bản có thể lên tới mấy chục trang. Đau đầu hơn là có những dòng nhìn thì tưởng là Heading, nhưng thực ra là normal text được format cho giống với Heading 🙁nên nhìn chung công việc xử lý dữ liệu đòi hỏi sự kiên nhẫn và tỉ mẩn rất cao. 
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

### Human - Con người 
Ở phần này, mình sẽ chia Con người thành hai team, team IT (nhóm chịu trách nhiệm chính cho việc phát triển prompt, phát triển tính năng, đề xuất giải pháp, tối ưu luồng xử lý dữ liệu,...) và team BU (hay là bộ phận nghiệp vụ, chịu trách nhiệm đưa ra đề bài, là end user hoặc là team gần gũi, thấu hiểu end user nhất). 
Theo mình thì để 1 dự án Gen AI có thể hoàn thành nhanh và hiệu quả thì cần chất lượng và số lượng ở cả hai team.

**Team IT** 

- Số lượng: Theo mình thì 1 core team triển khai 1 use case Gen AI, nếu không phải là build platform, thì sẽ không cần số lượng quá lớn. Core team nên chỉ khoảng từ 3-5 người (full-time), tuỳ vào khối lượng dữ liệu cần xử lý. Một team ít hơn 3 thì khối lượng công việc của người đó sẽ khá nhiều, thường xuyên phải multi-task, cần skill quá đa dạng, dẫn tới khó tuyển dụng. Một team trên 5 thì theo mình sẽ khó quản lý chất lượng đầu ra, dễ phân tán nguồn lực và hơi có sức ì do ỉ lại có nhiều người backup.
- Chất lượng: Các dự án chatbot Gen AI khối lượng công việc lớn thường tới từ việc xử lý dữ liệu, prompting và testing. Các công việc này đều không cần một role quá cụ thể, nhưng mọi người sẽ cần là một team rất active, chủ động và có tinh thần làm việc nhóm tốt để phối hợp và chia việc cho nhau. Sẽ có lúc, khối lượng dữ liệu cần xử lý, review là rất nhiều, nên mọi người sẽ cần phân chia và sync-up với nhau hàng ngày. Team cũng cần có thói quen thường xuyên nghiên cứu, cập nhật về các xu hướng mới về mặt công nghệ để có thể educate team BU và đề xuất giải pháp phù hợp hay tối ưu chatbot sau này.

**Team BU**

- Số lượng: Ngược lại với team IT số lượng không cần nhiều thì team BU sẽ cần dedicate 1 nguồn lực tương đối lớn, tỉ lệ thuận với lượng dữ liệu, văn bản mà BU muốn chatbot có thể trả lời được. BU sẽ là team chịu trách nhiệm chuẩn hoá lại dữ liệu theo format, template mà team IT đã định nghĩa, và đồng thời, cũng là team sẽ phải tổng hợp, thống kê các nghiệp vụ, dữ liệu cần đưa cho chatbot. Sau đó, team cũng cần dồn nguồn lực để UAT với lượng câu hỏi đủ lớn, đa dạng và có tracking lại các issue của chatbot để team IT có thể cải thiện sau đó. Đây đều là những công việc tỉ mẩn, có thể tốn thời gian nên sẽ cần một lực lượng lớn vào một số giai đoạn như Chuẩn bị dữ liệu và Testing.
- Chất lượng: BU là người đưa ra yêu cầu chính cho sản phẩm, do đó một team BU chất lượng cần 2 yếu tố chính, đó là
  1. Thấu hiểu end-user (ở đây BU có thể là end-user hoặc là khách hàng ngoài của ngân hàng)
  2. Hiểu về sự khác biệt giữa Gen AI và các điểm hạn chế của Gen AI
Đối với yếu tố thứ 2, Team IT sẽ là bên educate cho BU. Nếu BU không hiểu về sự khác biệt và các điểm hạn chế, khả năng cao sẽ đưa ra những yêu cầu không phù hợp cũng như những chỉ số đo lường vô lí. Ví dụ, BU bên mình từng có các yêu cầu hay thắc mắc không hợp lý như sau:
```
- Chị cho list keyword rồi bot của em bắt keyword để phản hồi ra các kịch bản tương ứng được không em?
- Sao bot của em mỗi lần lại trả lời 1 kiểu thế? Thế làm sao để chị kiểm soát được độ chính xác của nó được?
- Yêu cầu bot trả lời đủ ra hết thông tin dù câu hỏi chung chung.
...
``` 

### Plan and Process - Phối hợp và Tuân thủ
Để triển khai nhanh thì không chỉ cần con người mà mình thấy quy trình và kế hoạch cũng vô cùng quan trọng. Team của mọi người dù có giỏi nhưng lại lộn xộn, thích trễ deadline thì mình tin là không chỉ triển khai use case Gen AI mà use case nào cũng không ổn. Theo kinh nghiệm cá nhân thì mình thấy các thành phần sau là bắt buộc và vô cùng quan trọng cho dự án ngay từ bước đầu triển khai.

**Kế hoạch Master plan**
Một Master plan tốt sẽ là kim chỉ nam và cũng là quy định yêu cầu các thành viên từ 2 team phải tham gia. Master plan không phải là một plan chung chung, mà phải là một plan vô cùng chi tiết, cụ thể từng phase, task list, PIC, start date, end date, status và note issue. Master plan này sẽ được cả team BU và IT review và đồng thuận, coi như là một bản cam kết sẽ tuân thủ timeline và các scope công việc nêu trong plan. 
Để đảm bảo tuân thủ master plan, hai team cần meeting weekly catch up tiến độ, hay thậm chí catch up với tần suất 2 lần 1 tuần nếu là 1 dự án thực sự gấp hay có độ khó cao, khối lượng công việc nhiều (đây là catch up giữa BU và IT còn nội bộ mỗi team thì chắc chắn nên có daily catch up rùi). 

**Các document hướng dẫn**


**Thống kê dữ liệu**
Dữ liệu là vô cùng quan trọng, là xương sống của chatbot nên chắc chắn mọi người sẽ cần kiểm soát cấu phần này. Và cách tốt nhất hiện tại theo mình là có một sheet Thống kê dữ liệu. Sheet này dù có thể mất thời gian nhưng sẽ giúp mọi người nắm được:

- Bot đã nhận được những tri thức gì hay Scope tri thức mà bot có thể trả lời là gì. Từ đó, phía BU có thể có những kì vọng đúng đẵn khi xây dựng bộ câu hỏi UAT. Ngoài ra thì việc này cũng phục vụ tốt cho quá trình report, marketing về bot sau này, kiểu bot nắm được n văn bản, x nghiệp vụ, y bộ FAQ,... nghe cũng đã và uy tín hen :)
- Nếu Bot trả lời sai thì là do đâu. Bot không thể lúc nào cũng trả lời đúng 100%, và đối với những câu sai, thì nguyên nhân là do đâu? Là do dữ liệu truy vấn ra không đúng, không đủ để trả lời câu hỏi của người dùng, hay là do bản thân dữ liệu đưa cho bot đã sai? Việc trace lại nguyên nhân của câu trả lời sai sẽ nhanh hơn rất nhiều, nếu có bản thống kê dữ liệu này.
- Dữ liệu nào đã và cần được update và thay đổi. 
