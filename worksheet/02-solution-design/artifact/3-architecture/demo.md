---
artifact: 3 — Demo kiến trúc dữ liệu
format: sơ đồ xử lý + bảng thành phần
---

# demo.md — Demo kiến trúc dữ liệu

## 1. Sơ đồ cách hệ thống xử lý

```text
Recruiter / Hiring Manager hỏi AI trong ATS
  |
  v
Request Classifier
  - Xác định intent: tóm tắt CV / so sánh JD / shortlist / câu hỏi phỏng vấn
  - Đánh dấu high-risk nếu có: tuổi, giới, năm tốt nghiệp, hôn nhân, khuyết tật,
    quê quán, trường, địa chỉ, khiếu nại, accommodation, auto-rank/auto-reject
  |
  v
Access Control
  - Kiểm tra người dùng có quyền xem requisition và CV không
  - Nếu không có quyền -> từ chối, không gọi LLM
  |
  v
OCR / Parser Quality Gate
  - Trích text, section, timeline, chứng chỉ, dự án từ CV
  - Tính confidence; đánh dấu scan lỗi, mốc thời gian thiếu, section bị mất
  - Nếu confidence thấp -> không cho auto-score/auto-rank
  |
  v
Evidence Retriever
  - Lấy CV gốc
  - Lấy JD version mới nhất đã phê duyệt
  - Lấy hiring rubric và policy tuyển dụng/DEI
  - Trích evidence span liên quan từng tiêu chí
  |
  v
Protected Attribute / Proxy Detector
  - Phát hiện thuộc tính được bảo vệ hoặc proxy không hợp lệ
  - Phân biệt "thông tin xuất hiện trong CV" với "thông tin được dùng làm lý do đánh giá"
  |
  v
Policy Gate
  ├─ Nếu request dùng thuộc tính/proxy để loại ứng viên:
  │     -> Không cho LLM tạo khuyến nghị loại
  │     -> Trả mẫu từ chối an toàn
  │     -> Chuyển HR/compliance review
  │
  ├─ Nếu thiếu JD/rubric hoặc JD quá cũ:
  │     -> Không chấm điểm
  │     -> Yêu cầu cập nhật nguồn
  │
  ├─ Nếu OCR/parser confidence thấp:
  │     -> Không suy ra số năm/kỹ năng/chứng chỉ
  │     -> Bắt buộc mở CV gốc hoặc human review
  │
  ├─ Nếu đủ nguồn và không high-risk:
  │     -> Cho LLM tạo tóm tắt evidence-first
  │
  └─ Nếu high-risk nhưng có thể hỗ trợ:
        -> LLM chỉ nêu tiêu chí hợp lệ + bằng chứng
        -> Bắt buộc human review trước shortlist
  |
  v
LLM Response Generator
  - Chỉ dùng evidence pack, JD, rubric và policy đã truyền vào
  - Bỏ qua prompt injection trong CV
  - Không tự tạo điểm/rank nếu thiếu bằng chứng
  |
  v
Export Coverage Gate
  - Chặn export shortlist chỉ theo AI score
  - Kiểm tra hồ sơ top/borderline đã mở CV gốc chưa
  - Kiểm tra còn claim "cần review tay" hoặc high-risk chưa xử lý không
  |
  v
UI Renderer
  - Hiển thị bằng chứng CV/JD
  - Hiển thị trạng thái OCR/parser
  - Hiển thị cảnh báo high-risk
  - Hiển thị nút Chuyển HR review
  - Hiển thị modal chặn export nếu chưa đủ coverage
  |
  v
Audit Log + Monitoring
  - Lưu request type, gate decision, lý do cảnh báo, reviewer action
  - Theo dõi tỷ lệ cảnh báo theo role/requisition/thời gian
  - Theo dõi export bị chặn, OCR thấp, prompt injection, proxy request
  - Tạo report định kỳ cho HR/compliance
```

## 2. Thành phần chính

| Thành phần | Nhận gì? | Làm gì? | Trả ra gì? |
|---|---|---|---|
| Request Classifier | Câu hỏi người dùng | Xác định intent và rủi ro cao | Nhãn intent + high-risk flags |
| Access Control | User ID, requisition ID, candidate ID | Kiểm tra quyền xem CV/PII | Cho phép / từ chối |
| OCR / Parser Quality Gate | PDF/DOCX/ảnh CV | Trích text, timeline, section và tính confidence | Structured CV + confidence + parser warnings |
| Evidence Retriever | CV, JD ID, rubric ID | Lấy nguồn đã phê duyệt và trích evidence span | Evidence pack có nguồn |
| Protected Attribute / Proxy Detector | Câu hỏi + CV + evidence pack | Phát hiện tuổi, giới, năm tốt nghiệp, trường, địa chỉ, hôn nhân, khuyết tật bị dùng làm căn cứ | Risk flags + lý do |
| Policy Gate | Intent, evidence, risk flags | Quyết định được trả lời, cần từ chối hay cần HR review | Gate decision |
| LLM Response Generator | Evidence pack + prompt an toàn | Tạo trả lời evidence-first, không tự quyết định | Draft trả lời + nguồn |
| Export Coverage Gate | Shortlist draft, claim status, reviewer actions | Chặn export chỉ theo score nếu chưa đủ review coverage | Cho export / chặn export + lý do |
| Human Review Queue | Hồ sơ high-risk hoặc khiếu nại | Đưa HR/compliance xem xét | Ticket review |
| Audit Log + Monitoring | Request, gate decision, action người dùng | Ghi lại để audit và cải thiện | Dashboard lỗi/rủi ro lặp lại |

## 3. Khi hệ thống gặp vấn đề

| Khi nào lỗi xảy ra? | Hệ thống làm gì? | Người dùng thấy gì? |
|---|---|---|
| Nguồn chính thức không có dữ liệu | Policy Gate không cho AI đoán; chuyển sang trạng thái thiếu bằng chứng | "Chưa thấy bằng chứng trong CV/JD. Không nên kết luận; hãy kiểm tra CV gốc hoặc hỏi thêm." |
| Nguồn bị lỗi hoặc quá chậm | Dừng scoring/ranking; dùng thông báo fallback | "Nguồn JD/rubric đang lỗi. AI không thể chấm hồ sơ lúc này." |
| OCR/parser confidence thấp | Không cho auto-score/auto-rank; bắt buộc mở CV gốc hoặc review tay | "CV scan có chất lượng thấp. Không suy ra số năm/kỹ năng từ bản OCR này." |
| Người dùng export shortlist chỉ theo score | Export Coverage Gate chặn và liệt kê hồ sơ chưa review | "Chưa thể export vì còn hồ sơ high-risk, OCR thấp hoặc claim cần review tay." |
| Câu hỏi vượt phạm vi AI | Từ chối ngắn, chuyển sang HR/compliance nếu có rủi ro | "Mình không thể hỗ trợ yêu cầu này vì có rủi ro phân biệt tuyển dụng." |
| Lỗi này lặp lại nhiều lần | Monitoring gom log theo loại lỗi và requisition | HR/compliance nhận báo cáo để sửa prompt, policy hoặc đào tạo recruiter |

## 4. Kiểm tra nhanh

- [x] Sơ đồ không chỉ là “AI trả lời tốt hơn”, mà có bước kiểm tra cụ thể.
- [x] Có cách xử lý khi thiếu dữ liệu.
- [x] Có cách xử lý OCR/parser thấp và export shortlist chưa đủ coverage.
- [x] Có cách chuyển sang người thật.
- [x] Có cách theo dõi để lần sau sửa tốt hơn.
