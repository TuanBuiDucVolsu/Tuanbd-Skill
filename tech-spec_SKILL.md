---
name: tech-spec
description: >
  Dùng ngay khi DEV hoặc Tech Lead cần chuyển PRD thành Technical Specification
  để bắt đầu estimate và build. Trigger khi người dùng nói: "viết tech spec",
  "breakdown technical", "estimate sprint", "tạo task từ PRD", "thiết kế API",
  "lên kiến trúc cho tính năng này", "DEV cần document", hoặc khi nhận PRD
  có section "Handoff to DEV". Output đủ để DEV bắt đầu code, QA viết test,
  không cần họp thêm với BA.
---

# Tech-Spec Skill

Biến PRD thành Technical Specification — kiến trúc, API, task breakdown,
và estimate. DEV đọc xong là làm được, không cần dịch lại yêu cầu.

---

## Input chấp nhận

**Tốt nhất:** PRD đã approved (output từ `prd-generator` skill)

**Cũng chấp nhận:**
- User stories + AC riêng lẻ
- Mô tả tính năng từ BA/CEO
- Ticket Jira/Linear đã có AC

Nếu PRD thiếu AC hoặc NFR → ghi chú vào Tech Spec và ping BA trước khi estimate.

---

## Output chuẩn: Technical Specification

```markdown
# Tech Spec: [Tên tính năng]

**Version:** 1.0
**Ngày tạo:** [DD/MM/YYYY]
**Tech Lead / Author:** [Tên]
**Nguồn:** PRD v[X] — approved [ngày]
**Status:** Draft / In Review / Approved
**Sprint dự kiến:** Sprint [X] — bắt đầu [ngày]

---

## 1. Tổng quan kỹ thuật

### 1.1 Tóm tắt (TL;DR cho DEV mới onboard)
[3–5 câu: tính năng làm gì, impact đến hệ thống hiện tại như thế nào,
approach kỹ thuật chính là gì]

### 1.2 Kiến trúc tổng thể

**Affected components:**
| Component | Loại thay đổi | Mức độ ảnh hưởng |
|-----------|--------------|-----------------|
| [Service/Module A] | New / Modified / No change | High / Medium / Low |
| [Service/Module B] | ... | ... |

**Flow diagram (text-based):**
```
[Client] → [API Gateway] → [Service A] → [DB]
                        ↘ [Service B] → [Cache]
```

### 1.3 Tech stack & thư viện mới
| Package/Tool | Version | Lý do chọn | Alternative đã consider |
|-------------|---------|------------|------------------------|
| [Tên] | [v] | [Lý do] | [Tại sao không dùng alternative] |

---

## 2. Database Design

### 2.1 Schema changes

**Tables mới:**
```sql
CREATE TABLE [table_name] (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  [field_1]   [TYPE] NOT NULL,
  [field_2]   [TYPE],
  created_at  TIMESTAMP DEFAULT NOW(),
  updated_at  TIMESTAMP DEFAULT NOW(),
  
  CONSTRAINT fk_[name] FOREIGN KEY ([field]) REFERENCES [table]([id])
);

-- Index
CREATE INDEX idx_[table]_[field] ON [table_name]([field_1]);
```

**Tables modified:**
```sql
ALTER TABLE [existing_table]
  ADD COLUMN [new_field] [TYPE] DEFAULT [value],
  ADD COLUMN [new_field_2] [TYPE];
```

**Migration strategy:**
- [ ] Backward compatible? Yes / No
- [ ] Cần data migration script? Yes / No
- [ ] Rollback plan: [mô tả]

### 2.2 Data flow
[Mô tả dữ liệu đi từ đâu đến đâu, transform như thế nào]

---

## 3. API Design

### 3.1 Endpoints mới

#### `POST /api/v1/[resource]`
**Mục đích:** [Mô tả ngắn]
**Auth required:** Yes (Bearer token) / No
**Rate limit:** [X] requests/minute

**Request:**
```json
{
  "field_1": "string (required)",
  "field_2": 123,
  "field_3": {
    "nested": "object"
  }
}
```

**Response 200:**
```json
{
  "id": "uuid",
  "field_1": "string",
  "created_at": "ISO8601"
}
```

**Error responses:**
| Status | Code | Khi nào xảy ra |
|--------|------|---------------|
| 400 | VALIDATION_ERROR | Input không hợp lệ |
| 401 | UNAUTHORIZED | Token hết hạn / thiếu |
| 404 | NOT_FOUND | Resource không tồn tại |
| 409 | CONFLICT | [Duplicate / Race condition] |
| 500 | INTERNAL_ERROR | Unexpected server error |

#### `GET /api/v1/[resource]/:id`
[Lặp lại cấu trúc]

### 3.2 Endpoints modified
[List endpoints hiện tại bị thay đổi, ghi rõ breaking change hay không]

---

## 4. Task Breakdown & Estimate

### Epic: [Tên epic — map với PRD]

| Task ID | Task | Assignee | Estimate | Dependencies | Priority |
|---------|------|----------|----------|--------------|----------|
| T-001 | [Setup DB schema + migration] | [DEV] | [X]h | — | P0 |
| T-002 | [Build API endpoint POST /...] | [DEV] | [X]h | T-001 | P0 |
| T-003 | [Unit tests cho service layer] | [DEV] | [X]h | T-002 | P0 |
| T-004 | [Integration test] | [DEV] | [X]h | T-002, T-003 | P1 |
| T-005 | [Frontend component] | [DEV] | [X]h | T-002 | P1 |
| T-006 | [E2E test] | [QA/DEV] | [X]h | T-005 | P1 |

**Total estimate:** [X] points / [Y] ngày dev
**Buffer (20%):** [Z] ngày
**Dự kiến hoàn thành:** [ngày]

### Dependency graph
```
T-001 → T-002 → T-003 → T-004
              ↘ T-005 → T-006
```

---

## 5. Security Considerations

- [ ] Input validation: [mô tả approach]
- [ ] Authentication: [JWT / Session / API Key]
- [ ] Authorization: [RBAC / ABAC / custom]
- [ ] SQL Injection prevention: [ORM / parameterized queries]
- [ ] Rate limiting: [X req/min per user]
- [ ] Sensitive data: [encryption at rest / in transit]
- [ ] Audit log: [những action nào cần log]

---

## 6. Performance & Scalability

### 6.1 Expected load
| Metric | Current | Expected after launch |
|--------|---------|----------------------|
| Requests/day | [X] | [Y] |
| DB queries/request | [X] | [Y] |
| P95 latency | [X]ms | Target < [Y]ms |

### 6.2 Optimization plan
- **Caching strategy:** [Redis / in-memory / CDN] — cache key: [pattern]
- **Query optimization:** [Index, N+1 prevention, pagination]
- **Async processing:** [Queue nếu cần background job]

### 6.3 Load testing plan
- Tool: [k6 / Locust / Artillery]
- Scenario: [X] concurrent users trong [Y] phút
- Pass criteria: P95 < [X]ms, error rate < 1%

---

## 7. Testing Strategy

### 7.1 Unit tests
| Module | Coverage target | Framework |
|--------|----------------|-----------|
| [Service layer] | 80% | [Jest/pytest/...] |
| [Utils] | 90% | [...] |

### 7.2 Integration tests
- [API endpoint tests — happy path + error cases]
- [DB transaction tests]

### 7.3 Test cases từ AC (map với PRD)

| AC từ PRD | Test case | Type | Expected result |
|-----------|-----------|------|----------------|
| AC1: Given... | Test_[tên] | Unit | [Mô tả] |
| AC2 (Edge): Given... | Test_[tên] | Integration | [Mô tả] |

---

## 8. Deployment Plan

### 8.1 Checklist trước deploy
- [ ] DB migration tested trên staging
- [ ] Feature flag setup (nếu dùng)
- [ ] Rollback plan documented
- [ ] Monitoring/alerting configured
- [ ] Load test passed

### 8.2 Deploy strategy
- [ ] Blue/green / Canary / Rolling / Direct
- [ ] Smoke test sau deploy: [list các check]
- [ ] Rollback trigger: [điều kiện nào thì rollback]

### 8.3 Feature flag (nếu có)
```
FLAG_NAME: feature_[name]
Default: OFF
Enable for: [% users / specific users / all]
Remove flag after: [ngày]
```

---

## 9. Monitoring & Alerting

| Metric | Tool | Alert threshold | On-call action |
|--------|------|----------------|----------------|
| Error rate | [Datadog/Grafana] | > 1% trong 5 phút | [Action] |
| P95 latency | [...] | > [X]ms | [Action] |
| DB connection pool | [...] | > 80% | [Action] |

---

## 10. Open Technical Questions

| # | Câu hỏi | Owner | Deadline | Status |
|---|---------|-------|----------|--------|
| 1 | [Câu hỏi kỹ thuật còn pending] | [Tech Lead] | [ngày] | Open |
| 2 | [Trade-off cần quyết định] | [CTO/Tech Lead] | [ngày] | Open |

---

## 11. Sign-off

| Vai trò | Tên | Ngày | Ghi chú |
|---------|-----|------|---------|
| Tech Lead | | | |
| BA (confirm requirements) | | | |
| Security review (nếu cần) | | | |

---
*Tech Spec được tạo bằng Claude · tech-spec skill v1.0*
*Source PRD: [link hoặc tên file]*
```

---

## Quy tắc viết Tech Spec

1. **Không assume — document explicit.** Nếu chưa quyết định, ghi vào Open Questions.
2. **API contract trước code.** Viết request/response format trước khi code — BA và FE có thể review song song.
3. **Breaking changes phải có migration path.** Không chỉ ghi "thay đổi field X" mà phải ghi "client cũ xử lý như thế nào".
4. **Estimate phải có buffer.** Luôn cộng 20% vào estimate raw. Không negotiate buffer xuống — negotiate scope.
5. **Security section không được skip.** Dù tính năng nhỏ, phải check 5 điểm cơ bản: auth, input validation, rate limit, audit log, sensitive data.

---

## Cách xử lý tình huống đặc biệt

### PRD còn Open Questions chưa resolve
→ Không estimate task phụ thuộc vào open question đó. Ghi rõ: "Task X blocked bởi OQ-[số] trong PRD."

### Tính năng ảnh hưởng nhiều service (cross-team)
→ List tất cả teams bị ảnh hưởng, yêu cầu Tech Lead các team review trước khi approve.

### Cần thay đổi DB schema trên production có data
→ Bắt buộc có migration script + rollback script + estimate downtime. Không deploy nếu thiếu.

### DEV estimate khác xa BA/CEO expect
→ Không tự điều chỉnh estimate cho vừa deadline. Document gap, present options: (A) giữ scope, extend deadline; (B) giữ deadline, cut scope; (C) thêm resource.

---

## Handoff sang QA

Sau khi Tech Spec approved:
```
---
## Handoff to QA
Tech Spec status: ✅ Approved ngày [ngày]
Test cases từ AC: xem Section 7.3
Staging URL: [URL]
Test data setup: [hướng dẫn hoặc script]
Contact DEV: [tên] — bug report qua [Jira project / Slack channel]
```
