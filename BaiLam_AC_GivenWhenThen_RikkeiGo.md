

## Phần 1 – Phân tích

Mình đối chiếu từng AC của Tú với 4 quy tắc: Given là trạng thái có sẵn, When là đúng 1 hành động của người dùng, Then là kết quả cụ thể (số, thông báo, trạng thái), và mỗi AC chỉ 1 kịch bản.

| AC | Đúng/Sai | Quy tắc bị vi phạm | Hậu quả khi kiểm thử |
|----|----------|--------------------|----------------------|
| AC1 | **Đúng** | Không vi phạm. | Given có trạng thái rõ (cước 100.000đ, chưa áp mã), When có đúng 1 hành động (áp "GO20"), Then ra số cụ thể 80.000đ (100.000 × 80% = 80.000). Chữ "và" trong Given chỉ nối thêm điều kiện nên hợp lệ. |
| AC2 | **Sai** | (1) Then mơ hồ: "xử lý phù hợp" không phải số, thông báo hay trạng thái cụ thể. (2) When có 2 hành động ("áp mã" **và** "bấm Đặt xe"), dùng "và" sai chỗ, thực chất là 2 kịch bản gộp lại. | Tester không biết kết quả đúng là gì (báo lỗi? chặn đặt xe? vẫn đặt xe?), mỗi người hiểu một kiểu nên không viết được test case có expected result. Khi test fail cũng không biết fail ở bước áp mã hay bước đặt xe. |
| AC3 | **Sai** | Given và When bị đảo: Given chứa **hành động** ("khách bấm Áp dụng") còn When lại chứa **trạng thái** ("mã không tồn tại"). | Tester không biết hành động kích hoạt là gì, không dựng được điều kiện đầu vào đúng (phải chuẩn bị mã không tồn tại trước khi bấm), nên test case không lặp lại được. |
| AC4 | **Đúng** | Không vi phạm. | Given là trạng thái có sẵn (đã áp "GO20"), When có đúng 1 hành động (áp thêm "FREEKM"), Then có thông báo và con số cụ thể. Có 2 chữ "và" nhưng đều nối **thêm kết quả** trong Then (thông báo + cước giữ nguyên 80.000đ) nên vẫn đúng, và 80.000đ khớp với AC1. |

---

## Phần 2 – Sửa lỗi

AC1 và AC4 đúng nên **giữ nguyên, không sửa**.

### AC2 (sửa)

**Given** cước chuyến là 100.000đ và mã "GO20" đã hết hạn,
**When** khách áp mã "GO20",
**Then** hệ thống hiển thị "Mã khuyến mãi đã hết hạn" và cước giữ nguyên 100.000đ.

Lý do sửa:
- Bỏ "bấm Đặt xe" khỏi When để chỉ còn 1 hành động (áp mã). Việc có cho đặt xe tiếp hay không là kịch bản khác, nếu cần thì viết thành AC riêng.
- Thay "xử lý phù hợp" bằng thông báo cụ thể và con số cụ thể (100.000đ) để tester đối chiếu được ngay.
- Lưu ý: nội dung thông báo "Mã khuyến mãi đã hết hạn" là mình đặt theo cùng văn phong với AC3, cần Đức (PO) xác nhận lại wording chính thức.

### AC3 (sửa)

**Given** khách đã nhập mã "ABC123" vào ô mã khuyến mãi và mã "ABC123" không tồn tại trong hệ thống,
**When** khách bấm "Áp dụng",
**Then** hệ thống hiển thị "Mã khuyến mãi không hợp lệ".

Lý do sửa:
- Chuyển "mã không tồn tại" về Given (đây là trạng thái có sẵn).
- Chuyển "bấm Áp dụng" về When (đây là hành động duy nhất của khách).
- Then giữ nguyên vì đã là thông báo cụ thể.

---

## Kiểm tra lại: chuyển được thành test case

| AC | Input / Tiền điều kiện | Hành động | Expected result |
|----|------------------------|-----------|-----------------|
| AC1 | Cước 100.000đ, chưa áp mã | Áp "GO20" | Cước hiển thị 80.000đ |
| AC2 (sửa) | Cước 100.000đ, "GO20" hết hạn | Áp "GO20" | Thông báo "Mã khuyến mãi đã hết hạn", cước giữ 100.000đ |
| AC3 (sửa) | Đã nhập "ABC123" (không tồn tại) | Bấm "Áp dụng" | Thông báo "Mã khuyến mãi không hợp lệ" |
| AC4 | Đã áp "GO20", cước 80.000đ | Áp thêm "FREEKM" | Thông báo "Mỗi chuyến chỉ áp được 01 mã", cước giữ 80.000đ |

Cả 4 AC đều có Given rõ, 1 When, Then kiểm chứng được, không phải đoán kết quả.
