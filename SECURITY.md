# Security Policy

## Reporting a Vulnerability

Do not open a public issue for security vulnerabilities.

Email [povvo.dev@gmail.com](mailto:povvo.dev@gmail.com) directly.

Please include:

- A description of the vulnerability
- Steps to reproduce
- Potential impact
- Any suggested fixes

You will receive an initial response within 72 hours. If the vulnerability is confirmed, remediation will be prioritised according to severity and affected scope. Credit will be given unless you prefer otherwise.

---

## What Counts as a Security Issue

A finding is a security issue if it creates a realistic path to one of the following outcomes:

| Category | Examples |
|----------|----------|
| **Injection** | SQL injection, command injection, template injection, path traversal |
| **Authentication bypass** | Missing or bypassable auth checks, insecure token validation |
| **Exposed secrets** | Hardcoded API keys, passwords, or cryptographic material in source or logs |
| **Unvalidated input to dangerous sinks** | User-controlled data reaching a DB query, file system, `eval()`, or external command without sanitisation |
| **Sensitive data exposure** | PII, credentials, or session tokens in logs, error responses, or version control |
| **Privilege escalation** | A lower-privileged user obtaining access to resources or operations above their role |

---

## Attack Surface: Entry Points and Sinks

Every security review traces data flow from entry points to sinks.

**Entry points (sources):**
- Browser input: forms, query parameters, headers, cookies
- File uploads
- WebSocket messages
- External API feeds
- Environment variables, if user-controlled

**Dangerous sinks:**
- Database queries
- File system operations
- HTML or template output
- Shell command execution
- Log files
- External API calls

**Tracing process:**

```
1. Identify all entry points touched in the diff
2. For each entry point, trace the data flow:
      Entry → Processing → Sink
3. At each step, verify:
   - Is the data validated before use?
   - Is the data sanitised for the specific sink type?
   - Can the data escape its intended context?
```

**Example trace (vulnerable):**

```
Entry:      req.query.search  (line 45)
Processing: none — direct use
Sink:       db.query(`SELECT * FROM items WHERE name LIKE '%${search}%'`)
Finding:    SQL injection — no parameterisation
Confidence: 95
```

---

## Per-PR Security Checklist

Every pull request touching application logic should satisfy:

- [ ] User input validated before use
- [ ] SQL queries use parameterised statements, with no string concatenation of user data
- [ ] File paths sanitised; no path traversal possible
- [ ] No `eval()` or equivalent called with user-controlled data
- [ ] Secrets not logged, returned in error responses, or hardcoded
- [ ] Authentication and authorisation checked on every protected route or operation
- [ ] No `.env` files or configuration containing credentials in the diff
- [ ] New dependencies do not introduce known CVEs

**Secrets patterns to flag:**

| Pattern | Example |
|---------|---------|
| Hardcoded credential | `password = "hunter2"`, `apiKey: "sk-..."` |
| Debug credentials | `admin/admin`, `test123` in source |
| Committed env file | `.env` appearing in diff; it must be ignored |
| Config with embedded secrets | `config.json` containing credentials |
