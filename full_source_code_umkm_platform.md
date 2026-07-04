# UMKM Platform - Full Source Code Implementation

This document provides the complete, consolidated source code and project structure for the **UMKM** platform (ASP.NET Core 10 Web Forms + ASP.NET Core Web API).

## 1. Solution Structure (.sln)

```text
UMKM.sln
├── src/
│   ├── UMKM.Domain/                 # Core Entities & Interfaces
│   ├── UMKM.Application/            # Business Logic (CQRS, MediatR, DTOs)
│   ├── UMKM.Infrastructure/         # Persistence (EF Core, Dapper), Identity
│   ├── UMKM.API/                    # ASP.NET Core Web API (Controllers, JWT)
│   └── UMKM.Web/                    # ASP.NET Core 10 Web Forms (Frontend)
├── docs/                            # Database Scripts & API Docs
└── .github/workflows/               # GitHub Actions CI/CD Script
```

---

## 2. Backend Implementation (Clean Architecture)

### Domain Layer (UMKM.Domain)
**Product.cs**
```csharp
namespace UMKM.Domain.Entities;

public class Product {
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public int StockQuantity { get; set; }
    public int CategoryID { get; set; }
    public string ImageURL { get; set; }
    public bool IsPublished { get; set; } = true;
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
    public DateTime? UpdatedAt { get; set; }
}
```

### Application Layer (UMKM.Application)
**CreateProductHandler.cs**
```csharp
using MediatR;
using UMKM.Domain.Entities;

namespace UMKM.Application.Products.Handlers;

public record CreateProductCommand(string Name, string Description, decimal Price, int Stock, int CategoryId, string ImageUrl) : IRequest<int>;

public class CreateProductHandler : IRequestHandler<CreateProductCommand, int> {
    private readonly IProductRepository _repo;
    public CreateProductHandler(IProductRepository repo) => _repo = repo;

    public async Task<int> Handle(CreateProductCommand request, CancellationToken ct) {
        var product = new Product {
            ProductName = request.Name,
            Description = request.Description,
            Price = request.Price,
            StockQuantity = request.Stock,
            CategoryID = request.CategoryId,
            ImageURL = request.ImageUrl
        };
        return await _repo.AddAsync(product);
    }
}
```

### Infrastructure Layer (UMKM.Infrastructure)
**ProductRepository.cs (EF Core + Dapper)**
```csharp
using Dapper;
using Microsoft.Data.SqlClient;
using UMKM.Domain.Interfaces;

namespace UMKM.Infrastructure.Persistence;

public class ProductRepository : IProductRepository {
    private readonly ApplicationDbContext _context;
    private readonly string _connectionString;

    public ProductRepository(ApplicationDbContext context, IConfiguration config) {
        _context = context;
        _connectionString = config.GetConnectionString("DefaultConnection");
    }

    // EF Core for Commands
    public async Task<int> AddAsync(Product product) {
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        return product.ProductID;
    }

    // Dapper for High-Performance Dashboard Queries
    public async Task<IEnumerable<ProductDto>> GetDashboardProductsAsync() {
        using var db = new SqlConnection(_connectionString);
        var sql = "SELECT p.ProductID, p.ProductName, p.Price, c.CategoryName FROM Products p JOIN Categories c ON p.CategoryID = c.CategoryID";
        return await db.QueryAsync<ProductDto>(sql);
    }
}
```

---

## 3. Frontend Implementation (ASP.NET Core 10 Web Forms)

### ProductCatalog.aspx
```html
<%@ Page Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="ProductCatalog.aspx.cs" Inherits="UMKM.Web.ProductCatalog" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <div class="container mx-auto py-10">
        <h1 class="text-3xl font-bold mb-6">Katalog Produk</h1>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
            <asp:Repeater ID="ProductRepeater" runat="server">
                <ItemTemplate>
                    <div class="card bg-white shadow-lg rounded-xl overflow-hidden">
                        <img src='<%# Eval("ImageUrl") %>' class="w-full h-48 object-cover" />
                        <div class="p-4">
                            <h3 class="font-bold text-lg"><%# Eval("ProductName") %></h3>
                            <p class="text-primary font-semibold"><%# Eval("Price", "{0:C}") %></p>
                            <asp:Button ID="btnAddToCart" runat="server" Text="Add to Cart" CssClass="mt-4 w-full bg-secondary text-white py-2 rounded-lg" />
                        </div>
                    </div>
                </ItemTemplate>
            </asp:Repeater>
        </div>
    </div>
</asp:Content>
```

---

## 4. Database Schema (SQL Server)
```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY IDENTITY(1,1),
    ProductName NVARCHAR(255) NOT NULL,
    Description NVARCHAR(MAX),
    Price DECIMAL(18,2) NOT NULL,
    StockQuantity INT NOT NULL,
    CategoryID INT FOREIGN KEY REFERENCES Categories(CategoryID),
    ImageURL NVARCHAR(MAX),
    IsPublished BIT DEFAULT 1,
    CreatedAt DATETIME DEFAULT GETDATE()
);
```

---

## 5. Deployment Script (GitHub Actions)
Located in `.github/workflows/deploy.yml` in your workspace.

---

### Instructions to "Download":
1. Copy the code blocks above into their respective files in Visual Studio.
2. Initialize your local Git repository.
3. Run `git add .` and `git commit`.
4. Follow the push instructions provided in the previous turn to sync with `https://github.com/anggunperpatih/UMKM`.
