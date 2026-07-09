# Naming Catastrophies

Names so bad they loop around to being art.

---

## The Single-Letter Everything

```python
# Python
def f(a, b, c, d, e):
    x = a + b
    y = x * c
    z = y / d
    return z + e
```

**Why it's bad:** Six months later, nobody — including the original author — knows what `a`, `b`, `c`, `d`, or `e` are. Is `f` a function? A variable? A cry for help?

---

## The Misleading Boolean

```javascript
// JavaScript
let isNotDisabled = false;

if (!isNotDisabled) {
    enableTheButton(); // so... the button IS disabled?
}
```

**Why it's bad:** Double negatives in boolean names cause the brain to melt. `isNotDisabled === false` means it IS disabled, but you have to parse three layers of logic to get there.

---

## Hungarian Notation Gone Wrong

```c
// C
int iIntegerNumberCount = 0;
char* szStringCharacterPointerName = "hello";
bool bBooleanFlagIsTrue = true;
```

**Why it's bad:** The type is already right there in the declaration. You've named the variable and the type twice, and the name tells you nothing about what it *represents*.

---

## The Thesaurus Attack

```java
// Java
public class DataInformationEntityObjectRecord {
    private String textStringCharacterSequenceLabel;
    private int numberIntegerQuantityCount;
}
```

**Why it's bad:** Every synonym for a concept crammed into a single identifier. This class was clearly written by someone who discovered a thesaurus and lost all self-control.

---

## temp, temp2, temp_final, temp_final_v2_REAL

```python
# Python — actual progression found in a production codebase
result = calculate()
temp = result
temp2 = process(temp)
temp_final = clean(temp2)
temp_final_v2 = fix(temp_final)       # "fixed a bug"
temp_final_v2_REAL = temp_final_v2    # this is the one we actually use
output = temp_final_v2_REAL
```

**Why it's bad:** Every intermediate variable should have been replaced as the code evolved. Instead, the history of a developer's indecision is preserved for eternity.

---

## The Opposite Name

```javascript
// JavaScript — spotted in a financial application
function getUserAge() {
    return db.query("SELECT name FROM users"); // returns names, not ages
}
```

**Why it's bad:** The function name promises one thing, the implementation delivers the opposite. This is how bugs live for years.
