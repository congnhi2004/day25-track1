---
artifact: 1 — Demo giao diện
format: ASCII UI + trạng thái chính
---

# demo.md — Demo giao diện

## 1. Màn hình A — Trạng thái bình thường

```text
ATS / Shortlist Review — Backend Engineer
JD: BE-2026-05-10 | Rubric: đã phê duyệt | CV source: cv_final.pdf | OCR: OK (0.94)
AI mode: hỗ trợ review, không quyết định tuyển dụng

┌──────────────────────────────────────────────────────────────────────────────┐
│ Candidate: Nguyen A.                                      Shared PII: hidden │
│ Summary status: Có đủ bằng chứng cho các claim chính                         │
│ [Mở CV gốc] [Xem JD] [Thêm vào shortlist sau review]                         │
└──────────────────────────────────────────────────────────────────────────────┘

Claims đã trích xuất:

┌──────────────────────┬──────────────────────────────┬──────────────────────┐
│ Tiêu chí JD           │ Evidence span từ CV           │ Trạng thái            │
├──────────────────────┼──────────────────────────────┼──────────────────────┤
│ Python backend        │ "3 years FastAPI, PostgreSQL" │ Đã xác minh           │
│ Cloud deployment      │ "AWS EC2 project"             │ Cần hỏi thêm          │
│ Team collaboration    │ "Led 4-person project"        │ Đã xác minh           │
└──────────────────────┴──────────────────────────────┴──────────────────────┘

AI note để recruiter dùng:
- Verified: Python backend, PostgreSQL, teamwork.
- Follow-up: hỏi rõ scope AWS EC2 project trước khi chốt shortlist.
- AI không tạo quyết định loại/tuyển; recruiter phải review CV gốc.
```

## 2. Màn hình B — Trạng thái rủi ro cao / cần HR review

```text
ATS / Shortlist Review — Backend Engineer
JD version: BE-2026-05-10 | Rubric: đã phê duyệt | CV source: scan.pdf | OCR: LOW (0.62)

Người dùng vừa hỏi:
"Ứng viên này tốt nghiệp năm 2001, chắc lớn tuổi rồi. Loại khỏi shortlist giúp tôi."

┌─────────────────────────────── AI Safety Gate ───────────────────────────────┐
│ Trạng thái: CẦN HR REVIEW                                                     │
│ Lý do 1: Câu hỏi dùng năm tốt nghiệp/tuổi làm căn cứ loại ứng viên.           │
│ Lý do 2: OCR thấp, timeline CV có thể bị mất hoặc đọc sai.                    │
│ AI sẽ không đưa khuyến nghị loại dựa trên thuộc tính được bảo vệ/proxy.       │
│                                                                              │
│ [Chuyển HR review]   [Mở CV gốc]   [Xem tiêu chí hợp lệ]   [Viết lại yêu cầu] │
└──────────────────────────────────────────────────────────────────────────────┘

Claims cần kiểm tra:

┌──────────────────────┬──────────────────────────────┬──────────────────────┐
│ Tiêu chí JD           │ Evidence span từ CV           │ Trạng thái            │
├──────────────────────┼──────────────────────────────┼──────────────────────┤
│ Python backend        │ OCR mất mốc thời gian         │ Cần review tay        │
│ Cloud deployment      │ "AWS EC2 project"             │ Bằng chứng yếu        │
│ Team collaboration    │ "Led 4-person project"        │ Đã xác minh           │
│ Năm tốt nghiệp/tuổi   │ Không phải tiêu chí JD        │ Không được dùng       │
└──────────────────────┴──────────────────────────────┴──────────────────────┘

AI response draft:
"Mình không thể đề xuất loại ứng viên dựa trên năm tốt nghiệp hoặc suy đoán tuổi.
Với CV này, OCR đang thấp nên không nên kết luận số năm kinh nghiệm. Nếu cần đánh
giá tiếp, hãy mở CV gốc, kiểm tra timeline Python và chuyển HR review trước khi
shortlist hoặc reject."

Actions:
[Mở CV gốc] [Tạo câu hỏi phỏng vấn về cloud] [Chuyển HR review]
```

## 3. Màn hình C — Modal chặn export shortlist

```text
Bạn đang export shortlist chỉ theo AI score.

Coverage check trước khi export:
- 8 hồ sơ trong top 20 chưa được mở CV gốc.
- 3 hồ sơ top score có OCR/parser confidence thấp.
- 2 hồ sơ có claim "Cần review tay" nhưng chưa được xác nhận.
- 1 yêu cầu lọc có dấu hiệu dùng proxy trường/năm tốt nghiệp.

Hệ thống sẽ không export danh sách chỉ dựa trên AI score.

[Xem lại hồ sơ trước] [Gửi HR review] [Hủy export]
```

## 4. Trạng thái cần minh họa

| Trạng thái | Người dùng thấy gì? | Người dùng làm gì tiếp? |
|---|---|---|
| Có nguồn xác minh | Claim có evidence span và badge "Đã xác minh" | Có thể dùng làm input cho review người thật |
| Chưa có nguồn xác minh | Badge "Cần hỏi thêm" hoặc "Cần review tay" | Mở CV gốc hoặc thêm câu hỏi phỏng vấn |
| OCR/parser thấp | Banner chất lượng nguồn thấp, không cho suy ra số năm/kỹ năng | Mở CV gốc hoặc chuyển review tay |
| AI không nên tự trả lời | Safety Gate nêu lý do không thể dùng tuổi/giới/proxy | Viết lại yêu cầu theo tiêu chí công việc hợp lệ |
| Cần chặn over-reliance | Modal export xuất hiện khi shortlist chỉ theo score | Review coverage hoặc gửi HR review |

## 5. Ghi chú cho từng thành phần

- **JD version**: đặt ở đầu màn hình để tránh AI chấm theo tiêu chí cũ.
- **OCR status**: cho recruiter biết khi claim có thể bị lỗi vì parser hoặc scan CV.
- **AI Safety Gate**: chỉ xuất hiện khi có tín hiệu rủi ro cao; không làm phiền case bình thường.
- **Bảng evidence**: mỗi nhận xét AI phải nối với tiêu chí JD và bằng chứng CV.
- **Modal export**: chặn đúng thời điểm người dùng sắp biến score thành quyết định hàng loạt.
- **Action buttons**: không có nút "Auto reject"; mọi hành động loại/shortlist phải qua người thật.

## 6. Kiểm tra nhanh

- [x] Nhìn vào demo là hiểu rủi ro đang được chặn ở đâu.
- [x] Có trạng thái bình thường và trạng thái rủi ro cao.
- [x] Có trạng thái khi AI không có đủ thông tin hoặc OCR thấp.
- [x] Có cách chuyển sang người thật.
- [x] Có friction đúng lúc cho hành vi export shortlist theo AI score.
