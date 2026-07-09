# The God Object

One class to rule them all. One class to find them. One class to bring them all, and in the darkness bind them.

---

## The Application Class

```python
# Python — a real pattern found in legacy codebases
class Application:
    def run(self): ...
    def connect_to_database(self): ...
    def send_email(self): ...
    def parse_csv(self): ...
    def generate_pdf(self): ...
    def calculate_tax(self): ...
    def validate_phone_number(self): ...
    def resize_image(self): ...
    def compress_file(self): ...
    def authenticate_user(self): ...
    def log_to_file(self): ...
    def format_currency(self): ...
    def translate_text(self): ...
    def backup_database(self): ...
    def send_sms(self): ...
    def render_html(self): ...
    def schedule_job(self): ...
    def process_payment(self): ...
    # ... 200 more methods
```

**Why it's bad:** This class knows about everything and is responsible for everything. It cannot be tested in isolation, it cannot be extended safely, and every change to any feature in the system touches this one file. Merge conflicts are a daily ritual.

---

## The Utils File

```javascript
// JavaScript — utils.js in every project ever
export function formatDate() { ... }
export function validateEmail() { ... }
export function hashPassword() { ... }
export function generateUUID() { ... }
export function renderChart() { ... }
export function connectDatabase() { ... }
export function parseCsv() { ... }
export function sendWebhook() { ... }
export function resizeImage() { ... }
// utils.js: 4,200 lines and growing
```

**Why it's bad:** `utils.js` is where code goes when nobody wants to think about where it actually belongs. It starts innocently with a date formatter and ends up being an undocumented standard library that everything depends on and nobody dares touch.

---

## The 10,000-Line Method

```java
// Java — encountered in a payment processing system
public void processOrder(Order order) {
    // Line 1: validate customer
    // Line 47: check inventory
    // Line 203: apply discount
    // Line 891: calculate tax
    // Line 1204: charge credit card
    // Line 1892: send confirmation email
    // Line 2001: update analytics
    // ...
    // Line 9,847: done
}
```

**Why it's bad:** A 10,000-line method has a cyclomatic complexity in the hundreds. It is untestable, unreadable, and unmaintainable. When it breaks, and it will break, finding the problem requires reading a short novel of procedural code.

---

## Global State Everywhere

```python
# Python
current_user = None
current_request = None
current_session = None
database_connection = None
config = None
cache = None
logger = None

def do_thing():
    global current_user, current_request, current_session
    global database_connection, config, cache, logger
    # Now we can begin
    ...
```

**Why it's bad:** Global state makes the execution order of code matter unpredictably, makes testing nearly impossible (state leaks between tests), and makes the function's dependencies invisible to its callers.
