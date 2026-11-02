# SQL Injection (SQLi)

## What

User input is inserted into a SQL query as code instead of data, so the attacker can change what the query does.

**Impact:** read other users' data, modify or delete data, sometimes escalate to server compromise or denial of service.

## Why it exists

The app builds queries by concatenating strings. The database can't tell the developer's SQL from the attacker's input, so it runs both.

## How to detect

Test every entry point: URL parameters, POST fields, cookies, headers, JSON values.

| Test | Example | What to look for |
|---|---|---|
| Single quote | `'` | SQL errors or odd behavior |
| Boolean logic | `' OR 1=1--` vs `' OR 1=2--` | Different responses between true and false |
| Same vs different value | Input that evaluates to the original value, then to another | Systematic response differences |
| Time delay | DB-specific sleep payload | Slower response |
| OAST | Payload that triggers an outbound request | Interaction in Burp Collaborator |

> Sleep and OAST payloads differ per database (Oracle, MySQL, PostgreSQL, MSSQL). Notes will be in `cheatsheets/`.

## Where it can occur

- `WHERE` clause of a `SELECT` (most common)
- `UPDATE`: values or `WHERE` clause
- `INSERT`: inserted values
- `SELECT`: table or column names
- `SELECT`: `ORDER BY` clause

## Example: retrieving hidden data

A shop filters products by category:

```
/products?category=Gifts
```

Backend query:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

`released = 1` hides unreleased products.

**Attack 1: comment out the rest of the query**

```
/products?category=Gifts'--
```

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

`--` starts a comment, so `AND released = 1` is ignored and unreleased products appear.

**Attack 2: make the condition always true**

```
/products?category=Gifts'+OR+1=1--
```

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

`1=1` is always true, so every product from every category is returned.

> MySQL needs a space after `--` (or use `#`). Check the DB type before choosing the comment syntax.

## How to fix it

- Use **parameterized queries / prepared statements** for all values.
- For table names, column names and `ORDER BY`, which can't be parameterized, use an **allowlist** of permitted values.
- Run the app's DB account with **least privilege**.
- Don't rely on escaping or blocklists alone.

## How to prevent it systematically

- **Code review:** flag any string concatenation or formatting inside a query.
- **SAST:** Semgrep or CodeQL rules for SQL built from user input.
- **DAST:** Burp Scanner or ZAP against every endpoint.
- **Runtime:** watch logs for quotes, `UNION`, `SLEEP`, and `OR 1=1` patterns.

## Labs

- [SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](../labs/sqli/01-where-clause-hidden-data.md)
