---
layout: section
title: Introduction to Blazor
nav_order: 1
parent: Blazor
---

# Introduction to Blazor

Blazor lets you build interactive web UIs using C# instead of JavaScript.

## Component Structure

A Blazor component is a self-contained piece of UI:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

## Key Concepts

1. **Components**
   - Reusable UI pieces
   - Mix HTML and C# code
   - Handle events and state

2. **Data Binding**
   - One-way binding (`@value`)
   - Two-way binding (`@bind`)
   - Event binding (`@onclick`)

3. **Lifecycle Methods**
   - `OnInitialized`
   - `OnParametersSet`
   - `OnAfterRender`

## Hosting Models

1. **Blazor Server**
   ```csharp
   builder.Services.AddServerSideBlazor();
   ```

2. **Blazor WebAssembly**
   ```csharp
   builder.Services.AddBlazorWebAssembly();
   ```
