---
name: qa-checklist
description: >
  Dùng ngay khi BA hoặc DEV cần tạo checklist kiểm thử từ PRD và Tech Spec
  trước khi release. Trigger khi người dùng nói: "tạo test case", "viết checklist QA",
  "chuẩn bị test", "release checklist", "kiểm tra trước khi ship", "QA sign-off",
  "regression test", hoặc khi nhận được PRD/Tech Spec có section "Handoff to QA".
  Output là checklist có thể chạy ngay — không cần đọc lại PRD hay Tech Spec.
---

# QA Checklist Skill

Biến PRD + Tech Spec thành checklist kiểm thử đầy đủ — functional, edge case,
performance, security. QA (hoặc DEV tự test) chạy xong là biết có ship được không.

---

## Input chấp nhận

**Tốt nhất:** PRD approved + Tech Spec approved

**Chấp nhận:**
- Chỉ PRD (sẽ tạo functional checklist, thiếu technical checks)
- Chỉ AC từ user stories
- Mô tả tính năng từ BA/DEV

Nếu thiếu AC cụ thể → yêu cầu BA cung cấp trước khi tạo checklist.

---

## Output chuẩn: QA Checklist

```markdown
# QA Checklist: [Tên tính năng]

**Version:** 1.0
**Ngày tạo:** [DD/MM/YYYY]
**Tester:** [Tên / chưa assign]
**Source:** PRD v[X] + Tech Spec v[X]
**Environment:** Staging / UAT / Production
**Build/Version:** [build number]

---

## Tóm tắt

| Category | Total | Passed | Failed | Blocked | Skip |
|----------|-------|--------|--------|---------|------|
| Functional | [X] | | | | |
| Edge Cases | [X] | | | | |
| UI/UX | [X] | | | | |
| API | [X] | | | | |
| Performance | [X] | | | | |
| Security | [X] | | | | |
| Regression | [X] | | | | |
| **TOTAL** | **[X]** | | | | |

**Go/No-Go decision:** ⬜ GO ⬜ NO-GO
**Sign-off:** [Tên] ngày [ngày]

---

## 1. Functional Tests (Happy Path)

<!-- Map với user stories trong PRD -->

### Epic 1: [Tên epic]

#### TC-001: [Tên test case — map với Story 1.1]
- **Precondition:** [Điều kiện cần có trước khi test]
- **Test data:** [Data cụ thể dùng để test]
- **Steps:**
  1. [Bước 1]
  2. [Bước 2]
  3. [Bước 3]
- **Expected result:** [Kết quả mong đợi cụ thể — có số, có text copy]
- **AC reference:** AC1 trong Story 1.1
- **Priority:** P0 / P1 / P2
- **Result:** ⬜ Pass ⬜ Fail ⬜ Blocked
- **Notes:** [Để trống hoặc ghi bug ID nếu fail]

#### TC-002: [Tên test case]
[Lặp lại cấu trúc]

---

## 2. Edge Cases & Negative Tests

#### TC-E001: [Tên edge case — map với AC edge case trong PRD]
- **Scenario:** [Mô tả tình huống bất thường]
- **Test data:** [Data cụ thể trigger edge case]
- **Steps:** [...]
- **Expected result:** [Error message chính xác / behavior mong đợi]
- **AC reference:** AC (edge) trong Story [X.Y]
- **Result:** ⬜ Pass ⬜ Fail ⬜ Blocked

#### TC-E002: Input validation
- **Scenario:** User nhập data không hợp lệ
- **Test cases:**
  - [ ] Field bắt buộc để trống → hiển thị "[error message chính xác]"
  - [ ] Input vượt max length → [behavior]
  - [ ] Special characters (`, ', ", <, >) → [không crash, sanitized]
  - [ ] SQL injection attempt → [rejected, không lộ error detail]
  - [ ] XSS attempt (`<script>alert(1)</script>`) → [escaped, không execute]

#### TC-E003: Concurrent actions
- **Scenario:** 2 user thực hiện cùng action cùng lúc
- **Expected:** [Race condition được handle — không duplicate, không mất data]
- **Result:** ⬜ Pass ⬜ Fail ⬜ Skip

---

## 3. UI/UX Checks

### 3.1 Layout & Responsive
- [ ] Desktop (1440px): layout đúng theo design
- [ ] Tablet (768px): không bị vỡ layout
- [ ] Mobile (375px): usable, không bị overflow
- [ ] Dark mode (nếu support): colors đúng

### 3.2 States
- [ ] Loading state: spinner/skeleton hiển thị trong khi fetch
- [ ] Empty state: hiển thị đúng theo spec khi không có data
- [ ] Error state: error message đúng copy, có CTA phù hợp
- [ ] Success state: confirmation rõ ràng sau action thành công
- [ ] Disabled state: button/input disabled khi đúng điều kiện

### 3.3 Accessibility
- [ ] Keyboard navigation hoạt động (Tab, Enter, Escape)
- [ ] Screen reader: các element có aria-label đúng
- [ ] Contrast ratio: text đạt WCAG AA (4.5:1)
- [ ] Focus indicator visible

### 3.4 Copy & Content
- [ ] Tất cả text đúng theo spec (không typo, không placeholder)
- [ ] Error messages đúng theo spec trong PRD
- [ ] Ngôn ngữ nhất quán (không mix tiếng Anh/Việt nếu không theo design)

---

## 4. API Tests

### 4.1 Endpoints mới (từ Tech Spec)

#### `[METHOD] /api/v1/[endpoint]`
- [ ] **Happy path:** Request đúng → Response 200 đúng schema
- [ ] **Auth:** Request không có token → 401
- [ ] **Auth:** Request token hết hạn → 401
- [ ] **Validation:** Thiếu required field → 400 với error message rõ
- [ ] **Not found:** ID không tồn tại → 404
- [ ] **Response time:** < [X]ms ở P95

**Response schema check:**
- [ ] Tất cả fields có trong response schema
- [ ] Data types đúng (string/number/boolean/null)
- [ ] Timestamps đúng format ISO8601
- [ ] Không lộ sensitive fields (password, internal IDs)

### 4.2 Rate limiting
- [ ] Vượt [X] requests/minute → 429 Too Many Requests
- [ ] Retry-After header có trong response 429

---

## 5. Performance Tests

### 5.1 Page load
- [ ] Time to First Byte (TTFB) < [X]ms
- [ ] Largest Contentful Paint (LCP) < [X]ms
- [ ] Không có layout shift (CLS < 0.1)

### 5.2 API response time
| Endpoint | P50 target | P95 target | Result |
|----------|-----------|-----------|--------|
| [GET /...] | < [X]ms | < [Y]ms | |
| [POST /...] | < [X]ms | < [Y]ms | |

### 5.3 Load test (nếu P0 feature)
- Tool: [k6 / manual]
- Scenario: [X] users đồng thời trong [Y] phút
- Pass criteria: error rate < 1%, P95 < [Z]ms
- **Result:** ⬜ Pass ⬜ Fail ⬜ Skip

---

## 6. Security Checks

- [ ] **Auth bypass:** Truy cập resource của user khác → 403
- [ ] **IDOR:** Thay đổi ID trong request để truy cập data không được phép → 403
- [ ] **CSRF:** Request từ origin khác bị reject (nếu applicable)
- [ ] **Sensitive data in logs:** Không có password/token trong server logs
- [ ] **HTTPS only:** HTTP request bị redirect sang HTTPS
- [ ] **Error messages:** 500 error không lộ stack trace ra client

---

## 7. Regression Tests

<!-- Các tính năng hiện tại có thể bị ảnh hưởng bởi thay đổi này -->

| Tính năng | Test case | Expected | Result |
|-----------|-----------|----------|--------|
| [Feature hiện tại A] | [Smoke test] | [Vẫn hoạt động bình thường] | ⬜ Pass ⬜ Fail |
| [Feature hiện tại B] | [Smoke test] | [...] | ⬜ Pass ⬜ Fail |

---

## 8. Bug Log

| Bug ID | Mô tả | Severity | TC liên quan | Status |
|--------|-------|----------|-------------|--------|
| BUG-001 | [Mô tả bug] | Critical/High/Medium/Low | TC-[X] | Open/Fixed/Won't Fix |

---

## 9. Go/No-Go Criteria

**Tự động NO-GO nếu:**
- [ ] Bất kỳ P0 test case nào fail
- [ ] Bất kỳ Critical bug nào còn Open
- [ ] Security check fail
- [ ] Performance test fail với margin > 20%

**Có thể GO nếu:**
- [ ] Tất cả P0 passed
- [ ] P1 fail < 10% và có workaround documented
- [ ] Không có Critical/High bug open
- [ ] Known issues đã được document và accepted bởi CEO/Product

---
*QA Checklist được tạo bằng Claude · qa-checklist skill v1.0*
```

---

## Quy tắc tạo checklist

1. **Map từng test case với AC trong PRD.** Không có AC = không có test case = BA thiếu sót, không phải QA tự nghĩ.
2. **Expected result phải cụ thể.** "Button hiện ra" không đủ. "Button 'Xác nhận' màu #0066CC hiện ra và clickable" mới đủ.
3. **Test data phải explicit.** Không ghi "nhập email hợp lệ" — ghi "nhập test@company.com".
4. **P0 = ship blocker.** Nếu P0 fail, không ship. Không negotiate.
5. **Regression test không skip.** Dù tính năng nhỏ, phải check ít nhất 3 tính năng liên quan.

---

## Severity levels

| Level | Định nghĩa | Ví dụ | SLA fix |
|-------|-----------|-------|---------|
| Critical | App crash, data loss, security hole | Login không được, payment fail | Trước khi ship |
| High | Feature chính không hoạt động | Không submit được form | Trước khi ship |
| Medium | Feature hoạt động nhưng UX tệ | Loading 5s thay vì 1s | Sprint tiếp theo |
| Low | Cosmetic, minor UX | Typo, màu lệch 1px | Backlog |

---

## Handoff sau QA

```
---
## QA Sign-off
Status: ✅ GO / 🔴 NO-GO
Tester: [Tên] ngày [ngày]
Passed: [X]/[Total] test cases
Open bugs: [X] Critical, [X] High, [X] Medium
Known issues shipped: [list nếu có, với CEO approval]
Release notes: dùng skill release-notes để generate
```
