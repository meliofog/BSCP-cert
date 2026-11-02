# SQL Injection (SQLi)

## What

User input is inserted into a SQL query as code instead of data, so the attacker can change what the query does.

**Impact:** read other users' data, modify or delete data, sometimes escalate to server compromise or denial of service.

## Why it exists

The app builds queries by concatenating strings. The database can't tell the developer's SQL from the attacker's input, so it runs both.

## How to detect

Test every entry point: URL parameters, POST fields, cookies, headers, JSON values.

| Test | Example | Look for |
|---|---|---|
| Single quote | `'` | SQL errors or odd behavior |
| Boolean logic | `' OR 1=1--` vs `' OR 1=2--` | Different responses for true and false |
| Time delay | DB-specific sleep payload | Slower response |
| OAST | Payload that triggers an outbound request | Interaction in Burp Collaborator |

## Where it can occur

- `WHERE` clause of a `SELECT` (most common)
- `UPDATE` values or `WHERE`, `INSERT` values
- Table or column names, `ORDER BY`

## Core examples

**Retrieving hidden data**

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

- `Gifts'--` comments out `AND released = 1`, so unreleased products show.
- `Gifts'+OR+1=1--` makes the condition always true, so every product shows.

**Subverting application logic (login bypass)**

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

Username `administrator'--` with an empty password comments out the password check and logs in as that user.

> MySQL needs a space after `--` (or use `#`). Check the DB type before choosing the comment syntax.

## Techniques (details live in the lab write-ups)

| Technique | When | Idea |
|---|---|---|
| UNION attack | Query results appear in the response | Append `UNION SELECT` with the same number of columns and compatible types to pull data from other tables |
| Database fingerprinting | Before extracting data | Version queries and comment syntax differ per DB |
| Blind (boolean) | No output or errors visible | Infer data from differences in the response |
| Error-based | Errors are visible, or can be triggered | Force an error that leaks data, or one that depends on a condition |
| Time-based | No visible difference at all | Make the DB delay only if a condition is true |
| OAST | Everything else fails, or the query runs asynchronously | Trigger a DNS/HTTP request to Collaborator, optionally carrying data |
| Second-order | Input is stored, then used in a query later | Payload fires on a different request than the one that sent it |

## How to fix it

- Use **parameterized queries / prepared statements** for all values.
- For table names, column names and `ORDER BY`, use an **allowlist**.
- Run the app's DB account with **least privilege**.
- Don't rely on escaping or blocklists alone.

## How to prevent it systematically

- **Code review:** flag string concatenation inside queries.
- **SAST:** Semgrep or CodeQL rules for SQL built from user input.
- **DAST:** Burp Scanner or ZAP on every endpoint.
- **Runtime:** alert on `'`, `UNION`, `SLEEP` and `OR 1=1` patterns, and on spikes of 500 errors.

## Labs
Write-ups in `labs/sqli/`.

- [SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](../labs/sqli/01-where-clause-hidden-data.md)
