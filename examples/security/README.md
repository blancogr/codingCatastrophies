# Security Nightmares

These examples demonstrate dangerous patterns. **Do not replicate these in real systems.**

---

## SQL Injection: The Classic

```php
// PHP — do not do this
$username = $_POST['username'];
$password = $_POST['password'];

$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";
$result = mysqli_query($conn, $query);
```

**Why it's catastrophic:** An attacker entering `' OR '1'='1` as the username bypasses authentication entirely. This single pattern has caused more data breaches than any other vulnerability. Always use parameterized queries or prepared statements.

**Safe version:**
```php
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $username, $password);
```

---

## Passwords Stored in Plain Text

```python
# Python — please, no
def create_user(username, password):
    db.execute(
        "INSERT INTO users (username, password) VALUES (?, ?)",
        (username, password)  # stored exactly as typed
    )
```

**Why it's catastrophic:** When (not if) the database is breached, every user's password is exposed. Since most people reuse passwords, this becomes a credential for every other service they use. Always hash with `bcrypt`, `argon2`, or `scrypt`.

---

## Hardcoded Credentials in Source Code

```javascript
// JavaScript — committed to a public GitHub repo
const db = mysql.createConnection({
    host: 'prod-db.company.internal',
    user: 'root',
    password: 'Company@2019!',   // the production root password
    database: 'customers'
});
```

**Why it's catastrophic:** Source code is frequently public, shared, or leaked. Credentials in code get indexed by GitHub search, copied into Slack messages, and emailed in diffs. Use environment variables or secrets managers.

---

## eval() on User Input

```javascript
// JavaScript
const userFormula = req.query.formula; // e.g. "2 + 2"
const result = eval(userFormula);      // e.g. "require('child_process').exec('rm -rf /')"
res.json({ result });
```

**Why it's catastrophic:** `eval()` executes arbitrary JavaScript. A user supplying a carefully crafted string can run any code on the server. There is almost no legitimate use case for `eval()` on untrusted input.

---

## The "Security Through Obscurity" API

```python
# Python — an "authentication" system
def get_admin_data(secret_param):
    if secret_param == "letmein":   # nobody will guess this
        return db.get_all_user_data()
    return None
```

**Why it's catastrophic:** Security through obscurity is not security. The secret string lives in the source code (often public), in logs, in browser history, and in network traces. One leak and every user's data is exposed.

---

## Path Traversal

```python
# Python — a file download endpoint
def download_file(filename):
    filepath = f"/app/uploads/{filename}"
    with open(filepath, 'rb') as f:
        return f.read()

# An attacker requests: filename = "../../etc/passwd"
# The server happily returns the system password file.
```

**Why it's catastrophic:** Without sanitizing the filename, an attacker can traverse the filesystem and read any file the server process has access to. Always validate and sanitize file paths, and use `os.path.realpath` + prefix checking.
