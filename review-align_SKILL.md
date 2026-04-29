---
name: review-align
description: >
  Dùng ngay khi CEO hoặc BA cần so sánh Strategic Brief với PRD để tìm điểm
  lệch nhau trước buổi review. Trigger khi người dùng nói: "review PRD",
  "check brief vs PRD", "có gì khác không", "PRD đúng chưa", "align brief và PRD",
  "chuẩn bị họp review", hoặc khi được cung cấp cả brief lẫn PRD cùng lúc.
  Output là Gap Report ngắn gọn — chỉ focus vào điểm lệch, không đọc lại toàn bộ.
---

# Review-Align Skill

So sánh Strategic Brief và PRD, tìm gap, output Gap Report 1 trang.
Buổi review tập trung vào quyết định — không mất thời gian đọc lại tài liệu.

---

## Input chấp nhận

**Bắt buộc có đủ 2:**
- Strategic Brief (đã approved)
- PRD (bản draft hoặc đã approved)

Nếu chỉ có 1 trong 2 → yêu cầu cung cấp cái còn lại trước khi chạy skill.

---

## Quy trình so sánh (Claude thực hiện nội bộ)

1. Đọc Brief → extract: mục tiêu, scope, KPI, constraints, deadline
2. Đọc PRD → extract: mục tiêu, scope, stories, AC, NFR, open questions
3. So sánh từng dimension → phân loại gap theo mức độ nghiêm trọng
4. Output Gap Report

---

## Output chuẩn: Gap Report

```markdown
# Gap Report: [Tên dự án]

**So sánh:** Brief v[X] (approved [ngày]) vs PRD v[X] (draft [ngày])
**Tạo bởi:** Claude · review-align skill
**Ngày:** [DD/MM/YYYY]
**Thời gian review dự kiến:** [X] phút

---

## Tóm tắt nhanh

| | Brief | PRD | Trạng thái |
|--|-------|-----|------------|
| Mục tiêu chính | [1 câu] | [1 câu] | ✅ Aligned / ⚠️ Lệch |
| Scope | [X items] | [Y stories] | ✅ / ⚠️ / 🔴 |
| KPI/Metric | [X metrics] | [Y metrics] | ✅ / ⚠️ / 🔴 |
| Deadline | [ngày] | [ngày] | ✅ / 🔴 |
| Người dùng | [X personas] | [Y personas] | ✅ / ⚠️ |

**Kết luận:** [PRD sẵn sàng approve / Cần BA chỉnh X điểm trước khi approve]

---

## 🔴 Critical Gaps — Phải giải quyết trước khi approve

### Gap 1: [Tên gap]
- **Brief nói:** [quote ngắn hoặc paraphrase]
- **PRD nói:** [quote ngắn hoặc paraphrase / "Không đề cập"]
- **Rủi ro nếu bỏ qua:** [impact cụ thể]
- **Hành động:** [CEO quyết định / BA bổ sung / cả hai họp]
- **Owner:** [tên] · **Deadline:** [ngày]

### Gap 2: [Tên gap]
[Lặp lại cấu trúc]

---

## ⚠️ Medium Gaps — Nên giải quyết, không block approve

### Gap 3: [Tên gap]
- **Brief nói:** [...]
- **PRD nói:** [...]
- **Hành động gợi ý:** [...]

---

## ✅ Điểm đã aligned tốt

- [Dimension 1]: Brief và PRD nhất quán
- [Dimension 2]: PRD đã expand đúng hướng từ brief
- [Dimension 3]: ...

---

## Scope Delta

**Có trong PRD nhưng KHÔNG có trong Brief (scope creep?):**
- [Story/feature X] — [BA ghi chú lý do thêm / cần CEO confirm]

**Có trong Brief nhưng KHÔNG có trong PRD (bị bỏ sót?):**
- [Requirement Y] — [cần BA bổ sung story]

---

## Câu hỏi cần quyết định trong buổi review

1. [Câu hỏi cụ thể cần CEO trả lời] → Ảnh hưởng đến: [story X, Y]
2. [Câu hỏi về trade-off] → Nếu chọn A thì..., nếu chọn B thì...
3. [Câu hỏi về priority] → Must-have hay có thể push sang v2?

---

## Kết quả buổi review (BA điền sau)

- [ ] Gap 1: [Quyết định]
- [ ] Gap 2: [Quyết định]
- [ ] PRD version tiếp theo: v[X] — BA update trước [ngày]
- [ ] Sign-off: CEO _____ ngày _____

---
*review-align skill v1.0 · Tạo tự động từ Brief + PRD*
```

---

## Quy tắc phân loại gap

| Level | Định nghĩa | Ví dụ |
|-------|-----------|-------|
| 🔴 Critical | Nếu không giải quyết, DEV sẽ build sai hoặc lãng phí sprint | KPI trong brief khác hoàn toàn với metric trong PRD |
| ⚠️ Medium | Ảnh hưởng đến chất lượng nhưng không block launch | Edge case chưa được cover trong AC |
| ✅ Aligned | Hai tài liệu nhất quán | Persona, scope chính xác |

---

## Cách xử lý tình huống đặc biệt

### Brief và PRD khác nhau hoàn toàn về mục tiêu
→ Dừng lại, không output Gap Report. Thông báo: "Brief và PRD có vẻ mô tả hai sản phẩm khác nhau. Cần BA và CEO clarify trước."

### PRD có nhiều hơn Brief (scope expansion)
→ List tất cả items thêm vào section "Scope Delta", không tự đánh giá là đúng hay sai — để CEO quyết định.

### Brief đã cũ (> 2 tuần so với PRD)
→ Warning ở đầu Gap Report: "Brief này được tạo [X] ngày trước. Confirm rằng brief vẫn còn valid trước khi dùng report này."

---

## Checklist trước khi gửi Gap Report

- [ ] Mọi Critical Gap đều có owner và deadline cụ thể
- [ ] Scope Delta đã được list đầy đủ cả 2 chiều
- [ ] Câu hỏi cho buổi review được viết dưới dạng có thể trả lời Yes/No hoặc chọn option
- [ ] Thời gian review dự kiến được estimate (dựa trên số lượng gap)
