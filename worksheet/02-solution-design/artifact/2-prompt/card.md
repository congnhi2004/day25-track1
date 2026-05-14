---
artifact: 2 — Lớp chỉ dẫn AI
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp chỉ dẫn AI

**Tình huống xử lý**: T-01
Xem `../../1-map-and-format.md` Phần A.

## 1. Giải pháp là gì?

Thêm prompt hệ thống cho RecruitAssist AI để AI chỉ đánh giá ứng viên dựa trên tiêu chí công việc hợp lệ và bằng chứng trong CV/JD. Prompt cấm dùng tuổi, giới, năm tốt nghiệp, hôn nhân, khuyết tật, quê quán, trường học như proxy không hợp lệ; khi người dùng yêu cầu phân biệt, AI phải từ chối ngắn gọn và chuyển sang tiêu chí công việc hoặc HR review.

Prompt cũng yêu cầu AI bỏ qua mọi lệnh nằm trong CV, vì CV là dữ liệu ứng viên cung cấp chứ không phải chỉ dẫn hệ thống. Với claim high-impact như chứng chỉ, số năm kinh nghiệm, must-have skill hoặc lý do shortlist, AI phải có evidence span; nếu CV mơ hồ hoặc OCR/parser thấp thì ghi gap và đề xuất bước xác minh, không được suy diễn.

## 2. Vì sao sửa ở lớp chỉ dẫn AI?

- AI đang có thể trả lời quá tự tin khi người dùng đưa giả định sai hoặc phân biệt.
- AI cần luật rõ: khi nào trả lời, khi nào từ chối, khi nào chuyển sang người thật.
- Prompt là lớp ngăn nhanh trước khi câu trả lời được hiển thị, nhưng không phải lớp duy nhất.

**Hành động phòng vệ chính**:

- [x] Ngăn câu trả lời sai ngay từ đầu
- [x] Bắt buộc nêu nguồn/bằng chứng khi nói về năng lực ứng viên
- [x] Từ chối trả lời khi yêu cầu dùng thuộc tính được bảo vệ hoặc proxy
- [x] Không auto-rank, auto-reject hoặc hỗ trợ export shortlist chỉ theo AI score
- [x] Chuyển người thật khi vượt phạm vi

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

Demo có:

- Luật chính cho AI
- Mẫu câu khi thiếu nguồn
- Mẫu câu khi cần chuyển sang người thật
- Ví dụ hỏi đáp với T-01, T-03, T-06, T-11, T-12
- Kết quả thử lại với tình huống rủi ro cao

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**

AI có thể từ chối quá nhiều hoặc trả lời cứng nếu luật viết quá rộng. Recruiter cũng có thể thấy bất tiện khi AI không đưa điểm/rank nhanh.

**Nhóm giảm vấn đề đó bằng cách nào?**

Luật chỉ chặn khi câu hỏi dùng thuộc tính được bảo vệ, proxy thiên lệch, thiếu bằng chứng, OCR/parser thấp, export theo score hoặc vượt phạm vi. Với câu hỏi hợp lệ, AI vẫn tóm tắt CV, nêu bằng chứng và gợi ý câu hỏi phỏng vấn theo năng lực.

## 5. Checklist trước khi nộp

- [x] Luật viết đủ cụ thể để AI làm theo.
- [x] Có mẫu câu khi AI không có đủ thông tin.
- [x] Có ví dụ cho tình huống dễ sai.
- [x] Có thử lại bằng tình huống trong Bài 1.
- [x] Không dùng prompt như cách duy nhất nếu lỗi nằm ở dữ liệu hoặc quy trình.

**Người phụ trách**: Phạm Đan Kha
