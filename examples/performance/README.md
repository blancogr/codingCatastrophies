# Performance Horrors

Code that works perfectly — right up until anyone actually uses it.

---

## N+1 Queries

```python
# Python (Django ORM) — a classic
def get_order_summaries():
    orders = Order.objects.all()         # 1 query
    for order in orders:
        customer = order.customer.name   # 1 query per order
        print(f"{customer}: {order.total}")

# With 10,000 orders: 10,001 database queries.
# With select_related: 1 query.
```

**Why it's catastrophic:** Each loop iteration fires a separate database query. With a small dataset in development this feels instant. With production data it buries the database and times out. The fix — `select_related` or `prefetch_related` — takes about five seconds.

---

## Sorting Inside a Loop

```javascript
// JavaScript
function findTopItem(data) {
    for (let i = 0; i < data.length; i++) {
        data.sort((a, b) => b.value - a.value); // O(n log n) inside O(n)
        if (data[i].value > threshold) {
            return data[i];
        }
    }
}
```

**Why it's catastrophic:** The array is re-sorted on every iteration of the loop. The overall complexity is O(n² log n) for something that could be O(n log n) with a single sort before the loop, or O(n) with a linear scan.

---

## Concatenating Strings in a Loop

```java
// Java
String result = "";
for (String item : millionItemList) {
    result = result + item + ", "; // creates a new String object every iteration
}
```

**Why it's catastrophic:** Strings in Java are immutable. Each `+` creates a new `String` object, copying all previous content. Concatenating a million strings this way allocates roughly 500 billion bytes of temporary objects. Use `StringBuilder`.

---

## Loading Everything Into Memory

```python
# Python — processing a large log file
with open("server.log", "r") as f:
    lines = f.readlines()  # reads the entire 40GB file into RAM

for line in lines:
    process(line)
```

**Why it's catastrophic:** `readlines()` loads the whole file into memory at once. A 40GB log file on a server with 16GB of RAM will cause an OOM kill. Iterating the file object directly streams one line at a time and uses constant memory.

---

## Polling Instead of Events

```javascript
// JavaScript — checking for new messages
setInterval(() => {
    fetch('/api/messages/new')
        .then(r => r.json())
        .then(msgs => displayMessages(msgs));
}, 100); // every 100ms, for every connected user
```

**Why it's catastrophic:** 1,000 connected users generate 10,000 HTTP requests per second to check for new messages, almost all of which return nothing. WebSockets or Server-Sent Events push data only when it exists, reducing server load by orders of magnitude.

---

## The Accidental O(n³)

```python
# Python — finding common elements between three lists
def find_common(a, b, c):
    result = []
    for x in a:
        for y in b:
            for z in c:
                if x == y == z:
                    result.append(x)
    return result

# With 1,000 elements per list: 1,000,000,000 comparisons.
# With sets: set(a) & set(b) & set(c) — O(n).
```

**Why it's catastrophic:** Triple-nested loops over large lists are a performance cliff. The set intersection version accomplishes the same result in linear time. This exact pattern shows up in interview problems for a reason.
