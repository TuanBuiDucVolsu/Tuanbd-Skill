---
name: prd-generator
description: >
  Dùng ngay khi BA cần viết PRD (Product Requirements Document) từ Strategic Brief
  của CEO, từ yêu cầu miệng, hoặc từ bất kỳ input mô tả tính năng nào.
  Trigger khi người dùng nói: "viết PRD", "tạo tài liệu yêu cầu", "breakdown tính năng",
  "viết user stories", "tạo acceptance criteria", "BA cần document này", hoặc khi
  nhận được một Strategic Brief (đặc biệt có section "Handoff to BA").
  Output chuẩn để DEV có thể bắt đầu estimate và build ngay, không cần họp thêm.
  Luôn dùng skill này thay vì tự format PRD — đảm bảo consistency toàn team.
---

# PRD Generator Skill

Biến Strategic Brief (hoặc yêu cầu thô) thành PRD đầy đủ — đủ để DEV estimate,
đủ để QA viết test case, đủ để CEO review trong 10 phút.

---

## Input chấp nhận

**Tốt nhất:** Strategic Brief đã được approve (output từ `strategic-brief` skill)

**Cũng chấp nhận:**
- Yêu cầu miệng từ CEO/stakeholder
- Email mô tả tính năng
- Ticket Jira/Trello còn sơ sài
- Meeting notes

Nếu không có Strategic Brief, BA phải confirm 3 điều trước khi viết:
1. Mục tiêu business (tại sao làm?)
2. Người dùng (ai dùng?)
3. Định nghĩa done (thế nào là xong?)

---

## Output chuẩn: PRD Template

```markdown
# PRD: [Tên tính năng / Dự án]

**Version:** 1.0
**Ngày tạo:** [DD/MM/YYYY]
**BA phụ trách:** [Tên]
**Nguồn:** Strategic Brief v[X] — approved [ngày]
**Status:** Draft / In Review / Approved

---

## 1. Tổng quan

### 1.1 Bối cảnh
[2–3 câu: vấn đề đang xảy ra, tại sao cần giải quyết ngay.
Copy từ brief nhưng viết lại dưới góc nhìn product, không phải CEO.]

### 1.2 Mục tiêu sản phẩm
[Đúng 1 câu, đo được, gắn với KPI trong brief]

### 1.3 Người dùng mục tiêu (Personas)

| Persona | Mô tả | Pain point chính | Frequency dùng |
|---------|-------|-----------------|----------------|
| [Tên persona 1] | [Vai trò, ngữ cảnh] | [Vấn đề cụ thể] | [Hàng ngày/tuần] |
| [Tên persona 2] | ... | ... | ... |

---

## 2. Phạm vi

### 2.1 Trong scope (v[X])
- [Feature 1]
- [Feature 2]
- [Feature 3]

### 2.2 Ngoài scope (explicitly excluded)
- [Item A — lý do exclude]
- [Item B — dự kiến v2]

### 2.3 Dependencies
- [System/team/data mà feature này phụ thuộc vào]

---

## 3. User Stories & Acceptance Criteria

<!-- Mỗi story theo format: As a [persona], I want [action] so that [benefit] -->
<!-- Mỗi AC theo format: Given [context] / When [action] / Then [outcome] -->

### Epic 1: [Tên epic]

#### Story 1.1: [Tên story ngắn gọn]
**As a** [persona]
**I want to** [hành động cụ thể]
**So that** [lợi ích đo được]

**Acceptance Criteria:**
- [ ] **AC1:** Given [điều kiện ban đầu], when [hành động], then [kết quả mong đợi]
- [ ] **AC2:** Given [...], when [...], then [...]
- [ ] **AC3 (Edge case):** Given [...], when [...], then [...]

**Priority:** Must-have / Should-have / Nice-to-have
**Story Points:** [để DEV estimate]
**Notes:** [ràng buộc kỹ thuật, UI reference nếu có]

---

#### Story 1.2: [Tên story]
[Lặp lại cấu trúc trên]

---

### Epic 2: [Tên epic]
[Lặp lại]

---

## 4. Non-Functional Requirements

| Category | Requirement | Metric |
|----------|-------------|--------|
| Performance | [Ví dụ: Load time trang chính] | < 2s ở P95 |
| Security | [Ví dụ: Auth method] | OAuth 2.0 / JWT |
| Scalability | [Ví dụ: Concurrent users] | 1,000 users đồng thời |
| Availability | [Ví dụ: Uptime] | 99.5% |
| Data retention | [Ví dụ: Log giữ bao lâu] | 90 ngày |

---

## 5. UI/UX Requirements

### 5.1 User flows chính
[Mô tả bằng text hoặc link Figma — không vẽ wireframe trong PRD]

**Flow A: [Tên flow]**
1. User vào [điểm bắt đầu]
2. User thấy [màn hình/state]
3. User thực hiện [hành động]
4. System phản hồi [kết quả]
5. [Tiếp tục...]

### 5.2 Error states
| Scenario | Message hiển thị | Hành động gợi ý |
|----------|-----------------|-----------------|
| [Lỗi 1] | "[Copy chính xác]" | [CTA] |
| [Lỗi 2] | "[Copy chính xác]" | [CTA] |

### 5.3 Empty states
[Mô tả UI khi không có data — thường bị bỏ quên và DEV tự xử lý]

---

## 6. Analytics & Tracking

| Event | Trigger | Properties cần log |
|-------|---------|-------------------|
| [event_name] | [Khi nào fire] | [user_id, timestamp, ...] |

---

## 7. Open Questions

| # | Câu hỏi | Owner | Deadline | Status |
|---|---------|-------|----------|--------|
| 1 | [Quyết định còn pending] | [CEO/Tech lead/BA] | [ngày] | Open |
| 2 | [...] | [...] | [...] | Resolved: [...] |

---

## 8. Revision History

| Version | Ngày | Người thay đổi | Thay đổi gì |
|---------|------|----------------|-------------|
| 1.0 | [ngày] | [BA] | Initial draft |

---

## 9. Sign-off

| Vai trò | Tên | Ngày approve | Ghi chú |
|---------|-----|-------------|---------|
| CEO/Product Owner | | | |
| Tech Lead/DEV | | | |
| BA | | | |

---
*PRD này được tạo bằng Claude · prd-generator skill v1.0*
*Source brief: [link hoặc tên file brief]*
```

---

## Quy tắc viết PRD

### Về User Stories
1. **1 story = 1 hành động của user**, không phải 1 tính năng kỹ thuật.
   - ❌ "System sẽ có API lấy danh sách đơn hàng"
   - ✅ "As a manager, I want to see all pending orders so that I can prioritize assignments"

2. **AC phải test được**, không phải mô tả. Nếu không thể viết test case từ AC, AC đó quá mơ hồ.
   - ❌ "Giao diện thân thiện"
   - ✅ "Given user nhập sai password 3 lần, when cố đăng nhập lần 4, then account bị lock 15 phút và hiện thông báo X"

3. **Must-have ≤ 60% total stories.** Nếu mọi thứ đều Must-have, không có gì là Must-have.

4. **Mỗi story phải có ít nhất 1 edge case AC.** DEV không tự nghĩ ra edge case, BA phải cho sẵn.

### Về Scope
- Viết "ngoài scope" TRƯỚC KHI viết stories — tránh vô tình thêm stories cho những thứ ngoài scope.
- Mỗi item ngoài scope phải có lý do (tech debt, timeline, không đủ data, dự kiến v2).

### Về Open Questions
- Không giả định để điền chỗ trống. Để trống và ghi vào Open Questions.
- Deadline cho mỗi open question phải trước ngày DEV bắt đầu sprint liên quan.

---

## Cách xử lý các tình huống đặc biệt

### Nhận Strategic Brief có nhiều epic
→ Tạo PRD riêng cho mỗi epic nếu epic đó > 5 stories, hoặc gộp nếu nhỏ hơn.

### Brief thiếu KPI / metric
→ Điền `[TBD — cần CEO confirm trước [ngày]]` vào metric, không tự bịa số.

### Có conflict giữa brief và yêu cầu kỹ thuật DEV nêu lên
→ Document conflict trong Open Questions, không tự resolve, ping CEO + Tech Lead.

### CEO muốn thêm scope sau khi PRD đã approved
→ Tạo PRD v1.1, highlight thay đổi bằng `[NEW]` hoặc `[CHANGED]`, yêu cầu sign-off lại.

### BA nhận yêu cầu trực tiếp không qua brief
→ Chạy `strategic-brief` skill trước, sau đó mới chạy `prd-generator`. Không skip bước này.

---

## Checklist trước khi gửi PRD cho CEO review

- [ ] Mọi story đều có ít nhất 2 AC (1 happy path + 1 edge case)
- [ ] Section "Ngoài scope" có ít nhất 3 items
- [ ] Không có AC nào dùng từ "user-friendly", "fast", "easy" mà không có số cụ thể
- [ ] Open Questions đã có owner và deadline
- [ ] Non-functional requirements đã có metric đo được
- [ ] Error states và empty states đã được define
- [ ] Story Points đã được để trống cho DEV estimate (không BA tự estimate)

---

## Handoff sang DEV

Sau khi PRD được sign-off, thêm dòng này:

```
---
## Handoff to DEV
PRD status: ✅ Approved — [CEO tên] + [Tech Lead tên] ngày [ngày]
Để tạo technical spec từ PRD này: dùng skill tech-spec với toàn bộ nội dung trên.
Sprint planning: [ngày]
Contact BA: [tên] — mọi câu hỏi về requirement gửi qua [Slack channel / email]
```
