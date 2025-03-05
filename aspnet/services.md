---
layout: section
title: What Are Services?
parent: ASP.NET
nav_order: 3
---

# What Are Services in ASP.NET?

## Understanding Services

Think of services like workers in a restaurant:
- The **Chef** cooks the food (Business Logic Service)
- The **Waiter** serves customers (Controller)
- The **Cashier** handles payments (Payment Service)
- The **Manager** coordinates everything (Program File)

In ASP.NET, services are classes that provide functionality to your application:
```csharp
// Example service that handles orders
public interface IOrderService
{
    Task<Order> CreateOrder(OrderRequest request);
    Task<Order> GetOrder(int orderId);
}

public class OrderService : IOrderService
{
    private readonly ApplicationDbContext _db;
    
    // ASP.NET automatically provides (injects) the database context
    public OrderService(ApplicationDbContext db)
    {
        _db = db;
    }

    public async Task<Order> CreateOrder(OrderRequest request)
    {
        // Business logic for creating orders
    }
}
```

## Why Do We Need Services?

### 1. Separation of Concerns
```csharp
// ❌ Bad: Everything in the controller
public class OrderController
{
    public IActionResult CreateOrder()
    {
        // Database logic here
        // Email notification logic here
        // Payment processing here
        // Everything mixed together!
    }
}

// ✅ Good: Using services
public class OrderController
{
    private readonly IOrderService _orderService;
    private readonly IEmailService _emailService;
    private readonly IPaymentService _paymentService;

    public OrderController(
        IOrderService orderService,
        IEmailService emailService,
        IPaymentService paymentService)
    {
        _orderService = orderService;
        _emailService = emailService;
        _paymentService = paymentService;
    }

    public async Task<IActionResult> CreateOrder(OrderRequest request)
    {
        // Each service handles its specific responsibility
        var order = await _orderService.CreateOrder(request);
        await _paymentService.ProcessPayment(order);
        await _emailService.SendOrderConfirmation(order);
        return Ok(order);
    }
}
```

### 2. Reusability
```csharp
// The same email service can be used anywhere in your app
public class EmailService : IEmailService
{
    public async Task SendOrderConfirmation(Order order)
    {
        // Email logic
    }

    public async Task SendPasswordReset(User user)
    {
        // Same email service, different purpose
    }
}
```

### 3. Testing
```csharp
// Easy to test with mock services
[Fact]
public async Task CreateOrder_ValidRequest_ReturnsOrder()
{
    // Arrange
    var mockOrderService = new Mock<IOrderService>();
    mockOrderService.Setup(s => s.CreateOrder(It.IsAny<OrderRequest>()))
        .ReturnsAsync(new Order { Id = 1 });

    var controller = new OrderController(mockOrderService.Object);

    // Act
    var result = await controller.CreateOrder(new OrderRequest());

    // Assert
    Assert.NotNull(result);
}
```

## Types of Services

### 1. Transient Services
```csharp
// Created every time they're requested
builder.Services.AddTransient<IEmailService, EmailService>();

// Good for:
// - Lightweight, stateless services
// - Services that don't share data
```

### 2. Scoped Services
```csharp
// Created once per request
builder.Services.AddScoped<IOrderService, OrderService>();

// Good for:
// - Database contexts
// - Services that need to maintain state during a request
```

### 3. Singleton Services
```csharp
// Created once for the entire application
builder.Services.AddSingleton<ICacheService, CacheService>();

// Good for:
// - Caching
// - Configuration
// - Services that share state across requests
```

## Common Service Examples

### 1. Data Access Services
```csharp
public interface IProductRepository
{
    Task<List<Product>> GetAllProducts();
    Task<Product> GetProductById(int id);
}
```

### 2. Business Logic Services
```csharp
public interface IDiscountService
{
    decimal CalculateDiscount(Order order);
    bool IsEligibleForDiscount(Customer customer);
}
```

### 3. Infrastructure Services
```csharp
public interface IEmailService
{
    Task SendEmail(string to, string subject, string body);
}

public interface ILoggingService
{
    void LogError(Exception ex);
    void LogInfo(string message);
}
```

## Best Practices

### 1. Use Interfaces
```csharp
// ✅ Good: Define interface first
public interface IOrderProcessor
{
    Task ProcessOrder(Order order);
}

public class OrderProcessor : IOrderProcessor
{
    public async Task ProcessOrder(Order order)
    {
        // Implementation
    }
}
```

### 2. Single Responsibility
```csharp
// ❌ Bad: Service doing too much
public class SuperService
{
    public void SendEmail() { }
    public void ProcessPayment() { }
    public void UpdateDatabase() { }
}

// ✅ Good: Separate services for separate concerns
public class EmailService { }
public class PaymentService { }
public class DataService { }
```

### 3. Dependency Injection
```csharp
// ❌ Bad: Creating services manually
public class OrderController
{
    private readonly OrderService _orderService = new OrderService();
}

// ✅ Good: Using dependency injection
public class OrderController
{
    private readonly IOrderService _orderService;
    
    public OrderController(IOrderService orderService)
    {
        _orderService = orderService;
    }
}
```

## Common Questions

### "When Should I Create a Service?"
Create a service when you have:
- Reusable functionality
- Complex business logic
- External system interactions
- Need for testing isolation

### "How Many Services Is Too Many?"
Follow these guidelines:
- Each service should have a single responsibility
- Don't create services for simple CRUD operations
- Combine related operations in one service
- Split service when it becomes too complex

### "Which Service Lifetime Should I Choose?"
- **Transient**: For lightweight, stateless services
- **Scoped**: For most business logic and data access
- **Singleton**: For shared resources and caching
