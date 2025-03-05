---
layout: section
title: Database Approaches
nav_order: 3
parent: MVC
---

# Database Approaches in ASP.NET MVC

## Table of Contents
1. [Database-First Approach](#database-first-approach)
   - [When to Use Database-First](#when-to-use-database-first)
   - [Implementation Steps](#implementation-steps)
2. [Code-First Approach](#code-first-approach)
   - [When to Use Code-First](#when-to-use-code-first)
   - [Implementation Steps](#implementation-steps-1)
3. [Comparing Approaches](#comparing-approaches)
   - [Database-First Advantages](#database-first-advantages)
   - [Code-First Advantages](#code-first-advantages)
4. [Best Practices](#best-practices)
5. [Common Pitfalls to Avoid](#common-pitfalls-to-avoid)
6. [Next Steps](#next-steps)

When working with databases in ASP.NET MVC, you have two main approaches: Database-First and Code-First. Each approach has its advantages and is suited for different scenarios.

## Database-First Approach

In the Database-First approach, you start with an existing database and generate your Entity Framework models from it.

### When to Use Database-First
- Working with an existing database
- Complex database schemas that are managed by DBAs
- When database design drives the application design
- Integration with legacy systems

### Implementation Steps
1. **Create/Connect to Database**
   ```sql
   CREATE TABLE Products (
       ProductId INT PRIMARY KEY IDENTITY(1,1),
       Name NVARCHAR(100),
       Price DECIMAL(18,2),
       Description NVARCHAR(MAX)
   )
   ```

2. **Generate Models**
   ```bash
   Scaffold-DbContext "Server=.;Database=YourDB;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
   ```

3. **Use Generated Models**
   ```csharp
   public class ProductsController : Controller
   {
       private readonly YourDBContext _context;

       public ProductsController(YourDBContext context)
       {
           _context = context;
       }

       public async Task<IActionResult> Index()
       {
           return View(await _context.Products.ToListAsync());
       }
   }
   ```

## Code-First Approach

In the Code-First approach, you start by creating your C# classes (models) and Entity Framework generates the database from these classes.

### When to Use Code-First
- New projects without existing databases
- Full control over database schema through code
- Test-driven development
- Agile development with frequent model changes

### Implementation Steps
1. **Install Required Packages**
   ```bash
   # Install Entity Framework Core packages
   dotnet add package Microsoft.EntityFrameworkCore
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.Tools
   ```

   Or add to your `.csproj` file:
   ```xml
   <ItemGroup>
     <PackageReference Include="Microsoft.EntityFrameworkCore" Version="7.0.0" />
     <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="7.0.0" />
     <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="7.0.0">
       <PrivateAssets>all</PrivateAssets>
       <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
     </PackageReference>
   </ItemGroup>
   ```

2. **Configure Services in Program.cs**
   ```csharp
   var builder = WebApplication.CreateBuilder(args);

   // Add DbContext to the services
   builder.Services.AddDbContext<ShopContext>(options =>
       options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
   ```
   > Wondering why we need a Program file? Learn its purpose and importance in our [Program File guide](../program-file.md).
   >
   > For details about Entity Framework configuration and dependency injection, see our [Configuration & DI guide](../entity-framework/configuration.md).

3. **Add Connection String in appsettings.json**
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=.;Database=ShopDB;Trusted_Connection=True;TrustServerCertificate=True;"
     }
   }
   ```

4. **Create Model Classes**
   ```csharp
   public class Product
   {
       public int ProductId { get; set; }
       public string Name { get; set; }
       public decimal Price { get; set; }
       public string Description { get; set; }
   }
   ```

2. **Create DbContext**
   ```csharp
   public class ShopContext : DbContext
   {
       public ShopContext(DbContextOptions<ShopContext> options)
           : base(options)
       {
       }

       public DbSet<Product> Products { get; set; }

       protected override void OnModelCreating(ModelBuilder modelBuilder)
       {
           modelBuilder.Entity<Product>()
               .Property(p => p.Name)
               .IsRequired()
               .HasMaxLength(100);

           modelBuilder.Entity<Product>()
               .Property(p => p.Price)
               .HasPrecision(18, 2);
       }
   }
   ```

3. **Create Migration**
   ```bash
   Add-Migration InitialCreate
   Update-Database
   ```

## Comparing Approaches

### Database-First Advantages
- Perfect for existing databases
- Accurate representation of complex database schemas
- DBAs can manage database design independently
- Better for complex stored procedures and views

### Code-First Advantages
- Better version control of database schema
- Easier unit testing
- More control over domain model
- Simpler deployment process
- Better for rapid development

## Best Practices

1. **Choose the Right Approach**
   - Consider your project requirements
   - Think about team expertise
   - Evaluate maintenance needs

2. **Version Control**
   - Keep migrations in source control
   - Document major schema changes
   - Use meaningful migration names

3. **Performance**
   - Use appropriate indexes
   - Implement lazy loading where needed
   - Monitor query performance

4. **Security**
   - Use parameterized queries
   - Implement proper connection string security
   - Follow principle of least privilege

## Common Pitfalls to Avoid

1. **Database-First**
   - Don't modify generated code directly
   - Be careful with manual schema changes
   - Watch for relationship mapping issues

2. **Code-First**
   - Don't ignore migration conflicts
   - Be careful with model changes in production
   - Don't forget to backup before migrations

## Next Steps
- Learn more about [Entity Framework Core](../entity-framework/index.md)
- Understand [Entity Framework configuration and dependency injection](../entity-framework/configuration.md)
- Study [Entity Framework best practices](../entity-framework/best-practices.md)
- Explore [DbContext in detail](../entity-framework/dbcontext.md)
