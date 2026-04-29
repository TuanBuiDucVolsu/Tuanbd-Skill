---
name: retro-summary
description: >
  Dùng ngay sau mỗi sprint, release, hoặc project milestone để tổng hợp kết quả
  thực tế so với mục tiêu ban đầu và cải tiến skill cho chu kỳ tiếp theo.
  Trigger khi người dùng nói: "tổng kết sprint", "retro", "retrospective",
  "đánh giá kết quả", "lessons learned", "review kết quả release", "cải tiến quy trình",
  hoặc sau khi QA sign-off và feature được ship. Output gồm 2 phần:
  (1) Retro Report cho CEO và team, (2) Skill improvement notes để nâng cấp
  các skill đã dùng trong chu kỳ — đây là vòng lặp GSD quan trọng nhất.
---

# Retro-Summary Skill

Tổng kết chu kỳ làm việc, đo kết quả thực tế vs mục tiêu, và — quan trọng nhất —
cải tiến skill cho lần sau. Đây là vòng lặp khép kín của GSD × Claude Skills.

---

## Input chấp nhận

**Tốt nhất:** Tất cả tài liệu của chu kỳ:
- Strategic Brief gốc
- PRD version cuối
- Tech Spec
- QA Checklist + Bug Log
- Dữ liệu thực tế sau launch (nếu có)

**Chấp nhận tối thiểu:**
- Brief gốc + kết quả thực tế (team tự mô tả)
- Meeting notes của buổi retro

**Nếu thiếu data thực tế:** Ghi rõ "Chưa có data — review lại sau [X] ngày post-launch."

---

## Quy trình chạy skill

1. **Thu thập input** từ tất cả phases
2. **So sánh mục tiêu vs thực tế** theo từng dimension
3. **Phân tích nguyên nhân** (không blame người — blame process)
4. **Tổng hợp Retro Report** cho CEO và team
5. **Tạo Skill Improvement Notes** — đây là output quan trọng nhất về dài hạn

---

## Output 1: Retro Report

```markdown
# Retro Report: [Tên dự án / Sprint X]

**Chu kỳ:** [ngày bắt đầu] → [ngày kết thúc]
**Facilitator:** [CEO / Tech Lead / BA]
**Participants:** [List tên hoặc vai trò]
**Ngày tạo:** [DD/MM/YYYY]

---

## 1. Kết quả thực tế vs Mục tiêu

### 1.1 KPI Dashboard

| KPI | Target (Brief) | Thực tế | Delta | Đánh giá |
|-----|---------------|---------|-------|----------|
| [KPI 1] | [X] | [Y] | [+/-Z%] | ✅ Đạt / ⚠️ Gần đạt / 🔴 Không đạt |
| [KPI 2] | ... | ... | ... | ... |
| [Deadline] | [ngày target] | [ngày thực tế] | [+/-X ngày] | ✅ / ⚠️ / 🔴 |
| [Budget] | [X] | [Y] | [+/-Z%] | ✅ / ⚠️ / 🔴 |

**Overall:** ✅ Thành công / ⚠️ Partial / 🔴 Cần cải thiện

### 1.2 Delivery Summary

| Phase | Estimate | Thực tế | Chênh lệch | Lý do chính |
|-------|----------|---------|------------|-------------|
| Brief → PRD | [X] ngày | [Y] ngày | [+/-] | [...] |
| PRD → Tech Spec | [...] | [...] | [...] | [...] |
| Dev | [...] | [...] | [...] | [...] |
| QA | [...] | [...] | [...] | [...] |
| Total | [...] | [...] | [...] | [...] |

### 1.3 Scope Delta

**Scope thêm vào (không có trong brief gốc):**
- [Item A] — thêm vào ngày [X] — lý do: [...]
- [Item B] — ...

**Scope bị cắt (có trong brief nhưng không ship):**
- [Item C] — cắt ngày [X] — lý do: [...] — dự kiến: v[next]
- [Item D] — ...

---

## 2. What Went Well ✅

*Những gì hoạt động tốt — nên giữ và nhân rộng*

| # | Điều tốt | Impact | Nhân tố thành công |
|---|---------|--------|-------------------|
| 1 | [Ví dụ: Brief rõ ràng → BA không cần hỏi lại] | [Tiết kiệm ~2 ngày] | [Dùng strategic-brief skill] |
| 2 | [...] | [...] | [...] |
| 3 | [...] | [...] | [...] |

---

## 3. What Didn't Go Well ⚠️

*Không blame cá nhân — phân tích process và system*

| # | Vấn đề | Root cause | Impact | Phase xảy ra |
|---|--------|-----------|--------|-------------|
| 1 | [Ví dụ: Estimate sai 3x] | [AC trong PRD thiếu edge case → DEV phải refactor] | [Delay 4 ngày] | Dev |
| 2 | [...] | [...] | [...] | [...] |
| 3 | [...] | [...] | [...] | [...] |

---

## 4. Action Items

*Cải tiến cụ thể — có owner, có deadline, không phải "sẽ cố gắng hơn"*

| # | Action | Owner | Deadline | Metric thành công |
|---|--------|-------|----------|------------------|
| 1 | [Ví dụ: BA bổ sung edge case section vào prd-generator skill] | [BA] | [ngày] | [PRD tiếp theo có ≥3 edge case AC mỗi story] |
| 2 | [DEV thêm load test vào Definition of Done] | [Tech Lead] | [ngày] | [100% P0 feature có load test trước ship] |
| 3 | [...] | [...] | [...] | [...] |

---

## 5. Shoutouts 🎉

*Ghi nhận những đóng góp nổi bật — không bỏ qua phần này*

- [Tên]: [Đóng góp cụ thể và impact]
- [Tên]: [...]

---

## 6. Decisions Made

| Quyết định | Người quyết định | Ngày | Context |
|-----------|-----------------|------|---------|
| [Ví dụ: Cut feature X sang v2] | CEO | [ngày] | [Lý do] |
| [...] | [...] | [...] | [...] |

---

## 7. Carry-over sang chu kỳ tiếp theo

**Bugs known, chưa fix:**
- [BUG-X]: [Mô tả] — severity: [High/Med/Low] — target fix: Sprint [X]

**Tech debt được accept:**
- [Item]: [Lý do accept] — phải giải quyết trước: [ngày/milestone]

**Features bị cut, plan cho v[next]:**
- [Feature]: [Dự kiến Sprint/Quarter X]

---
*Retro Report được tạo bằng Claude · retro-summary skill v1.0*
```

---

## Output 2: Skill Improvement Notes

*Đây là output quan trọng nhất — giúp hệ thống skill ngày càng tốt hơn*

```markdown
# Skill Improvement Notes: Chu kỳ [X]

**Ngày:** [DD/MM/YYYY]
**Dựa trên:** Retro Report [tên dự án]

---

## strategic-brief (CEO skill)

**Vấn đề gặp phải trong chu kỳ này:**
- [Ví dụ: CEO input quá ngắn, brief thiếu constraints]

**Cải tiến đề xuất:**
- [ ] Thêm câu hỏi "Điều gì sẽ khiến dự án này fail?" vào phần input gathering
- [ ] Bổ sung ví dụ bad brief vs good brief vào SKILL.md

**Priority:** High / Medium / Low
**Owner:** [BA / Tech Lead]
**Target:** Áp dụng từ Sprint [X]

---

## prd-generator (BA skill)

**Vấn đề gặp phải:**
- [Ví dụ: AC thiếu edge case → DEV phải hỏi lại nhiều lần]

**Cải tiến đề xuất:**
- [ ] Bổ sung checklist "mỗi story phải có ít nhất 2 edge case AC" vào quy tắc
- [ ] Thêm template cho error states — phần này hay bị bỏ sót

**Priority:** High
**Owner:** [BA]

---

## review-align (CEO + BA skill)

**Vấn đề gặp phải:**
- [Ví dụ: Gap Report quá dài, CEO không đọc hết]

**Cải tiến đề xuất:**
- [ ] Giới hạn Gap Report tối đa 1 trang — move detail sang appendix
- [ ] Thêm "Executive Summary 3 dòng" lên đầu report

**Priority:** Medium
**Owner:** [BA]

---

## tech-spec (DEV skill)

**Vấn đề gặp phải:**
- [Ví dụ: Estimate sai vì không tính migration time]

**Cải tiến đề xuất:**
- [ ] Thêm "DB migration estimate" thành line item riêng trong task breakdown
- [ ] Bắt buộc có "Rollback plan" trong mọi spec có DB change

**Priority:** High
**Owner:** [Tech Lead]

---

## qa-checklist (BA + DEV skill)

**Vấn đề gặp phải:**
- [Ví dụ: Regression test bị skip vì time pressure]

**Cải tiến đề xuất:**
- [ ] Thêm "Regression test là P0, không skip được" vào Go/No-Go criteria
- [ ] Giảm số regression test case bằng cách chỉ test smoke cho unrelated features

**Priority:** High
**Owner:** [Tech Lead + BA]

---

## Skill mới cần tạo

| Skill | Lý do | Priority | Owner | Target Sprint |
|-------|-------|----------|-------|--------------|
| [release-notes] | [Mỗi release DEV phải viết tay tốn 2h] | High | [DEV] | Sprint [X] |
| [...] | [...] | [...] | [...] | [...] |

---

## Quy trình mới cần document

- [Ví dụ: "Khi nào cần tạo PRD riêng vs chỉ cần ticket?" → cần SOP rõ]
- [...]

---
*Skill Improvement Notes · retro-summary skill v1.0*
*Áp dụng ngay từ chu kỳ tiếp theo — không để improvement notes nằm trong file*
```

---

## Quy tắc chạy retro

### Về process
1. **Retro tổ chức trong 48h sau khi ship** — không để quá 1 tuần, context bị mất.
2. **Không blame người — blame process.** "BA viết thiếu AC" → "quy trình review PRD chưa có checklist edge case."
3. **Action items phải có metric.** "Cải thiện communication" không phải action item. "BA và DEV sync 15 phút mỗi sáng trong sprint" mới là action item.
4. **Skill Improvement Notes phải được apply ngay sprint sau.** Nếu không apply, đừng tạo.

### Về GSD mindset
- **Đo kết quả thực tế, không đo effort.** "Team làm việc rất chăm chỉ" không phải metric.
- **Cải tiến nhỏ và áp dụng ngay > cải tiến lớn và không bao giờ làm.** Mỗi retro chỉ cần 2–3 action items, không phải 20.
- **Skill improvement là đầu tư.** Mỗi lần cải tiến skill tiết kiệm time ở tất cả dự án tiếp theo.

---

## Tần suất chạy retro

| Loại | Tần suất | Input | Thời gian tổ chức |
|------|----------|-------|------------------|
| Sprint retro | Sau mỗi sprint (~2 tuần) | Sprint log + velocity | 60 phút |
| Release retro | Sau mỗi release lớn | Tất cả tài liệu + data post-launch | 90 phút |
| Quarterly retro | Mỗi quý | Tổng hợp sprint retros + OKR review | 3 giờ |
| Skill audit | Mỗi quý | Tất cả Skill Improvement Notes | 2 giờ |
