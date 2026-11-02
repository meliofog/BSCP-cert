\# \[Lab title]



| | |

|---|---|

| \*\*Topic\*\* | e.g. SQL injection |

| \*\*Level\*\* | Apprentice / Practitioner / Expert |

| \*\*Lab link\*\* | https://portswigger.net/web-security/... |

| \*\*Date solved\*\* | YYYY-MM-DD |

| \*\*Time taken\*\* | e.g. 25 min |

| \*\*Hints used?\*\* | No / Yes (which) |

| \*\*Status\*\* | ✅ Solved / 🟨 Partial / ⬜ Revisit |



\## What



One or two sentences: what is the vulnerability in this lab?



\## Why it exists



What does the code or design do wrong? Name the root cause (for example: user input concatenated into a query, missing server-side check, trust in a client-controlled value).



> If I can't fill this section in, I haven't understood the bug yet. Go back before moving on.



\## How to find it



\- Where in the app did I notice it? (parameter, header, cookie, feature)

\- Which Burp tool or request pattern revealed it? (Proxy, Repeater, Intruder, Scanner, Collaborator)

\- What was the first sign something was wrong?



\## How to exploit it



1\. Step one

2\. Step two

3\. Step three



\*\*Working payload / request:\*\*



```

paste the minimal payload or request here

```



\## How to fix it



The secure version, in code or configuration:



```

secure code or config snippet

```



\## How to prevent it systematically



\- \*\*Design / framework:\*\* e.g. parameterized queries by default, ORM, strict CSP

\- \*\*Code review check:\*\* what would I look for?

\- \*\*Automated detection:\*\* SAST rule, DAST check, pipeline step, WAF rule

\- \*\*Runtime detection:\*\* what would this attack look like in logs?



\## Mistakes and dead ends



\- What I tried that didn't work, and why it failed.



\## Exam relevance



Which exam stage could this technique help with? Does it chain with other bugs?



\## Related



\- Related labs: 

\- Related notes: \[`../../notes/topic.md`](../../notes/topic.md)

\- Cheat sheet entry added? Yes / No

