# BSCP Notes

Personal notes, lab write-ups, and cheat sheets from my preparation for the **Burp Suite Certified Practitioner (BSCP)** exam, using the PortSwigger Web Security Academy.

> Goal: understand *why* each vulnerability exists, not just how to exploit it. Every lab ends with a short write-up in my own words.

---

## Why this repo exists

- Turn lab practice into connected knowledge: bug, root cause, detection, fix, prevention.
- Build a searchable cheat sheet I can use during the exam.
- Show my learning process publicly as part of my AppSec portfolio.

## Repo structure

```
bscp-notes/
├── README.md              
├── LEARNINGS.md           # one-line lessons from mistakes and surprises
├── templates/
│   └── lab-template.md
├── notes/                 # one file per vulnerability class
│   ├── sql-injection.md
│   ├── xss.md
│   └── ...
├── labs/                  # individual lab write-ups
│   └── sqli/
│       └── 01-lab-temp.md
├── cheatsheets/           # quick-reference payloads and Burp workflows
└── exam-prep/             # stage strategy, timing, mock exam logs
```

## Note template

Every vulnerability note and lab write-up follows the same structure:

| Section | Question it answers |
|---|---|
| **What** | What is the bug, in one or two sentences? |
| **Why it exists** | What does the code or design do wrong? (If I can't answer this, I haven't understood it yet.) |
| **How to find it** | Where do I look, and which Burp feature or request pattern reveals it? |
| **How to exploit it** | Steps and payloads that worked. |
| **How to fix it** | The secure code or configuration. |
| **How to prevent it systematically** | SAST rule, framework control, WAF, code review check, pipeline step. |
| **Mistakes / dead ends** | What I tried that failed and why. |

The ready-to-copy version is in [`templates/lab-template.md`](templates/lab-template.md).

## Progress tracker

Legend: ⬜ not started | 🟨 in progress | ✅ done (labs solved without hints)

### Server-side

| Topic | Apprentice | Practitioner | Expert | Notes |
|---|---|---|---|---|
| SQL injection | 🟨 | ⬜ | ⬜ | |
| Authentication | ⬜ | ⬜ | ⬜ | |
| Path traversal | ⬜ | ⬜ | ⬜ | |
| OS command injection | ⬜ | ⬜ | ⬜ | |
| Business logic vulnerabilities | ⬜ | ⬜ | ⬜ | |
| Information disclosure | ⬜ | ⬜ | ⬜ | |
| Access control and IDOR | ⬜ | ⬜ | ⬜ | |
| File upload vulnerabilities | ⬜ | ⬜ | ⬜ | |
| Race conditions | ⬜ | ⬜ | ⬜ | |
| SSRF | ⬜ | ⬜ | ⬜ | |
| XXE injection | ⬜ | ⬜ | ⬜ | |
| NoSQL injection | ⬜ | ⬜ | ⬜ | |
| API testing | ⬜ | ⬜ | ⬜ | |
| Web cache deception | ⬜ | ⬜ | ⬜ | |

### Client-side

| Topic | Apprentice | Practitioner | Expert | Notes |
|---|---|---|---|---|
| Cross-site scripting (XSS) | 🟨 | ⬜ | ⬜ | |
| CSRF | ⬜ | ⬜ | ⬜ | |
| CORS | ⬜ | ⬜ | ⬜ | |
| Clickjacking | ⬜ | ⬜ | ⬜ | |
| DOM-based vulnerabilities | ⬜ | ⬜ | ⬜ | |
| WebSockets | ⬜ | ⬜ | ⬜ | |

### Advanced topics

| Topic | Apprentice | Practitioner | Expert | Notes |
|---|---|---|---|---|
| JWT attacks | ⬜ | ⬜ | ⬜ | |
| OAuth authentication | ⬜ | ⬜ | ⬜ | |
| HTTP Host header attacks | ⬜ | ⬜ | ⬜ | |
| HTTP request smuggling | ⬜ | ⬜ | ⬜ | |
| Web cache poisoning | ⬜ | ⬜ | ⬜ | |
| Server-side template injection | ⬜ | ⬜ | ⬜ | |
| Insecure deserialization | ⬜ | ⬜ | ⬜ | |
| Prototype pollution | ⬜ | ⬜ | ⬜ | |
| GraphQL API vulnerabilities | ⬜ | ⬜ | ⬜ | |
| Web LLM attacks | ⬜ | ⬜ | ⬜ | |

## Study workflow

1. Read the Academy topic page first, without touching the labs.
2. Do the Apprentice labs, then Practitioner labs. Use hints only after a real attempt.
3. Write the lab note right after solving, while it's fresh.
4. Add reusable payloads and Burp steps to `cheatsheets/`.
5. Add one line to `LEARNINGS.md` whenever something surprises me.
6. Once a week, redo one old lab without notes.

## Exam prep

The BSCP exam is a timed, hands-on test where I have to compromise web applications using Burp Suite. See PortSwigger's official exam page for the current format, rules, and price.

- Exam strategy and timing notes: [`exam-prep/`](exam-prep/)
- Rule for booking: I'll book when I can solve Practitioner-level labs without hints.
- Mock exams: logged in `exam-prep/mock-exams.md` (date, result, time per stage, what blocked me).

## Resources

- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [Burp Suite Certified Practitioner exam](https://portswigger.net/web-security/certification)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/)

## Disclaimer

All testing is done on PortSwigger's labs and other intentionally vulnerable applications that I'm authorized to attack. Notes are written in my own words and don't reproduce the Academy's lab solutions in full.

## About

Maintained by **me**, M.Sc. Cybersecurity student at UMCS, working toward AppSec and DevSecOps roles.
