# Hướng dẫn: GSD × Claude Skills
### Quy trình chuẩn hóa cho CEO · DEV · BA

---

## Mục lục

1. [Tổng quan hệ thống](#1-tổng-quan-hệ-thống)
2. [6 Skills và cách dùng](#2-6-skills-và-cách-dùng)
3. [Quy trình làm việc theo phase](#3-quy-trình-làm-việc-theo-phase)
4. [Cách kết hợp với Claude](#4-cách-kết-hợp-với-claude)
5. [Setup Claude Projects (khuyến nghị)](#5-setup-claude-projects-khuyến-nghị)
6. [Triển khai theo GSD](#6-triển-khai-theo-gsd)
7. [FAQ](#7-faq)

---

## 1. Tổng quan hệ thống

### GSD là gì?
**Get Shit Done** — làm nhanh, đo kết quả, cải tiến liên tục. Không overthink, không chờ hoàn hảo.

### Claude Skills là gì?
Mỗi Skill là một file `SKILL.md` chứa hướng dẫn cụ thể để Claude xử lý đúng một loại công việc. Thay vì giải thích lại mỗi lần, bạn đưa skill cho Claude một lần — Claude tự biết làm gì, format nào, output ra sao.

### Tại sao kết hợp?
| Không có Skills | Có Skills |
|----------------|-----------|
| Mỗi người dùng Claude theo cách riêng | Toàn team output cùng format |
| CEO brief mơ hồ → BA hỏi lại nhiều lần | Brief chuẩn → BA làm được ngay |
| DEV nhận PRD thiếu AC → họp thêm | PRD đầy đủ → DEV estimate được luôn |
| Mỗi sprint phải làm lại từ đầu | Skill cải tiến dần, team nhanh hơn theo thời gian |

---

## 2. Six Skills và cách dùng

### Bộ 6 skills theo luồng CEO → BA → DEV

```
[CEO] → strategic-brief → [BA] → prd-generator → review-align
                                                        ↓
                                    [DEV] ← tech-spec ←┘
                                        ↓
                                   qa-checklist
                                        ↓
                                  retro-summary → (cải tiến skills)
```

---

### Skill 1 — Tóm tắt Chiến lược (`strategic-brief`)
**Dùng bởi:** CEO  
**Trigger:** "Tôi muốn làm X", "Lên kế hoạch cho Y", "Cần brief cho dự án Z"  
**Input:** Ý tưởng thô, câu nói miệng, email, bullet points lộn xộn  
**Output:** Strategic Brief 1 trang — mục tiêu, scope, KPI, deadline, rủi ro  

**Ví dụ dùng:**
```
CEO gõ vào Claude:
"Tôi muốn làm tính năng cho phép nhân viên đăng ký lịch làm việc từ xa.
Hiện tại đang dùng email qua lại rất mất thời gian."

→ Claude xuất Brief chuẩn, có KPI, có scope, có bước tiếp theo
```

---

### Skill 2 — Tạo Tài liệu Yêu cầu Sản phẩm (`prd-generator`)
**Dùng bởi:** BA  
**Trigger:** "Viết PRD", "Tạo user stories", "Breakdown tính năng"  
**Input:** Strategic Brief đã approved  
**Output:** PRD đầy đủ — user stories, acceptance criteria, NFR, open questions  

**Ví dụ dùng:**
```
BA gõ vào Claude:
[paste toàn bộ Strategic Brief từ CEO]
"Tạo PRD từ brief này."

→ Claude xuất PRD với đủ user stories, AC happy path + edge case
```

---

### Skill 3 — Rà soát & Đồng bộ (`review-align`)
**Dùng bởi:** CEO + BA  
**Trigger:** "Review PRD", "Check brief vs PRD", "Chuẩn bị họp review"  
**Input:** Strategic Brief + PRD (cả hai)  
**Output:** Gap Report — chỉ highlight điểm lệch, không đọc lại toàn bộ  

**Ví dụ dùng:**
```
CEO hoặc BA gõ vào Claude:
[paste Brief] + [paste PRD]
"So sánh và tìm điểm lệch."

→ Claude xuất Gap Report: Critical gaps cần xử lý, điểm đã aligned, câu hỏi cho buổi review
```

---

### Skill 4 — Đặc tả Kỹ thuật (`tech-spec`)
**Dùng bởi:** DEV / Tech Lead  
**Trigger:** "Viết tech spec", "Breakdown technical", "Lên kiến trúc cho tính năng này"  
**Input:** PRD đã approved  
**Output:** Technical Spec — DB schema, API design, task breakdown, estimate, deploy plan  

**Ví dụ dùng:**
```
DEV gõ vào Claude:
[paste PRD]
"Tạo technical spec và breakdown tasks."

→ Claude xuất spec với API endpoints, schema SQL, task list có estimate
```

---

### Skill 5 — Danh sách Kiểm thử (`qa-checklist`)
**Dùng bởi:** BA + DEV  
**Trigger:** "Tạo test case", "Viết checklist QA", "Chuẩn bị test trước release"  
**Input:** PRD + Tech Spec  
**Output:** Checklist đầy đủ — functional, edge case, API, performance, security, regression  

**Ví dụ dùng:**
```
BA hoặc DEV gõ vào Claude:
[paste PRD] + [paste Tech Spec]
"Tạo QA checklist cho tính năng này."

→ Claude xuất checklist có thể chạy ngay, map từng test case với AC trong PRD
```

---

### Skill 6 — Tổng kết & Cải tiến (`retro-summary`)
**Dùng bởi:** CEO + All  
**Trigger:** "Tổng kết sprint", "Retro", "Đánh giá kết quả release"  
**Input:** Brief gốc + kết quả thực tế + bug log (càng đầy đủ càng tốt)  
**Output:** Retro Report (KPI thực tế vs target) + Skill Improvement Notes (cải tiến skills cho lần sau)  

**Ví dụ dùng:**
```
CEO/Team lead gõ vào Claude:
[paste Brief gốc] + "Kết quả thực tế: launch trễ 3 ngày, KPI đạt 80%, 2 bug high severity"
"Tổng kết sprint này."

→ Claude xuất Retro Report + danh sách cải tiến cụ thể cho từng skill
```

---

## 3. Quy trình làm việc theo phase

### Phase 1 — CEO: Định hướng
```
Input thô của CEO
        ↓
   [strategic-brief]
        ↓
Strategic Brief 1 trang
(approved → Handoff to BA)
```
**Thời gian tiết kiệm:** Brief thường mất 2–4 giờ viết → xuống còn 30 phút

---

### Phase 2 — BA: Phân tích yêu cầu
```
Strategic Brief
      ↓
 [prd-generator]
      ↓
  PRD đầy đủ
(draft → gửi CEO review)
```
**Thời gian tiết kiệm:** PRD thường mất 1–2 ngày → xuống còn 3–4 giờ

---

### Phase 3 — CEO + BA: Review & Align
```
Brief + PRD
     ↓
[review-align]
     ↓
  Gap Report
(họp 30–60 phút, chỉ quyết định gap)
```
**Thời gian tiết kiệm:** Buổi review thường 2–3 giờ đọc lại → xuống còn 30–60 phút

---

### Phase 4 — DEV: Build
```
PRD approved
     ↓
 [tech-spec]
     ↓
Technical Spec + Tasks
(sprint planning → bắt đầu code)
```
**Thời gian tiết kiệm:** Tech spec thường mất 1 ngày → xuống còn 2–3 giờ

---

### Phase 5 — BA + DEV: QA
```
PRD + Tech Spec
       ↓
 [qa-checklist]
       ↓
Checklist đầy đủ
(chạy test → Go/No-Go)
```
**Thời gian tiết kiệm:** Viết test case thủ công 4–8 giờ → xuống còn 1 giờ

---

### Phase 6 — CEO + All: Retrospective
```
Kết quả thực tế
       ↓
 [retro-summary]
       ↓
Retro Report + Skill Improvements
(cập nhật skills → vòng lặp cải tiến)
```
**Giá trị:** Mỗi chu kỳ team nhanh hơn vì skills được nâng cấp liên tục

---

## 4. Cách kết hợp với Claude

### Cách 1 — Thủ công (bắt đầu ngay, không cần setup)

**Bước 1:** Mở file `SKILL.md` tương ứng với việc cần làm  
**Bước 2:** Copy toàn bộ nội dung file  
**Bước 3:** Paste vào đầu hội thoại mới trên Claude  
**Bước 4:** Thêm yêu cầu của bạn phía sau  
**Bước 5:** Claude tự nhận trigger và apply đúng skill  

```
Ví dụ hội thoại:

[paste toàn bộ nội dung strategic-brief/SKILL.md]

---

Yêu cầu: Tôi muốn làm app quản lý lịch họp nội bộ.
Hiện tại team đang dùng email và Zalo để đặt lịch, rất lộn xộn,
hay bị double-booking. Có khoảng 200 nhân viên sẽ dùng.
```

**Ưu điểm:** Không cần setup, dùng được ngay  
**Nhược điểm:** Phải paste lại mỗi hội thoại mới  

---

### Cách 2 — Claude Projects (khuyến nghị cho team)

Claude Projects cho phép lưu skill vĩnh viễn — team mở Project là dùng được, không cần paste.

**Xem hướng dẫn setup chi tiết ở Section 5 bên dưới.**

---

## 5. Setup Claude Projects (khuyến nghị)

### Tạo 3 Projects theo vai trò

#### Project 1: CEO Project

**Tên project:** `[Tên công ty] — CEO`

**Custom Instructions (dán vào ô "What would you like Claude to know?"):**
```
Tôi là CEO. Trong project này tôi dùng 3 skills sau:

SKILL 1 — Tóm tắt Chiến lược:
[paste toàn bộ nội dung file strategic-brief/SKILL.md]

SKILL 2 — Rà soát & Đồng bộ:
[paste toàn bộ nội dung file review-align/SKILL.md]

SKILL 3 — Tổng kết & Cải tiến:
[paste toàn bộ nội dung file retro-summary/SKILL.md]

Khi tôi đưa yêu cầu, hãy tự nhận diện skill phù hợp và apply đúng format.
```

**Upload files:** Không cần upload file thêm — skills đã có trong Custom Instructions

---

#### Project 2: BA Project

**Tên project:** `[Tên công ty] — BA`

**Custom Instructions:**
```
Tôi là Business Analyst. Trong project này tôi dùng 3 skills sau:

SKILL 1 — Tạo Tài liệu Yêu cầu Sản phẩm:
[paste toàn bộ nội dung file prd-generator/SKILL.md]

SKILL 2 — Rà soát & Đồng bộ:
[paste toàn bộ nội dung file review-align/SKILL.md]

SKILL 3 — Danh sách Kiểm thử:
[paste toàn bộ nội dung file qa-checklist/SKILL.md]

Khi tôi đưa yêu cầu hoặc upload tài liệu, hãy tự nhận diện skill phù hợp và apply đúng format.
```

---

#### Project 3: DEV Project

**Tên project:** `[Tên công ty] — DEV`

**Custom Instructions:**
```
Tôi là Developer/Tech Lead. Trong project này tôi dùng 3 skills sau:

SKILL 1 — Đặc tả Kỹ thuật:
[paste toàn bộ nội dung file tech-spec/SKILL.md]

SKILL 2 — Danh sách Kiểm thử:
[paste toàn bộ nội dung file qa-checklist/SKILL.md]

SKILL 3 — Tổng kết & Cải tiến:
[paste toàn bộ nội dung file retro-summary/SKILL.md]

Khi tôi đưa yêu cầu hoặc upload PRD, hãy tự nhận diện skill phù hợp và apply đúng format.
```

---

### Cách dùng sau khi setup Projects

1. Mở đúng Project theo vai trò
2. Bắt đầu hội thoại mới
3. Gõ yêu cầu hoặc paste tài liệu vào
4. Claude tự nhận skill và output đúng format
5. Không cần paste SKILL.md lại nữa

---

### Chia sẻ Project cho team

- Trên Claude Teams/Enterprise: invite thành viên vào Project
- Trên Claude Pro cá nhân: share Custom Instructions dưới dạng text để mỗi người tự setup Project riêng

---

## 6. Triển khai theo GSD

### Nguyên tắc: Đừng setup hết cùng lúc

> "Làm nhanh → Test thật → Đo kết quả → Cải tiến → Lặp lại"

---

### Tuần 1 — Thử nghiệm (Cách 1)

- [ ] Chọn 1 dự án đang chạy thực tế (không phải demo)
- [ ] CEO dùng `strategic-brief` để viết brief cho dự án đó
- [ ] BA nhận brief → dùng `prd-generator` viết PRD
- [ ] Đo: tiết kiệm được bao nhiêu thời gian so với trước?
- [ ] Ghi chú: output có thiếu gì không?

---

### Tuần 2–3 — Mở rộng

- [ ] Thêm `review-align` vào buổi review PRD
- [ ] DEV thử `tech-spec` với PRD từ BA
- [ ] Thu thập feedback từ cả 3 vai trò

---

### Tuần 4 — Chuẩn hóa (Cách 2)

- [ ] Setup Claude Projects cho từng vai trò
- [ ] Upload skills đã được chỉnh sửa dựa trên feedback tuần 1–3
- [ ] Rollout cho toàn team
- [ ] Đặt lịch retro đầu tiên (dùng `retro-summary`)

---

### Sau mỗi sprint/release

- [ ] Chạy `retro-summary`
- [ ] Cập nhật SKILL.md theo Skill Improvement Notes
- [ ] Upload SKILL.md mới vào Projects
- [ ] Bắt đầu chu kỳ tiếp theo với skill đã tốt hơn

---

### Tiêu chí đo thành công

| Metric | Baseline | Target sau 1 tháng |
|--------|----------|-------------------|
| Thời gian viết Brief | [X giờ] | Giảm 50% |
| Thời gian viết PRD | [X ngày] | Giảm 50% |
| Số lần BA hỏi lại CEO | [X lần/sprint] | Giảm 70% |
| Số lần DEV hỏi lại BA | [X lần/sprint] | Giảm 70% |
| Thời gian họp review PRD | [X giờ] | Giảm 50% |

---

## 7. FAQ

**Q: Skills có cần update không?**  
A: Có. Sau mỗi retro, `retro-summary` sẽ xuất Skill Improvement Notes — đó là danh sách cụ thể cần cập nhật. Thường mỗi sprint chỉ cần sửa 2–3 điểm nhỏ.

**Q: Nếu team không dùng Claude Pro/Teams thì sao?**  
A: Dùng Cách 1 (paste thủ công). Hiệu quả hơn 80% so với không có skills, chỉ chậm hơn Cách 2 ở chỗ phải paste lại mỗi lần.

**Q: Skills có thể dùng với AI khác (ChatGPT, Gemini) không?**  
A: Có. SKILL.md là plain text, hoạt động với bất kỳ LLM nào. Claude hiểu tốt nhất vì được thiết kế tương thích, nhưng 80% nội dung vẫn hoạt động với các model khác.

**Q: CEO không có thời gian học — có cần training không?**  
A: Không. CEO chỉ cần nói/gõ yêu cầu bình thường. Claude tự apply skill. Không cần biết skill là gì, không cần đọc SKILL.md.

**Q: BA đã có template PRD riêng — có cần thay không?**  
A: Không nhất thiết. Có thể tích hợp template hiện tại vào `prd-generator/SKILL.md` để Claude output theo format quen thuộc của team.

**Q: DEV dùng Jira/Linear — skills có tích hợp được không?**  
A: Skills xuất markdown có thể copy-paste vào Jira/Linear. Để tích hợp sâu hơn (tự tạo ticket), cần dùng Claude API — đây là bước nâng cao.

**Q: Nếu output của Claude không đúng ý thì làm sao?**  
A: Ghi lại điểm chưa đúng → vào SKILL.md tương ứng → thêm rule hoặc ví dụ vào phần "Quy tắc" → upload lại vào Project. Đây chính là vòng lặp cải tiến GSD.

---

## Tóm tắt nhanh

| Khi cần | Dùng skill | Ai dùng |
|---------|-----------|---------|
| Biến ý tưởng thành brief rõ ràng | `strategic-brief` | CEO |
| Viết PRD từ brief | `prd-generator` | BA |
| So sánh brief vs PRD trước khi review | `review-align` | CEO + BA |
| Chuyển PRD thành tech spec + tasks | `tech-spec` | DEV |
| Tạo checklist test trước khi ship | `qa-checklist` | BA + DEV |
| Tổng kết sprint và cải tiến | `retro-summary` | CEO + All |

---

*Tài liệu này được tạo như một phần của hệ thống GSD × Claude Skills*  
*Cập nhật lần cuối: tháng sau mỗi quarterly retro*  
*Liên hệ: [người phụ trách quy trình trong team]*
