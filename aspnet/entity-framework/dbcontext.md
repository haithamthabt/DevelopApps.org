---
layout: section
title: DbContext
parent: Entity Framework
grand_parent: ASP.NET
nav_order: 1
---

# Understanding DbContext

DbContext is the primary class that coordinates Entity Framework functionality for a given data model. It represents a session with the database and can be used to query and save instances of your entities.

## Key Responsibilities

1. **Database Connection Management**
   - Manages database connections
   - Handles connection string configuration
   - Provides transaction support

2. **Query Management**
   - Converts LINQ to SQL
   - Tracks query results
   - Manages caching

3. **Change Tracking**
   - Tracks entity modifications
   - Records insert/update/delete operations
   - Manages entity states

4. **Data Persistence**
   - Saves tracked changes to database
   - Handles concurrency
   - Manages transactions

## Creating a DbContext

```csharp
public class ApplicationDbContext : DbContext
{
    // DbSet properties for your entities
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("your_connection_string");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Configure entity relationships, constraints, etc.
        modelBuilder.Entity<Order>()
            .HasOne(o => o.Customer)
            .WithMany(c => c.Orders);
    }
}
```

## Using DbContext

```csharp
using (var context = new ApplicationDbContext())
{
    // Querying data
    var customers = context.Customers
        .Where(c => c.Country == "USA")
        .ToList();

    // Adding new data
    var newCustomer = new Customer { Name = "John Doe" };
    context.Customers.Add(newCustomer);

    // Updating data
    var customer = context.Customers.First();
    customer.Name = "Jane Doe";

    // Deleting data
    context.Customers.Remove(customer);

    // Save all changes
    context.SaveChanges();
}
```

## Best Practices

1. **Lifetime Management**
   - Use dependency injection
   - Create scoped instances
   - Dispose properly

2. **Configuration**
   - Use separate configuration classes
   - Apply conventions consistently
   - Document relationships

3. **Performance**
   - Use async methods
   - Enable change tracking only when needed
   - Use projections for read-only queries

4. **Error Handling**
   - Handle concurrency conflicts
   - Implement retry logic
   - Log database errors
