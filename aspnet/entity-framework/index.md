---
layout: section
title: Entity Framework
parent: ASP.NET
nav_order: 5
has_children: true
---

# Entity Framework Core

Entity Framework Core (EF Core) is Microsoft's modern object-database mapper for .NET. It enables developers to work with databases using .NET objects, eliminating the need for most of the data-access code they usually need to write.

## Why Use Entity Framework?

1. **Object-Relational Mapping (ORM)**
   - Work with database using C# classes
   - No need to write SQL queries manually
   - Automatic mapping between objects and database tables

2. **Database Independence**
   - Switch between different database providers
   - Support for SQL Server, PostgreSQL, MySQL, SQLite
   - Same code works with different databases

3. **Productivity Features**
   - Automatic change tracking
   - LINQ support for queries
   - Migration management
   - Relationship handling

4. **Development Approaches**
   - Database-First: Generate models from existing database
   - Code-First: Create database from C# classes
   - Model-First: Design models and generate both

## Key Components

1. **DbContext**
   - Central class for database interaction
   - Manages entity instances
   - Handles database connections
   - Configures model & relationships

2. **Entities**
   - C# classes that map to database tables
   - Properties represent table columns
   - Navigation properties for relationships
   - Data annotations for configuration

3. **Migrations**
   - Version control for database schema
   - Track database changes
   - Apply/revert database updates
   - Generate SQL scripts

4. **LINQ Provider**
   - Query databases using LINQ
   - Type-safe queries
   - IntelliSense support
   - Translated to efficient SQL

## Getting Started

1. Install NuGet Packages:
   ```shell
   dotnet add package Microsoft.EntityFrameworkCore
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer  # For SQL Server
   ```

2. Create Entity Classes
3. Set up DbContext
4. Configure Connection String
5. Generate and Apply Migrations
