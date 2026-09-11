# K4 — Ngày 1: Bài Tập & Phản Ánh

## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature

Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)

> Trải qua bốn phản hồi, nhận thấy khi càng tăng temperature thì câu trả lời càng mạch lạc, lời văn càng mượt mà, khi để temperature thấp thì câu trả lời mang tính lý thuyết và trực tiếp hơn không diễn giải dài dòng và thiếu sáng tạo về câu văn 

### Câu 1.2 — Chọn temperature cho sản phẩm

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

> *Nếu là chatbot hỗ trợ khách hàng thì em chọn temperature 0.2-0.5 để đảm bảo tính nhất quán, ngắn gọn và ổn định hơn* 

### Câu 1.3 — Đánh đổi chi phí

Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**

> *GPT-4o đắt hơn GPT-4o-mini sấp sỉ 16.7 lần. Trong trường hợp task cần độ chính xác cao, câu hỏi phức tạp như về luật, y tế, ... thì nên dùng GPT-4o, trong trường hợp task đơn giản, lượng công việc lớn cần nhiều chat thì nên dùng GPT-4o-mini*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona

Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:

- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)

> Khi sử dụng system_prompt đầu tiên câu trả lời ngắn hơn, sử dụng những từ vựng đời thường hơn, giải thích bằng các vidu trong cuộc sống hàng ngày. Khi sử dụng system_prompt thứ hai, sử dụng những từ ngữ chuyên môn hơn, trả lời dài hơn, lấy các ví dụ trong thực tế xuất hiện trong nền kinh tế. 

### Câu 2.2 — tiktoken vs đếm từ

Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**

> *Hai số chênh lệch nhau khoảng 14-15%. Tiếng việt tốn nhiều token hơn vì tiếng việt có dấu và một số từ tiếng việt phải tách thành nhiều token hơn tiếng anh* 

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming

**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)

> *Stream quan trọng nhất trong các ứng dụng chatbot thời gian thực hoặc dịch từ văn bản qua giọng nói. Lúc này người dùng cần nhất là tốc độ kịp với thời gian thực tế, nếu văn bản hiển thị đã lâu mà lúc sau mới đọc thì sẽ tạo trải nghiệm không tốt. Ngược lại nếu trong các task được phép chạy ngầm thì không cần stream, vì lúc này người dùng không quá gấp về mặt thời gian 

### Câu 3.2 — Vì sao backoff theo cấp số nhân?

**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**

> *So với delay cố định luôn chờ một giây, thì exponential backoff giúp tăng thời gian delay sau mỗi lần lỗi, giúp sever có thời gian phục hồi. Nếu hàng nghìn client cùng retry với delay cố định giống nhau thì sau khoảng thời gian delay thì sever lại tiếp tục gặp lỗi, và trong khoảng thời gian delay thì lượng truy cập quá ít tạo ra lãng phí thời gian* 

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona

**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**

> *Persona cho trợ lý : " Hãy trả lời ngắn gọn, đúng trọng tâm, lấy ví dụ thực tế ". Yêu cầu trả lời ngắn gọn vì câu trả lời có thể dài dòng không tập trung nội dung câu hỏi, lãng phí token. Đúng trọng tâm yêu cầu tập trung vào nội dung câu hỏi. Lấy ví dụ thực tế yêu cầu AI lấy các ví dụ đúng thực tế không quá trừu tượng* 

### Câu 4.2 — Hạn chế & cải thiện

**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**

> Hạn chế lớn nhất của trợ lý AI là lịch sử chat quá ngắn, AI quên các yêu cầu hoặc thông tin được cung cấp từ đầu. Đề xuất giải quyết bằng cách kết hợp một mạng LSTM để sàng lọc thông tin quan trọng cần thiết cho đoạn chat và cung cấp thông tin đó cho các câu trả lời tiếp theo. 

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
