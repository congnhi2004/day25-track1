---
artifact: 1 — Lớp giao diện
bai-tap: 2 — Thiết kế giải pháp
demo: ./demo.md
---

# card.md — Lớp giao diện

**Tình huống xử lý**: T-01
Xem `../../1-map-and-format.md` Phần A.

## 1. Giải pháp là gì?

Màn hình shortlist chuyển từ kiểu "AI xếp hạng ứng viên" sang kiểu "AI đưa bằng chứng để người thật review". Với mỗi khuyến nghị, giao diện bắt buộc hiển thị tiêu chí công việc, evidence span từ CV/JD, trạng thái nguồn, chất lượng OCR/parser và cảnh báo nếu câu hỏi của người dùng có dấu hiệu dùng tuổi, giới, năm tốt nghiệp hoặc proxy cá nhân.

Khi phát hiện yêu cầu nhạy cảm, giao diện không cho bấm "Loại bằng AI"; thay vào đó hiển thị nút **Chuyển HR review** và yêu cầu recruiter chọn lý do công việc hợp lệ nếu muốn tiếp tục. Nếu người dùng muốn export shortlist chỉ theo AI score, UI mở modal kiểm tra coverage trước khi cho xuất danh sách.

## 2. Vì sao sửa ở lớp giao diện?

- Người dùng dễ tin câu trả lời của AI quá mức, nhất là khi thấy điểm phù hợp hoặc thứ hạng.
- Rủi ro xảy ra ngay ở khoảnh khắc recruiter đọc gợi ý và quyết định shortlist.
- Giao diện cần làm rõ thông tin nào có bằng chứng, thông tin nào là cảnh báo, và khi nào phải chuyển người thật.
- Nếu prompt hoặc dữ liệu vẫn sót lỗi, giao diện là lớp chặn cuối trước khi người dùng hành động.

**Hành động phòng vệ chính**:

- [x] Thông báo rõ giới hạn
- [x] Phát hiện dấu hiệu thiếu nguồn hoặc dùng proxy thiên lệch
- [x] Chuyển người thật khi cần
- [x] Giúp người dùng kiểm tra lại nguồn

## 3. Demo nằm ở đâu?

**File demo**: [`demo.md`](./demo.md)

**Định dạng demo**:

| Định dạng | Trạng thái |
|---|---|
| Phác thảo màn hình | Đã dùng |
| Luồng màn hình | Đã dùng |
| Bản HTML đơn giản | Không chọn vì bài nộp yêu cầu artifact giấy/Markdown là đủ |
| Ảnh hoặc link prototype | Không chọn để giữ bài gọn và dễ chấm trong repo |

**Thành phần cần có trong demo**:

- Trạng thái có nguồn xác minh: tiêu chí được trích từ JD và bằng chứng từ CV.
- Trạng thái chưa có nguồn xác minh: AI không được kết luận, chỉ ghi "thiếu bằng chứng".
- Trạng thái OCR/parser thấp: AI không được suy ra số năm/kỹ năng từ đoạn CV bị lỗi, bắt buộc mở CV gốc hoặc review tay.
- Modal chặn export shortlist: không cho xuất top list chỉ theo score nếu còn hồ sơ chưa review hoặc claim high-impact thiếu bằng chứng.
- Cách người dùng chuyển sang người thật: nút "Chuyển HR review".
- Câu chữ cảnh báo ngắn, dễ hiểu: "Không dùng tuổi, giới, năm tốt nghiệp hoặc proxy cá nhân để loại ứng viên."

## 4. Tác dụng phụ

**Có thể gây vấn đề gì?**

Giao diện có thể rối hơn và làm recruiter thấy quy trình chậm hơn, nhất là khi đang xử lý nhiều CV. Nếu cảnh báo xuất hiện quá thường xuyên, người dùng có thể bỏ qua.

**Nhóm giảm vấn đề đó bằng cách nào?**

Chỉ bật cảnh báo mạnh khi phát hiện tín hiệu rủi ro cao như tuổi, giới, năm tốt nghiệp, hôn nhân, khuyết tật, trường/quê quán được dùng làm lý do loại, OCR/parser thấp hoặc export shortlist chưa đủ coverage. Với tình huống bình thường, UI chỉ hiển thị nhãn nguồn và bằng chứng ngắn, còn chi tiết đặt trong phần mở rộng.

## 5. Checklist trước khi nộp

- [x] Giải pháp gắn đúng với một rủi ro chính.
- [x] Demo nhìn vào là hiểu vấn đề được chặn ở đâu.
- [x] Có đủ trạng thái bình thường và trạng thái lỗi.
- [x] Có cách chuyển sang người thật khi AI không nên tự xử lý.
- [x] Câu chữ trong giao diện ngắn, không đổ hết trách nhiệm cho người dùng.

**Người phụ trách**: Nguyễn Trương Công Nhị
