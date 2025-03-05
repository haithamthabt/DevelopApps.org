---
layout: section
title: Configuration & DI
parent: Entity Framework
grand_parent: ASP.NET
nav_order: 2
---

# Entity Framework Configuration and Dependency Injection

## Why Configure EF Core in Program.cs?

Entity Framework Core uses the dependency injection (DI) pattern for several important reasons:

1. **Lifetime Management**
   ```csharp
   builder.Services.AddDbContext<ShopContext>(options =>
       options.UseSqlServer(configuration.GetConnectionString("DefaultConnection")));
   ```
   - Automatically manages DbContext lifetime
   - Ensures proper disposal of resources
   - Prevents memory leaks
   - Creates new instance per HTTP request

2. **Configuration Management**
   ```csharp
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=.;Database=ShopDB;..."
     },
     "EntityFramework": {
       "CommandTimeout": 30,
       "EnableSensitiveDataLogging": false
     }
   }
   ```
   - Centralizes database configuration
   - Supports different environments (Development/Staging/Production)
   - Secures sensitive information
   - Makes deployment easier

3. **Testing and Maintainability**
   ```csharp
   // Easy to replace with mock for testing
   services.AddDbContext<ShopContext>(options =>
       options.UseInMemoryDatabase("TestDb"));
   ```
   - Facilitates unit testing
   - Enables swapping implementations
   - Supports different database providers
   - Makes code more modular

## Configuration Options

### Basic Configuration
```csharp
builder.Services.AddDbContext<ShopContext>(options =>
{
    options.UseSqlServer(connectionString, sqlOptions =>
    {
        sqlOptions.EnableRetryOnFailure(
            maxRetryCount: 3,
            maxRetryDelay: TimeSpan.FromSeconds(5),
            errorNumbersToAdd: null);
    });
});
```

### Advanced Options
```csharp
builder.Services.AddDbContext<ShopContext>(options =>
{
    options
        .UseSqlServer(connectionString)
        .EnableSensitiveDataLogging()
        .EnableDetailedErrors()
        .LogTo(Console.WriteLine);
});
```

## Dependency Injection Patterns

### Constructor Injection
```csharp
public class ProductService
{
    private readonly ShopContext _context;

    // DbContext is automatically injected
    public ProductService(ShopContext context)
    {
        _context = context;
    }

    public async Task<List<Product>> GetProductsAsync()
    {
        return await _context.Products.ToListAsync();
    }
}
```

### Scoped vs Singleton Services
```csharp
// ❌ Bad: Don't inject DbContext into Singleton services
public class SingletonService
{
    private readonly ShopContext _context; // This can cause issues!
    
    public SingletonService(ShopContext context)
    {
        _context = context;
    }
}

// ✅ Good: Use scoped services with DbContext
public class ScopedService
{
    private readonly ShopContext _context;
    
    public ScopedService(ShopContext context)
    {
        _context = context;
    }
}
```

## Environment-Specific Configuration

### Development
```csharp
if (env.IsDevelopment())
{
    options
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging();
}
```

### Production
```csharp
if (env.IsProduction())
{
    options
        .EnableRetryOnFailure()
        .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
}
```

## Best Practices

1. **Connection String Management**
   - Store in configuration files
   - Use secrets management in development
   - Use environment variables in production
   - Never commit connection strings to source control

2. **Performance Optimization**
   ```csharp
   options.UseSqlServer(connectionString, sqlOptions =>
   {
       sqlOptions.CommandTimeout(30);
       sqlOptions.EnableRetryOnFailure();
       sqlOptions.MigrationsHistoryTable("__EFMigrationsHistory");
   });
   ```

3. **Error Handling**
   ```csharp
   try
   {
       await _context.SaveChangesAsync();
   }
   catch (DbUpdateException ex)
   {
       // Log the error
       _logger.LogError(ex, "Error saving changes");
       throw;
   }
   ```

4. **Logging Configuration**
   ```csharp
   options.LogTo(
       filter: (eventId, level) => level >= LogLevel.Warning,
       logger: (eventData) =>
       {
           // Custom logging logic
       });
   ```

## Common Issues and Solutions

1. **Disposing Issues**
   - Let DI container manage DbContext lifecycle
   - Don't store DbContext instances
   - Don't create DbContext manually

2. **Connection Resiliency**
   - Enable retry on failure
   - Use appropriate timeouts
   - Handle transient failures

3. **Memory Leaks**
   - Use async/await properly
   - Dispose of heavy resources
   - Don't track unnecessary entities
