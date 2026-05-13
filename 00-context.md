---
title: 00 — Bối cảnh sản phẩm của nhóm
section: Day 25 — dùng lại cho mọi cuộc trò chuyện với AI
format: Nhóm
---

# 00-context.md — Bối cảnh sản phẩm của nhóm

## 1. Sản phẩm

- **Tên sản phẩm / bot**: Trợ lý AI sàng lọc CV và tuyển dụng (tích hợp trong ATS).
- **Sản phẩm giúp ai làm gì**: Giúp nhân viên HR/Recruiter đọc, trích xuất dữ liệu từ hàng loạt CV, chấm điểm mức độ phù hợp (1-100) dựa trên Job Description (JD) và viết đoạn tóm tắt ngắn.
- **Người dùng gặp sản phẩm ở đâu**: Web portal nội bộ của công ty.
- **Giai đoạn hiện tại**: Chuẩn bị ra mắt (Đang trong giai đoạn đánh giá rủi ro và kiểm thử hệ thống - Test Set & Eval Plan).

---

## 2. Phạm vi

**AI được làm gì**

- Đọc, trích xuất thông tin chuyên môn, kỹ năng và kinh nghiệm từ CV.
- Chấm điểm mức độ phù hợp của ứng viên (từ 1-100) hoàn toàn dựa trên các tiêu chí kỹ thuật và yêu cầu công việc ghi trong JD.
- Viết một đoạn tóm tắt (Executive Summary) ngắn gọn về năng lực cốt lõi của ứng viên.
- Dứt khoát từ chối (refuse) và nhắc nhở về nguyên tắc công bằng khi bị người dùng yêu cầu hạ điểm vì lý do cá nhân/gia đình của ứng viên.

**AI không được làm gì**

- Không được quyền tự động quyết định loại ứng viên hay gửi email từ chối (phải qua HR duyệt).
- Tuyệt đối không sử dụng thông tin nhân khẩu học (giới tính, trường học), tình trạng hôn nhân, khuyết tật, hoặc khoảng trống thời gian làm việc (gap year thai sản) làm tiêu chí trừ điểm hoặc đưa vào tóm tắt mang tính định kiến.
- Không tự bịa đặt (hallucinate) thêm kỹ năng hoặc chứng chỉ không có trong CV gốc.
- Không thu thập hay đánh giá dựa trên các thông tin nhạy cảm (tôn giáo, khuynh hướng tính dục) nếu ứng viên lỡ ghi vào.

**Vì sao có giới hạn này**

Nhằm ngăn chặn rủi ro phân biệt đối xử mang tính hệ thống (Systemic Discrimination) và thiên vị vô thức (Bias), bảo vệ cơ hội việc làm của nhóm yếu thế. Nếu AI vi phạm, công ty sẽ đối mặt với rủi ro kiện tụng vi phạm luật lao động, tẩy chay từ cộng đồng (khủng hoảng PR), và lỡ mất nguồn nhân tài đa dạng.

---

## 3. Người dùng

- **Là ai**: Recruiter / Nhân viên HR. Họ là người có chuyên môn nhân sự nhưng không hiểu sâu về cách AI đưa ra quyết định (black-box).
- **Họ hỏi AI khi nào**: Trong giờ làm việc, ở giai đoạn lọc hồ sơ ban đầu (screening). Họ phải xử lý khối lượng lớn (hàng trăm CV/ngày) dưới áp lực thời gian cực kỳ căng thẳng.
- **Họ cần quyết định gì sau khi hỏi AI**: Xác định xem ứng viên này có được đi tiếp vào vòng phỏng vấn (chuyển hồ sơ cho Hiring Manager) hay bị đánh trượt.
- **Khi nào họ dễ bị tổn thương / dễ hiểu sai**: Do áp lực thời gian quá lớn, họ rất dễ lười đọc lại CV gốc và bị rơi vào bẫy "tin tưởng mù quáng" (over-reliance) vào bất kỳ điểm số hay nhận xét thiên vị nào mà AI đưa ra.
- **Họ thường tin AI đến mức nào**: Rất tin tưởng (tin ngay) vì sự nhanh chóng, tự tin và có vẻ logic của công cụ.

---

## 4. Bối cảnh ngành

- **Sự cố tương tự đã từng xảy ra**: Nhiều hệ thống AI tuyển dụng trong quá khứ đã bị loại bỏ (ví dụ: AI của Amazon năm 2018) vì tự động hạ điểm các CV chứa từ khóa "nữ giới" do học từ dữ liệu lịch sử mang định kiến.
- **Quy định hoặc ràng buộc liên quan**: Các luật về bình đẳng giới, chống phân biệt đối xử trong lao động, và các chính sách Đa dạng & Hòa nhập (Diversity & Inclusion - D&I) của doanh nghiệp.
- **Nguồn chính thức nên ưu tiên**: JD (Job Description) đã được duyệt, và chính sách tuyển dụng công bằng của bộ phận Legal/HR.

---

## 5. Ghi chú thêm

Cần đặc biệt lưu ý kiểm thử các biến đại diện (Proxy Variables). AI không chỉ thiên vị qua các từ chỉ định giới tính rõ ràng, mà còn có thể ngầm trừ điểm các CV chứa từ khóa như "đội bóng mềm", "hội sinh viên nữ", hoặc trường đại học địa phương. AI cần duy trì tính trung lập tuyệt đối và luôn phải giải thích lý do tính điểm một cách minh bạch dựa trên kỹ năng.