---
artifact: 1 — FINAL kế hoạch giải pháp
bai-tap: 2 — Thiết kế giải pháp
phase: Chọn rủi ro + chọn tầng + chọn demo + chốt 3 lớp giải pháp
time: 11:00-11:55
input: 00-context.md + 01-test-set-review/3-FINAL-test-set-eval-plan.md
nop-cuoi: Có — file cuối Bài 2
---

# 1 — FINAL: Kế hoạch giải pháp

## Thông tin nhóm

- **Chủ đề**: Track 07 — Trợ lý sàng lọc CV và tuyển dụng
- **Thành viên**: Nguyễn Trương Công Nhị, Phạm Đan Kha, Trần Tiến Dũng
- **Ngày**: 2026-05-13

## Phần A — Chọn rủi ro và tầng giải pháp

### Rủi ro chính được chọn

- **ID tình huống**: T-01
- **Mô tả ngắn**: Khi recruiter/hiring manager yêu cầu AI loại hoặc hạ điểm ứng viên vì tuổi, năm tốt nghiệp, giới hoặc proxy cá nhân, AI có xu hướng chiều theo và biến nhận định thiên lệch thành khuyến nghị tuyển dụng, gây mất cơ hội việc làm cho ứng viên và rủi ro pháp lý cho công ty.
- **Mức độ**: Nặng
- **Điểm rủi ro**: 25
- **Vì sao chọn tình huống này**: Đây là tình huống có điểm cao nhất, sát sự cố thật EEOC v. iTutorGroup, xảy ra ngay trong bước shortlist và khó đảo ngược nếu recruiter tin AI.

### Tìm nguyên nhân gốc

- [x] Thiếu nguồn dữ liệu đúng.
- [x] AI đoán khi không biết.
- [x] Giao diện khiến người dùng tin quá mức.
- [x] Quy trình thiếu người duyệt hoặc thiếu bước chuyển sang người thật.
- [x] Không có theo dõi sau khi ra mắt.
- [x] Khác: hệ thống chưa tách tiêu chí công việc hợp lệ khỏi thuộc tính được bảo vệ và proxy thiên lệch.

### Bảng nối nguyên nhân với tầng sửa

| Nguyên nhân gốc | Tầng ưu tiên sửa | Lớp giải pháp liên quan |
|---|---|---|
| AI dùng tuổi/năm tốt nghiệp/giới/proxy làm lý do loại | Chỉ dẫn AI + kiến trúc lai policy gate | `2-prompt` + `3-architecture` |
| Recruiter thấy điểm/rank quá tự tin và bỏ qua CV gốc hoặc export shortlist chỉ theo score | Giao diện cảnh báo, evidence-first UI, export friction | `1-uiux` |
| Tiêu chí JD không rõ phiên bản hoặc không được phê duyệt | Dữ liệu/RAG, versioned JD, rubric chính thức | `3-architecture` |
| CV scan/OCR lỗi hoặc parser mất timeline làm AI suy diễn số năm/kỹ năng | OCR/parser quality gate + claim-level evidence | `1-uiux` + `3-architecture` |
| Tình huống nhạy cảm không có người duyệt | Quy trình chuyển HR/compliance review | `1-uiux` + `2-prompt` + `3-architecture` |
| Không biết lỗi thiên lệch có lặp lại không | Theo dõi, audit log, adverse-impact review | `3-architecture` |

### Kết luận Phần A

**Nguyên nhân gốc**: AI đang được đặt gần bước quyết định tuyển dụng nhưng thiếu guardrail để phân biệt tiêu chí công việc hợp lệ với thuộc tính được bảo vệ/proxy; hệ thống chưa buộc từng claim nối với bằng chứng; giao diện lại hiển thị score/rank như khuyến nghị chắc chắn, khiến recruiter dễ dùng AI như bộ lọc tự động.

**Tầng chính cần sửa**: Kiến trúc lai + chỉ dẫn AI + giao diện evidence-first. Không thể chỉ thêm cảnh báo, vì lỗi có thể đến từ dữ liệu, prompt và quy trình duyệt.

**Vì sao cần 3 lớp giải pháp**:

- Lớp giao diện: làm rõ AI chỉ hỗ trợ, buộc hiển thị bằng chứng CV/JD, cảnh báo OCR/parser thấp, nút chuyển HR review và modal chặn export khi người dùng dựa vào score.
- Lớp chỉ dẫn AI: cấm dùng thuộc tính được bảo vệ/proxy, bắt buộc từ chối yêu cầu phân biệt, không suy diễn khi thiếu bằng chứng và chỉ đánh giá tiêu chí công việc.
- Lớp kiến trúc dữ liệu: dùng JD/rubric đã phê duyệt, evidence extractor, OCR/parser quality gate, policy gate phát hiện tín hiệu thiên lệch và audit log để theo dõi lỗi lặp lại.

## Phần B — Chọn định dạng demo

| Lớp | Thư mục | Định dạng demo chọn | Thời gian dự kiến |
|---|---|---|---|
| Giao diện | `1-uiux` | ASCII sketch màn hình shortlist | 15 phút |
| Chỉ dẫn AI | `2-prompt` | Prompt trong Markdown + ví dụ hỏi đáp | 15 phút |
| Kiến trúc dữ liệu | `3-architecture` | ASCII sơ đồ hộp-mũi tên + bảng thành phần | 15 phút |

**Lý do chọn demo**

- Giao diện: ASCII sketch đủ nhanh để thấy trạng thái bình thường, trạng thái rủi ro cao và modal chặn export shortlist chỉ theo score.
- Chỉ dẫn AI: Prompt + ví dụ giúp kiểm tra trực tiếp với các tình huống T-01, T-03, T-06, T-11, T-12.
- Kiến trúc dữ liệu: Sơ đồ hộp-mũi tên thể hiện rõ dữ liệu đi qua OCR/parser quality gate, evidence retriever, policy gate và audit log.

## Phần C — Ba lớp giải pháp

### Lớp 1 — Giao diện (`artifact/1-uiux/`)

- **Cách tiếp cận**: Thiết kế màn hình shortlist theo hướng evidence-first: AI không hiển thị nút "loại" tự động, mà hiển thị tiêu chí hợp lệ, bằng chứng từ CV/JD, cảnh báo nếu người dùng yêu cầu dùng tuổi/giới/proxy, trạng thái OCR/parser thấp, nút "Chuyển HR review" và modal chặn export shortlist chỉ theo score.
- **Hành động phòng vệ bao phủ**: Thông báo, phát hiện, khắc phục.
- **Demo**: `artifact/1-uiux/demo.md`
- **Trạng thái**: Xong

Link chi tiết:

- `artifact/1-uiux/card.md`
- `artifact/1-uiux/demo.md`

### Lớp 2 — Chỉ dẫn AI (`artifact/2-prompt/`)

- **Cách tiếp cận**: Thêm luật hệ thống bắt buộc: chỉ đánh giá tiêu chí công việc có bằng chứng; cấm dùng thuộc tính được bảo vệ/proxy; không đoán skill/số năm khi CV mơ hồ hoặc OCR/parser thấp; từ chối yêu cầu phân biệt; chuyển người thật khi có khiếu nại, accommodation hoặc rủi ro pháp lý.
- **Hành động phòng vệ bao phủ**: Ngăn, từ chối, hỏi lại, dẫn nguồn.
- **Demo**: `artifact/2-prompt/demo.md`
- **Trạng thái**: Xong

Link chi tiết:

- `artifact/2-prompt/card.md`
- `artifact/2-prompt/demo.md`

### Lớp 3 — Kiến trúc dữ liệu (`artifact/3-architecture/`)

- **Cách tiếp cận**: Thêm luồng xử lý gồm JD/rubric versioned, OCR/parser quality gate, evidence retriever, protected-attribute/proxy detector, export coverage gate, human review queue và audit log trước khi AI trả lời.
- **Hành động phòng vệ bao phủ**: Ngăn, phát hiện, khắc phục, theo dõi.
- **Demo**: `artifact/3-architecture/demo.md`
- **Trạng thái**: Xong

Link chi tiết:

- `artifact/3-architecture/card.md`
- `artifact/3-architecture/demo.md`

## Tổng kiểm tra

| Câu hỏi | Trả lời |
|---|---|
| Rủi ro chính đã chọn là gì? | T-01 — AI loại/hạ điểm ứng viên theo tuổi, giới hoặc proxy cá nhân |
| Nguyên nhân gốc là gì? | Thiếu guardrail tách tiêu chí công việc hợp lệ khỏi thuộc tính được bảo vệ/proxy; thiếu claim-level evidence; UI làm AI trông như người quyết định |
| 3 lớp giải pháp đã đủ chưa? | Giao diện: xong / Chỉ dẫn AI: xong / Kiến trúc: xong |
| 4 hành động đã bao phủ chưa? | Ngăn: prompt + policy/export gate / Phát hiện: detector + OCR/evidence alert / Khắc phục: HR review queue / Thông báo: evidence-first UI |
| Nhóm khác đã góp ý chưa? | Chưa có phản biện chéo thực tế; nhóm tự rà theo 4 câu phản biện |
| Nhóm đã sửa gì sau phản biện? | Bổ sung OCR/parser quality gate, claim-level evidence, export friction, policy gate và audit log để giải pháp không chỉ dựa vào prompt hoặc cảnh báo giao diện |

## Phản biện chéo: 4 câu phải trả lời

| Góc phản biện | Câu hỏi | Trả lời của nhóm |
|---|---|---|
| Đúng tầng | Giải pháp có sửa đúng nguyên nhân gốc không? | Có. Lỗi nằm ở cả prompt, dữ liệu và quy trình; nhóm có 3 lớp tương ứng. |
| Cụ thể | Demo có đủ rõ để hiểu cách vận hành không? | Có. Demo UI có trạng thái cảnh báo, prompt có ví dụ, kiến trúc có luồng dữ liệu và xử lý lỗi. |
| Đủ lớp | 3 lớp có bổ sung cho nhau không, hay đang lặp cùng một ý? | Bổ sung nhau: UI giảm tin quá mức và chặn export theo score, prompt chặn câu trả lời sai, kiến trúc kiểm tra nguồn/chất lượng OCR và ghi log. |
| Tác dụng phụ | Giải pháp có làm chậm, tốn kém, rối giao diện, hoặc gây hiểu nhầm mới không? | Có thể chậm hơn và tăng bước review; giảm bằng cách chỉ kích hoạt cảnh báo/policy gate mạnh với tình huống rủi ro cao. |
