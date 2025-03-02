---
layout: section
title: Control Structures
nav_order: 3
parent: Programming Fundamentals
grand_parent: Programming Fundamentals
nav_order: 3
---

# Control Structures

Control structures are the building blocks that determine the flow of program execution.

## If Statements

Decision making in programming:

```python
if age >= 18:
    print("You are an adult")
elif age >= 13:
    print("You are a teenager")
else:
    print("You are a child")
```

## Loops

### For Loops
Used for iterating a specific number of times:

```python
for i in range(5):
    print(i)  # Prints 0 to 4
```

### While Loops
Continues until a condition is false:

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

## Switch/Case Statements

Making multiple choices:

```javascript
switch(day) {
    case "Monday":
        console.log("Start of week");
        break;
    case "Friday":
        console.log("Weekend coming!");
        break;
    default:
        console.log("Regular day");
}
```

## Best Practices

1. Keep conditions simple
2. Avoid deep nesting
3. Use meaningful variable names
4. Consider all possible cases
5. Add proper documentation
