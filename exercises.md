# K3 — Ngày 1: Bài Tập & Phản Ánh

## Khám Phá LLM API | Phiếu Thực Hành
**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng.

---

# Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?**

> Khi temperature tăng, phản hồi trở nên đa dạng và sáng tạo hơn, ít lặp lại giữa các lần gọi. Ngược lại, temperature thấp tạo ra câu trả lời ổn định, nhất quán và ít thay đổi. Điều này cho thấy temperature kiểm soát mức độ ngẫu nhiên trong quá trình sinh văn bản của mô hình.

---

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

> Tôi sẽ chọn khoảng **0.2–0.4** cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời ổn định, chính xác và nhất quán, đồng thời vẫn giữ được cách diễn đạt tự nhiên. Đối với dịch vụ khách hàng, độ tin cậy quan trọng hơn khả năng sáng tạo.

---

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần, mỗi lần trung bình khoảng 350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini.**

> Với 10.000 người dùng × 3 lần/ngày × 350 token đầu ra, hệ thống sẽ tạo khoảng **10,5 triệu output token mỗi ngày**. Theo bảng giá của OpenAI, GPT-4o có chi phí cao hơn GPT-4o-mini khoảng **5–10 lần** (tùy thời điểm và bảng giá sử dụng). GPT-4o phù hợp khi cần suy luận phức tạp, phân tích chuyên sâu hoặc xử lý các tình huống quan trọng. GPT-4o-mini phù hợp cho chatbot hỗ trợ thông thường, tóm tắt văn bản hoặc các hệ thống có lưu lượng truy cập lớn cần tối ưu chi phí.

---

# Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi **"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau.

**Hai phản hồi khác nhau như thế nào? System prompt ảnh hưởng đến hành vi model ra sao?**

> Hai phản hồi khác nhau rõ rệt về cách diễn đạt, độ dài và mức độ chuyên môn. Với persona giáo viên tiểu học, câu trả lời ngắn gọn, sử dụng từ ngữ đơn giản và ví dụ gần gũi với trẻ em. Với persona chuyên gia tài chính, câu trả lời dài hơn, sử dụng các thuật ngữ như blockchain, hash, distributed ledger, consensus và giải thích sâu về mặt kỹ thuật. Điều này cho thấy system prompt quyết định vai trò mà mô hình đảm nhận, từ đó ảnh hưởng trực tiếp đến phong cách, nội dung và mức độ chi tiết của câu trả lời.

---

### Câu 2.2 — tiktoken vs đếm từ
**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài?**

> Với văn bản tiếng Việt, số token thực tế đo bằng **tiktoken** thường cao hơn ước lượng theo công thức **số từ / 0.75**, chênh khoảng **20–40%** tùy nội dung. Nguyên nhân là tokenizer của các mô hình LLM hoạt động theo **subword (đoạn từ)** chứ không phải theo từng từ hoàn chỉnh. Tiếng Việt có dấu, nhiều âm tiết và các chuỗi ký tự ít phổ biến hơn tiếng Anh nên một từ có thể bị tách thành nhiều token hơn, dẫn đến tổng số token lớn hơn.

---

# Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?**

> Streaming đặc biệt quan trọng trong các ứng dụng hội thoại thời gian thực như chatbot, trợ lý AI hoặc hỗ trợ khách hàng vì người dùng có thể nhìn thấy câu trả lời xuất hiện ngay khi mô hình đang sinh nội dung, giúp giảm cảm giác phải chờ đợi. Ngược lại, non-streaming phù hợp với các tác vụ cần kết quả hoàn chỉnh trước khi hiển thị, chẳng hạn như tạo báo cáo, tóm tắt tài liệu dài hoặc xử lý hàng loạt, nơi người dùng chỉ cần nhận kết quả cuối cùng.

---

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định, exponential backoff có lợi thế gì khi API bị quá tải?**

> Exponential backoff giúp tăng thời gian chờ sau mỗi lần request thất bại, từ đó giảm số lượng yêu cầu đồng thời gửi đến máy chủ và tạo điều kiện để hệ thống phục hồi. Nếu hàng nghìn client cùng retry với cùng một khoảng thời gian cố định, các request sẽ đồng loạt được gửi lại, gây ra hiện tượng **retry storm**, khiến API tiếp tục quá tải. Kết hợp exponential backoff với một khoảng ngẫu nhiên (jitter) sẽ giúp phân tán thời điểm retry và tăng khả năng thành công.

---

# Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình?**

> Tôi chọn persona là **"trợ lý học tập thân thiện dành cho sinh viên"**.
**System prompt:**

> *"Bạn là trợ lý học tập dành cho sinh viên. Hãy trả lời bằng tiếng Việt, ngắn gọn, rõ ràng, ưu tiên giải thích ý chính trước, sau đó đưa ví dụ đơn giản nếu cần. Nếu câu hỏi thiếu thông tin, hãy yêu cầu người dùng bổ sung thay vì tự suy đoán."*
**Giải thích:**

> Cụm từ **"ngắn gọn, rõ ràng"** giúp câu trả lời dễ đọc và tập trung vào nội dung quan trọng. Yêu cầu **"trả lời bằng tiếng Việt"** đảm bảo tính nhất quán về ngôn ngữ, còn yêu cầu **"không tự suy đoán"** giúp giảm nguy cơ mô hình tạo ra thông tin không chính xác.

---

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì? Đề xuất một cải thiện cụ thể và mô tả ngắn cách triển khai.**

> Hạn chế lớn nhất là trợ lý chưa có bộ nhớ dài hạn nên chỉ dựa vào một số lượt hội thoại gần nhất và dễ mất ngữ cảnh của các cuộc trò chuyện trước. Một cải thiện phù hợp là xây dựng cơ chế lưu trữ tóm tắt hội thoại (conversation summary) cùng các thông tin quan trọng vào cơ sở dữ liệu. Khi người dùng tiếp tục cuộc trò chuyện, hệ thống sẽ nạp lại phần tóm tắt này vào system prompt hoặc context để mô hình duy trì được ngữ cảnh mà không cần gửi toàn bộ lịch sử hội thoại.

---

# Danh Sách Kiểm Tra Nộp Bài

- `python grade.py` — xem điểm tự động (mục tiêu ≥ 75/100)
- Cả 4 checkpoint pytest đều pass
- Tất cả 9 câu trong file này đã được trả lời
- Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn trong README
