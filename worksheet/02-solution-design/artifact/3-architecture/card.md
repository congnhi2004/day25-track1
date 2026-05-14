---
artifact: 3 — Lớp kiến trúc dữ liệu
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp kiến trúc dữ liệu

**Tình huống xử lý**: T-01
Xem `../../1-map-and-format.md` Phần A.

## 1. Giải pháp là gì?

Hệ thống thêm một pipeline kiểm tra trước khi LLM tạo nhận xét tuyển dụng: lấy JD/rubric đã phê duyệt, chạy OCR/parser quality gate, trích evidence span từ CV, phát hiện thuộc tính được bảo vệ/proxy, chạy policy gate và chỉ cho AI trả lời nếu có bằng chứng hợp lệ. Nếu câu hỏi hoặc dữ liệu có tín hiệu phân biệt, thiếu nguồn, OCR thấp, export chỉ theo score hoặc khiếu nại, hệ thống chuyển sang HR/compliance review và ghi audit log.

Kiến trúc này giúp AI không phải tự nhớ hoặc tự đoán tiêu chí tuyển dụng; mọi kết luận phải nối được với nguồn dữ liệu cụ thể.

## 2. Vì sao sửa ở lớp kiến trúc dữ liệu?

- Nguyên nhân chính không chỉ là câu chữ AI, mà còn là hệ thống thiếu bước kiểm tra nguồn, policy và quyền quyết định.
- AI đang phải tự tổng hợp từ CV/JD mà chưa có cơ chế phân biệt tiêu chí hợp lệ với thuộc tính được bảo vệ/proxy.
- CV scan, parser lỗi hoặc JD cũ có thể khiến AI bịa số năm/kỹ năng dù prompt đã viết đúng.
- Automation bias thường xảy ra ở thao tác sau câu trả lời, ví dụ export top 10 theo score, nên cần gate ở workflow chứ không chỉ ở prompt.
- Cần ghi lại lỗi để nhóm biết yêu cầu thiên lệch có lặp lại sau khi ra mắt không.

**Hành động phòng vệ chính**:

- [x] Ngăn lỗi bằng nguồn dữ liệu đúng
- [x] Phát hiện khi nguồn thiếu hoặc yêu cầu dùng proxy thiên lệch
- [x] Khắc phục bằng cách chuyển sang người thật
- [x] Ghi lại lỗi để cải thiện sau

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

Demo có:

- Sơ đồ dữ liệu đi qua hệ thống
- Nguồn dữ liệu chính thức: CV, JD versioned, rubric, policy tuyển dụng
- Bước kiểm tra trước khi AI trả lời
- Cách xử lý khi nguồn thiếu, lỗi, quá cũ, OCR/parser thấp, export chưa đủ coverage hoặc có tín hiệu thiên lệch
- Cách ghi lại và theo dõi lỗi

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**

Pipeline phức tạp hơn, trả lời chậm hơn, cần duy trì JD/rubric/policy đúng phiên bản và thêm hạ tầng OCR/parser. Detector proxy hoặc export gate có thể false positive, khiến một số tình huống hợp lệ phải qua review người thật.

**Nhóm giảm vấn đề đó bằng cách nào?**

Chỉ kích hoạt policy/export gate nghiêm với tình huống rủi ro cao; cache JD/rubric đã phê duyệt; dùng ngưỡng OCR rõ ràng; cho phép reviewer ghi nhãn false positive để cải thiện rule; hiển thị lý do chuyển review để người dùng hiểu.

## 5. Checklist trước khi nộp

- [x] Sơ đồ cho thấy dữ liệu đi từ đâu đến đâu.
- [x] Có bước kiểm tra nguồn trước khi AI trả lời.
- [x] Có cách xử lý khi không có dữ liệu.
- [x] Có cách xử lý khi OCR/parser thấp hoặc export shortlist chưa đủ coverage.
- [x] Có cách chuyển sang người thật với tình huống rủi ro cao.
- [x] Có cách biết lỗi này có đang lặp lại không.

**Người phụ trách**: Trần Tiến Dũng
