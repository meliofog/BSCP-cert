# SQL injection vulnerability allowing login bypass

| | |
|---|---|
| **Topic** | SQL injection |
| **Level** | Apprentice |
| **Lab link** | https://portswigger.net/web-security/sql-injection/lab-login-bypass |
| **Date solved** | 2026-11-02 |
| **Time taken** | 5 min |
| **Hints used?** | No |
| **Status** | ✅ Solved |

## What

The login form puts the username straight into a SQL query. Adding a comment sequence to the username removes the password check, so I can log in as `administrator` without knowing the password.

## Why it exists

The app checks credentials with a query built by string concatenation:

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```

Login succeeds if a row comes back. Because the input isn't parameterized, a quote in the username ends the string and `--` turns the rest of the query, including the password condition, into a comment.

## How to find it

- **Entry point:** the `username` field of the login request (POST).
- **Signal:** this is a login query, so the likely pattern is `WHERE username = '...' AND password = '...'`. Putting a quote and a comment in the username is the obvious first test.

## How to exploit it

1. Intercept the login request (Burp Proxy) or edit it in Repeater.
2. Set `username` to `administrator'--` and the password to anything or leave it blank.
3. Send it. The app logs me in as `administrator`.

**Payload:**

```
username=administrator'--
password=
```

**Resulting query:**

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

The password condition is commented out, so the query returns the administrator's row.

**Tip:** this also works without Burp, by editing the request in the browser's dev tools, but Burp keeps a history I can replay and compare.

## How to fix it

- Use a parameterized query to fetch the user by username only:

```java
PreparedStatement stmt = conn.prepareStatement(
    "SELECT password_hash FROM users WHERE username = ?");
stmt.setString(1, username);
```

- Compare the submitted password against the stored **hash** in application code (bcrypt, Argon2 or similar). Never compare plaintext passwords inside SQL.

## How to prevent it systematically

- **Code review:** any login code that concatenates input into a query is a critical finding.
- **SAST:** Semgrep or CodeQL rules for SQL built from request parameters.
- **Runtime:** alert on `'` and `--` in login requests, and on successful logins that follow a malformed username.

## Related

- Notes: [`../../notes/sql-injection.md`](../../notes/sql-injection.md)
