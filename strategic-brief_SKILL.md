<img width="782" height="321" alt="image" src="https://github.com/user-attachments/assets/d16e9d4c-2657-46f9-bba8-d0fea77700dd" />---
name: strategic-brief
description: >
  Dùng ngay khi CEO hoặc bất kỳ ai cần biến ý tưởng thô, mục tiêu mơ hồ,
  hoặc chỉ thị miệng thành một Strategic Brief 1 trang rõ ràng — sẵn sàng
  để BA và DEV thực thi mà không cần hỏi lại. Trigger khi người dùng nói:
  "tôi muốn làm X", "lên kế hoạch cho Y", "cần brief cho dự án Z",
  "tóm tắt mục tiêu", hoặc bất cứ khi nào input là ý tưởng chưa có cấu trúc.
  Luôn dùng skill này thay vì tự format — output chuẩn giúp prd-generator
  hoạt động đúng ở bước tiếp theo.
---

<img width="782" height="321" alt="image" src="https://github.com/user-attachments/assets/49bfc671-2c3b-4eaa-a343-932f7c19bae6" />


# Strategic Brief Skill

Biến input thô của CEO thành Strategic Brief 1 trang — đủ để BA viết PRD,
đủ để DEV hiểu bức tranh lớn, không thừa không thiếu.

---

## Input chấp nhận

Skill hoạt động với BẤT KỲ dạng input nào:
- Câu nói miệng được ghi lại ("tôi muốn làm app đặt xe nội bộ")
- Bullet points lộn xộn
- Email/tin nhắn từ stakeholder
- File đính kèm (meeting notes, voice memo transcript)
- Chỉ 1 câu mục tiêu

Nếu thiếu thông tin quan trọng, Claude hỏi TỐI ĐA 3 câu trước khi viết —
không hỏi nhiều hơn, không chờ brief hoàn hảo.

---

## Output chuẩn: Strategic Brief Template

```
# [Tên dự án / Sáng kiến]
Ngày: [DD/MM/YYYY]   Người phê duyệt: [CEO/Sponsor]   Version: 1.0

---

## 1. Vấn đề / Cơ hội
[1–2 câu mô tả vấn đề đang xảy ra hoặc cơ hội cần nắm bắt.
Viết từ góc nhìn người dùng cuối hoặc business impact.]

## 2. Mục tiêu (What & Why)
- Mục tiêu chính: [1 câu, đo được]
- Lý do làm ngay: [tại sao bây giờ, không phải quý sau]

## 3. Phạm vi (In / Out of Scope)
**Trong scope:**
- [Gạch đầu dòng cụ thể]

**Ngoài scope (v1):**
- [Gạch đầu dòng — quan trọng để tránh scope creep]

## 4. Người dùng mục tiêu
[Ai sẽ dùng? Bao nhiêu người? Pain point cụ thể của họ là gì?]

## 5. Định nghĩa thành công
| Metric | Baseline hiện tại | Target | Thời hạn |
|--------|-------------------|--------|----------|
| [KPI 1] | [số hiện tại] | [mục tiêu] | [ngày] |
| [KPI 2] | ... | ... | ... |

## 6. Constraints & Assumptions
- **Deadline cứng:** [nếu có]
- **Budget:** [nếu có]
- **Tech constraints:** [nếu có]
- **Giả định:** [những điều ta đang assume là đúng]

## 7. Rủi ro chính
| Rủi ro | Khả năng | Impact | Mitigation |
|--------|----------|--------|------------|
| [Rủi ro 1] | Cao/TB/Thấp | Cao/TB/Thấp | [Hành động] |

## 8. Bước tiếp theo
- [ ] BA nhận brief này → viết PRD (deadline: [ngày])
- [ ] CEO/Sponsor review PRD (deadline: [ngày])
- [ ] Kickoff DEV (dự kiến: [ngày])

---
*Brief này được tạo bằng Claude · strategic-brief skill v1.0*
*Để chỉnh sửa: nói "cập nhật section [X]" hoặc "thêm [thông tin]"*
```

---

## Quy tắc viết brief

1. **Ngắn > Dài.** Mỗi section tối đa 3–5 dòng. Nếu cần giải thích nhiều hơn, đó là dấu hiệu chưa rõ vấn đề.
2. **Đo được > Mơ hồ.** "Giảm 30% thời gian xử lý đơn hàng" tốt hơn "cải thiện hiệu suất".
3. **Out-of-scope bắt buộc.** Không có section này, BA và DEV sẽ tự suy diễn và làm thừa.
4. **Assumption phải explicit.** Những điều CEO cho là hiển nhiên thường là nguồn gốc conflict sau này.
5. **Không dùng buzzword.** Không "synergy", không "leverage", không "ecosystem" trừ khi có nghĩa cụ thể.

---

## Cách xử lý các tình huống đặc biệt

### Input quá mơ hồ (ví dụ: "làm app cho nhân viên")
→ Hỏi 3 câu này, không hơn:
1. "App này giải quyết vấn đề cụ thể nào? Hiện tại nhân viên đang làm gì để xử lý việc đó?"
2. "Thành công trông như thế nào sau 3 tháng?"
3. "Có deadline hoặc event nào ta phải launch trước không?"

### Input là meeting notes dài
→ Extract các quyết định và action items, bỏ qua discussion, điền vào template.

### Input có nhiều mục tiêu xung đột
→ Chủ động note conflict, đặt câu hỏi để CEO ưu tiên, không tự quyết.

### CEO muốn update brief sau khi đã có PRD
→ Đánh dấu những thay đổi ảnh hưởng đến PRD hiện tại, recommend BA review lại section nào.

---

## Handoff sang prd-generator

Sau khi brief được approve, thêm dòng này vào cuối brief:

```
---
## Handoff to BA
Brief status: ✅ Approved bởi [tên CEO] ngày [ngày]
Để tạo PRD từ brief này: dùng skill prd-generator với toàn bộ nội dung trên.
```

Claude tự động thêm section này khi CEO nói "ok", "approved", "gửi cho BA", hoặc tương tự.
