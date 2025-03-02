---
layout: section
title: Introduction to Razor Pages
nav_order: 1
parent: Razor Pages
---

# Introduction to Razor Pages

Razor Pages is a page-based model for building web apps in ASP.NET Core.

## Page Structure

A Razor Page consists of two files:
1. `.cshtml` - The view template
2. `.cshtml.cs` - The page model (code-behind)

```csharp
// ContactModel.cshtml.cs
public class ContactModel : PageModel
{
    [BindProperty]
    public Contact Contact { get; set; }

    public async Task<IActionResult> OnPostAsync()
    {
        if (!ModelState.IsValid)
        {
            return Page();
        }

        // Save the contact...
        return RedirectToPage("./Success");
    }
}

// Contact.cshtml
@page
@model ContactModel
<h1>Contact Us</h1>
<form method="post">
    <div>
        <label asp-for="Contact.Name"></label>
        <input asp-for="Contact.Name" />
    </div>
    <div>
        <label asp-for="Contact.Email"></label>
        <input asp-for="Contact.Email" />
    </div>
    <button type="submit">Send</button>
</form>
```

## Key Concepts

1. **Page Model**
   - Handles page logic
   - Properties for view data
   - Event handlers for HTTP methods

2. **Handler Methods**
   - `OnGet()` - Handle GET requests
   - `OnPost()` - Handle POST requests
   - `OnPutAsync()` - Handle PUT requests

3. **Model Binding**
   - Automatic property binding
   - Form data mapping
   - Query string parameters
