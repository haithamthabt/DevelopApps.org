---
layout: default
title: C# Classes and Objects
nav_order: 3
parent: Tutorials
grand_parent: C# Tutorials
---

# C# Classes and Objects

Classes are the foundation of object-oriented programming in C#.

## Creating a Class

```csharp
public class Person
{
    // Properties
    public string Name { get; set; }
    public int Age { get; set; }
    
    // Constructor
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
    
    // Method
    public void Introduce()
    {
        Console.WriteLine($"Hi, I'm {Name} and I'm {Age} years old.");
    }
}
```

## Using Classes

```csharp
// Create an object
Person person1 = new Person("Alice", 25);

// Access properties
Console.WriteLine(person1.Name); // Outputs: Alice

// Call methods
person1.Introduce(); // Outputs: Hi, I'm Alice and I'm 25 years old.
```

## Inheritance

```csharp
public class Student : Person
{
    public string StudentId { get; set; }
    
    public Student(string name, int age, string studentId) 
        : base(name, age)
    {
        StudentId = studentId;
    }
}
```

## Best Practices

1. Use proper encapsulation
2. Follow naming conventions
3. Keep classes focused
4. Document your code
5. Consider inheritance carefully
