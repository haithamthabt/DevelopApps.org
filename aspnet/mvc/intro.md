---
layout: section
title: Introduction to MVC
nav_order: 1
parent: MVC
---

# Introduction to MVC

Model-View-Controller (MVC) is a software architectural pattern that separates an application into three main logical components:

## Model
- Represents the data and business logic
- Handles data validation
- Manages the application state

```csharp
public class Product
{
    public int Id { get; set; }
    [Required]
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Description { get; set; }
}
```

## View
- Presents data to users
- Handles the UI layout
- Uses Razor syntax

```cshtml
@model Product

<h1>@Model.Name</h1>
<div class="product-details">
    <p>Price: $@Model.Price</p>
    <p>@Model.Description</p>
</div>
```

## Controller
- Processes incoming requests
- Handles user input
- Works with the Model

```csharp
public class ProductController : Controller
{
    private readonly IProductService _service;

    public ProductController(IProductService service)
    {
        _service = service;
    }

    public IActionResult Details(int id)
    {
        var product = _service.GetProduct(id);
        return View(product);
    }
}
```

## Request Flow
1. User makes a request to `/Product/Details/1`
2. Routing system directs to `ProductController`
3. `Details` action method is called
4. Model data is retrieved
5. View is rendered with model data
6. Response is sent to user
