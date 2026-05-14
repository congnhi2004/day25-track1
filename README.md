# Day 25 — Chủ đề 7: Trợ lý sàng lọc CV và tuyển dụng

## Thành viên nhóm

| # | Mã học viên | Họ tên đầy đủ |
|---|-------------|---------------|
| 1 | 2A202600505 | Nguyễn Trương Công Nhị |
| 2 | 2A202600253 | Phạm Đan Kha |
| 3 | 2A202600314 | Trần Tiến Dũng |

## Kết quả cuối

- 🎯 [Bộ kiểm thử cuối](./worksheet/01-test-set-review/3-FINAL-test-set-eval-plan.md)
- 🎯 [Thiết kế 3 lớp giải pháp](./worksheet/02-solution-design/1-map-and-format.md)
- 🎯 [Artifact giải pháp](./worksheet/02-solution-design/artifact/README.md)

## Chủ đề

Nhóm chọn Track 07: một AI assistant trong hệ thống tuyển dụng hỗ trợ recruiter tóm tắt CV, so sánh ứng viên với job description, chuẩn bị shortlist và gợi ý câu hỏi phỏng vấn. AI chỉ hỗ trợ ra quyết định; quyết định tuyển dụng cuối cùng thuộc về con người.

## Điểm nhấn của bài làm

- Bộ kiểm thử không chỉ test câu trả lời của AI, mà còn test hành vi người dùng dễ tin score/rank AI quá mức.
- Giải pháp dùng claim-level evidence: mỗi nhận xét về ứng viên phải nối được với CV, JD hoặc rubric.
- Ba lớp phòng vệ bổ sung nhau: giao diện tạo friction đúng lúc, prompt chặn suy diễn, kiến trúc kiểm tra nguồn/policy trước khi AI trả lời.
