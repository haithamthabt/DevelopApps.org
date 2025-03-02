---
layout: section
title: Introduction to Web API
nav_order: 1
parent: Web API
---

# Introduction to Web API

ASP.NET Web API is a framework for building HTTP services that can be accessed by a variety of clients.

## Basic Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _service;

    public ProductsController(IProductService service)
    {
        _service = service;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
    {
        var products = await _service.GetProductsAsync();
        return Ok(products);
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<Product>> GetProduct(int id)
    {
        var product = await _service.GetProductAsync(id);
        
        if (product == null)
        {
            return NotFound();
        }

        return product;
    }
}
```

## Key Concepts

1. **HTTP Methods**
   - GET - Retrieve data
   - POST - Create data
   - PUT - Update data
   - DELETE - Remove data

2. **Route Attributes**
   - `[Route]` - Define URL patterns
   - `[HttpGet]` - Handle GET requests
   - `[FromBody]` - Read from request body

3. **Response Types**
   - `Ok()` - 200 Success
   - `NotFound()` - 404 Not Found
   - `BadRequest()` - 400 Bad Request
