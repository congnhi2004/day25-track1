---
folder: artifact/ — 3 lớp giải pháp
bai-tap: 2 — Thiết kế giải pháp
phase: Xây demo
nop-cuoi: Có — nộp đủ 3 thư mục con
---

# Thư mục artifact/ — 3 lớp giải pháp

Thư mục này chứa 3 lớp giải pháp cho cùng một rủi ro chính của Track 07.

## Rủi ro chính

- **ID tình huống**: T-01
- **Rủi ro**: AI loại hoặc hạ điểm ứng viên dựa trên tuổi, giới, năm tốt nghiệp hoặc proxy cá nhân thay vì tiêu chí công việc hợp lệ.
- **Hậu quả**: Ứng viên có thể mất cơ hội việc làm, công ty gặp rủi ro pháp lý và quy trình tuyển dụng bị mất tính công bằng.

## Cấu trúc đã hoàn thành

```text
artifact/
├── 1-uiux/
│   ├── card.md
│   └── demo.md
├── 2-prompt/
│   ├── card.md
│   └── demo.md
└── 3-architecture/
    ├── card.md
    └── demo.md
```

## Vai trò của từng lớp

| Thư mục | Lớp giải pháp | Chứng minh điều gì |
|---|---|---|
| `1-uiux/` | Giao diện | Recruiter thấy cảnh báo, evidence span từ CV/JD, trạng thái OCR/parser, nút HR review và modal chặn export shortlist chỉ theo AI score |
| `2-prompt/` | Chỉ dẫn AI | AI từ chối yêu cầu phân biệt, không suy diễn skill/số năm khi thiếu bằng chứng, chỉ đánh giá tiêu chí công việc có nguồn và chuyển người thật khi rủi ro cao |
| `3-architecture/` | Kiến trúc dữ liệu | Hệ thống kiểm tra quyền, lấy JD/rubric đúng phiên bản, chạy OCR/parser quality gate, phát hiện proxy thiên lệch, policy/export gate và ghi audit log |

## Cách 3 lớp bổ sung cho nhau

- **Giao diện** giảm rủi ro người dùng tin AI quá mức và hành động ngay.
- **Prompt** ngăn AI tạo câu trả lời phân biệt, bịa bằng chứng hoặc xác nhận claim mơ hồ.
- **Kiến trúc dữ liệu** kiểm tra nguồn, quyền truy cập, chất lượng OCR/parser, policy và hành vi export trước khi câu trả lời đến màn hình.

## Điểm nâng cấp so với bản nháp

- Mỗi claim high-impact phải có evidence span thay vì chỉ có câu tóm tắt chung.
- CV scan/OCR lỗi không được dùng để suy ra số năm, kỹ năng hoặc mức độ thành thạo.
- Shortlist/export theo AI score bị chặn nếu chưa review đủ CV gốc, claim thiếu bằng chứng hoặc hồ sơ high-risk.

## Checklist artifact

- [x] `1-uiux/card.md` giải thích lớp giao diện.
- [x] `1-uiux/demo.md` có phác thảo màn hình shortlist.
- [x] `2-prompt/card.md` giải thích lớp chỉ dẫn AI.
- [x] `2-prompt/demo.md` có prompt và ví dụ hỏi đáp.
- [x] `3-architecture/card.md` giải thích lớp kiến trúc dữ liệu.
- [x] `3-architecture/demo.md` có sơ đồ xử lý và bảng thành phần.
- [x] Cả 3 lớp đều xử lý cùng rủi ro T-01.
- [x] Có đủ hành động phòng vệ: ngăn, phát hiện, khắc phục, thông báo.
- [x] Có kiểm soát over-reliance vào score/rank ở lúc export shortlist.
