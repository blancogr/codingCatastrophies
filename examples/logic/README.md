# Logic Disasters

Code that almost does the right thing. *Almost.*

---

## The Infinite Loop Disguised as a Feature

```python
# Python
def wait_for_user():
    while True:
        response = input("Are you ready? (yes/no): ")
        if response == "yes":
            return True
        elif response == "no":
            return False
        # What if they type "Yes"? Or "YES"? Or press Enter?
        # They're trapped here forever.
```

**Why it's bad:** The loop has no escape for any input that isn't exactly `"yes"` or `"no"`. Caps lock users are simply locked out of the program.

---

## Off By One... In Every Direction

```javascript
// JavaScript — meant to process items 1 through 10
for (let i = 0; i <= 10; i++) {   // processes 0 through 10 (11 items!)
    processItem(items[i]);          // items[10] is undefined for a 10-element array
}
```

**Why it's bad:** Two off-by-one errors that partially cancel each other out, until the array has exactly 10 items and then it crashes.

---

## The Upside-Down Condition

```java
// Java — from a login system
public boolean authenticate(String password) {
    if (!isCorrectPassword(password)) {
        return true;  // logged in!
    }
    return false;  // wrong password, access denied
}
```

**Why it's bad:** The condition is perfectly inverted. Anyone with the *wrong* password gets in. This was in production for eight months.

---

## The Reassuring No-Op

```python
# Python
def delete_user(user_id):
    try:
        db.execute("DELETE FROM users WHERE id = ?", user_id)
    except Exception:
        pass  # it's fine
```

**Why it's bad:** If the delete fails — database is down, constraint violation, connection timeout — the caller is told nothing. The user is not deleted, but the code happily continues as if it was.

---

## Sorting by Stringified Numbers

```javascript
// JavaScript
const numbers = [1, 2, 10, 20, 100];
numbers.sort(); // [1, 10, 100, 2, 20]
```

**Why it's bad:** JavaScript's default `Array.sort()` converts elements to strings and sorts lexicographically. `"10"` comes before `"2"` because `"1"` < `"2"`. This bites every JavaScript beginner at least once.

---

## The Self-Defeating Guard Clause

```python
# Python
def process(data):
    if data is None:
        data = None  # handle the None case
    return transform(data)  # still passes None to transform
```

**Why it's bad:** The guard clause detects the problem and then does exactly nothing about it. `transform(None)` will crash just as before.

---

## Recursion Without a Base Case

```javascript
// JavaScript — computing factorial
function factorial(n) {
    return n * factorial(n - 1); // works great until it doesn't
}

factorial(5); // → Maximum call stack size exceeded
```

**Why it's bad:** Without `if (n <= 1) return 1`, this recurses until the call stack overflows. On some inputs it may take a while to crash, giving false confidence.
