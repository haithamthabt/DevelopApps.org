---
layout: section
title: Best Practices
parent: Entity Framework
grand_parent: ASP.NET
nav_order: 3
---

# Entity Framework Best Practices

## 1. Database Design and Modeling

### Use Meaningful Names
```csharp
// ❌ Bad
public class Tbl { }
public string Nm { get; set; }

// ✅ Good
public class Customer { }
public string FirstName { get; set; }
```

### Configure Relationships Explicitly
```csharp
// ✅ Good: Clear and explicit relationship configuration
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .HasOne(o => o.Customer)
        .WithMany(c => c.Orders)
        .HasForeignKey(o => o.CustomerId)
        .OnDelete(DeleteBehavior.Restrict);
}
```

### Use Data Annotations or Fluent API Consistently
```csharp
// Option 1: Data Annotations
public class Customer
{
    [Key]
    public int Id { get; set; }

    [Required]
    [StringLength(100)]
    public string Name { get; set; }
}

// Option 2: Fluent API
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Customer>()
        .Property(c => c.Name)
        .IsRequired()
        .HasMaxLength(100);
}
```

## 2. Performance Optimization

### Use Async Methods
```csharp
// ❌ Bad: Blocking calls
var customers = context.Customers.ToList();

// ✅ Good: Async operations
var customers = await context.Customers.ToListAsync();
```

### Include Only Required Data
```csharp
// ❌ Bad: Loading unnecessary data
var orders = context.Orders.Include(o => o.Customer)
                         .Include(o => o.OrderItems)
                         .ThenInclude(i => i.Product)
                         .ToList();

// ✅ Good: Loading only what's needed
var orderSummaries = context.Orders
    .Select(o => new OrderSummary
    {
        OrderId = o.Id,
        CustomerName = o.Customer.Name,
        TotalAmount = o.OrderItems.Sum(i => i.Quantity * i.Price)
    })
    .ToList();
```

### Use No-Tracking Queries for Read-Only Data
```csharp
// ✅ Good: No-tracking for read-only data
var products = await context.Products
    .AsNoTracking()
    .ToListAsync();
```

## 3. Change Tracking and Saving

### Use Bulk Operations for Large Datasets
```csharp
// ❌ Bad: Individual saves
foreach (var order in orders)
{
    context.Orders.Add(order);
    await context.SaveChangesAsync(); // Expensive!
}

// ✅ Good: Bulk operation
context.Orders.AddRange(orders);
await context.SaveChangesAsync(); // Single save
```

### Dispose DbContext Properly
```csharp
// ✅ Good: Using statement ensures proper disposal
using (var context = new MyDbContext())
{
    // Work with context
}

// ✅ Better: Dependency injection in ASP.NET Core
public class OrderService
{
    private readonly MyDbContext _context;
    
    public OrderService(MyDbContext context)
    {
        _context = context;
    }
}
```

## 4. Security and Error Handling

### Prevent SQL Injection
```csharp
// ❌ Bad: String concatenation
var query = "SELECT * FROM Customers WHERE Name = '" + userName + "'";

// ✅ Good: Parameterized queries
var customers = context.Customers
    .Where(c => c.Name == userName)
    .ToList();
```

### Handle Concurrency
```csharp
public class Order
{
    public int Id { get; set; }
    [ConcurrencyCheck]
    public byte[] RowVersion { get; set; }
}

// Handle concurrency conflicts
try
{
    await context.SaveChangesAsync();
}
catch (DbUpdateConcurrencyException ex)
{
    // Handle the conflict
}
```

## 5. Testing

### Use In-Memory Database for Testing
```csharp
public class OrderServiceTests
{
    [Fact]
    public async Task CreateOrder_ValidData_Success()
    {
        // Arrange
        var options = new DbContextOptionsBuilder<MyDbContext>()
            .UseInMemoryDatabase("TestDb")
            .Options;

        using var context = new MyDbContext(options);
        var service = new OrderService(context);

        // Act & Assert
        // ... test code
    }
}
```

## 6. Maintenance and Organization

### Use Repository Pattern When Needed
```csharp
public interface IOrderRepository
{
    Task<Order> GetByIdAsync(int id);
    Task<IEnumerable<Order>> GetAllAsync();
    Task AddAsync(Order order);
    Task UpdateAsync(Order order);
    Task DeleteAsync(int id);
}

public class OrderRepository : IOrderRepository
{
    private readonly MyDbContext _context;

    public OrderRepository(MyDbContext context)
    {
        _context = context;
    }

    public async Task<Order> GetByIdAsync(int id)
    {
        return await _context.Orders
            .Include(o => o.OrderItems)
            .FirstOrDefaultAsync(o => o.Id == id);
    }
    // ... other implementations
}
```

### Use Configuration Classes
```csharp
public class OrderConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> builder)
    {
        builder.HasKey(o => o.Id);
        
        builder.Property(o => o.OrderDate)
            .IsRequired();

        builder.HasOne(o => o.Customer)
            .WithMany(c => c.Orders)
            .HasForeignKey(o => o.CustomerId)
            .OnDelete(DeleteBehavior.Restrict);
    }
}

// In DbContext
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new OrderConfiguration());
}
```

## 7. Migration Management

### Version Control Migrations
```csharp
// Generate migrations with meaningful names
dotnet ef migrations add AddOrderStatusColumn

// Always review migration code before applying
public partial class AddOrderStatusColumn : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.AddColumn<string>(
            name: "Status",
            table: "Orders",
            maxLength: 50,
            nullable: false,
            defaultValue: "Pending");
    }
}
```

### Use Seed Data Properly
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderStatus>().HasData(
        new OrderStatus { Id = 1, Name = "Pending" },
        new OrderStatus { Id = 2, Name = "Processing" },
        new OrderStatus { Id = 3, Name = "Completed" }
    );
}
```
