---
layout: default
title: C# Variables and Types
nav_order: 2
parent: Tutorials
grand_parent: C# Tutorials
nav_order: 2
---

# C# Variables and Types

C# is a strongly-typed language, which means every variable has a specific type.

## Built-in Types

```csharp
// Numeric types
int age = 25;
double price = 19.99;
decimal money = 100.50m;

// Text
string name = "John";
char grade = 'A';

// Boolean
bool isActive = true;

// Date and Time
DateTime now = DateTime.Now;
```

## Type Conversion

### Implicit Conversion
```csharp
int num = 100;
long bigNum = num; // Automatically converts int to long
```

### Explicit Conversion (Casting)
```csharp
double pi = 3.14;
int rounded = (int)pi; // Explicitly convert double to int
```

## var Keyword

C# can infer types using var:

```csharp
var message = "Hello"; // Type is string
var number = 42;       // Type is int
var today = DateTime.Now; // Type is DateTime
```

## Best Practices

1. Choose appropriate types
2. Use meaningful names
3. Initialize variables
4. Consider nullable types
5. Use constants when values won't change
