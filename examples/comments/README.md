# Comment Catastrophies

Documentation that ranges from useless to actively misleading.

---

## The Obvious Comment

```python
# Python
x = x + 1  # increment x by 1

if user.is_logged_in():  # check if the user is logged in
    show_dashboard()     # show the dashboard
```

**Why it's bad:** Comments should explain *why*, not *what*. Reading `x = x + 1` already tells you `x` is incremented. Restating the code in English wastes space and creates twice the maintenance burden — now you have to keep the comment in sync with the code.

---

## The Outdated Lie

```java
// Java
/**
 * Returns the user's email address.
 */
public String getUsername() {  // was renamed from getEmail() 3 years ago
    return this.username;       // returns username, not email
}
```

**Why it's bad:** The comment describes what the function used to do. It now describes the opposite of what the function does. A developer trusting the comment will introduce a bug.

---

## TODO: Fix Later (2009 Edition)

```c
// C
// TODO: this is a temporary hack, fix before release
int buffer_size = 65536; // should be dynamic
```
*(Commit date: April 3rd, 2009. The product shipped in 2010.)*

**Why it's bad:** TODOs without an assignee, a ticket number, or any follow-up mechanism are just apologies left in the code. This one has survived four major versions and three teams.

---

## The Commented-Out Code Graveyard

```javascript
// JavaScript
function calculateTotal(items) {
    // let total = 0;
    // for (const item of items) {
    //     total += item.price;
    // }
    // return total;

    // v2 attempt:
    // return items.reduce((sum, i) => sum + i.price, 0);

    // tried this, didn't work:
    // return items.map(i => i.price).reduce((a, b) => a + b);

    return items.reduce((sum, item) => sum + item.price, 0); // this one works
}
```

**Why it's bad:** Version control exists precisely to preserve history without cluttering the code. Commented-out code is noise that other developers have to read and evaluate on every pass.

---

## The Passive-Aggressive Comment

```python
# Python — found in a financial calculation module
# I have no idea why this works but if you touch it everything breaks
result = (value * 1.07) / 0.93 + 0.001

# DO NOT CHANGE THE 0.001. I MEAN IT. I SPENT THREE DAYS ON THIS.
```

**Why it's bad:** This is a confession that the author didn't understand the domain well enough to write an explainable implementation. The magic constants and the warning comment are a time bomb waiting for the business rules to change.

---

## The Sarcastic Comment That Made It to Production

```javascript
// JavaScript
function getUserData(id) {
    // this function is called 10,000 times per second and makes a database
    // call each time. i told management. they said "it works fine."
    return db.query(`SELECT * FROM users WHERE id = ${id}`);
}
```

**Why it's bad:** The comment correctly identifies a catastrophic performance problem and a SQL injection vulnerability. Both were ignored. The sarcasm was preserved for posterity.
