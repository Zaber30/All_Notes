## ASP.NET Core Routing Types

### 1. Conventional Routing (Traditional)

Defined centrally in Program.cs. Best for MVC with Views.

C#

```
// In Program.cs
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

**Access**: /Products/Details/5

---

### 2. Attribute Routing - Basic

C#

```
[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll() { ... }

    [HttpGet("{id}")]
    public IActionResult GetById(int id) { ... }
}
```

**URLs**:

- GET /api/products
- GET /api/products/5

---

### 3. Attribute Routing with Tokens [controller] & [action]

C#

```
[Route("api/[controller]/[action]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll() { ... }

    [HttpGet]
    public IActionResult GetById(int id) { ... }
}
```

**URLs**:

- GET /api/products/getall
- GET /api/products/getbyid/5

---

### 4. Full Route Directly on Action

C#

```
[ApiController]
public class ProductsController : ControllerBase
{
    [Route("api/product/getall")]
    [HttpGet]
    public IActionResult GetAll() { ... }

    [Route("api/product/detail/{id}")]
    [HttpGet]
    public IActionResult GetById(int id) { ... }
}
```

---

### 5. Best Practice (Recommended for APIs)

C#

```
[ApiController]
[Route("api/products")]        // Base Route
public class ProductsController : ControllerBase
{
    [HttpGet]                                    // GET /api/products
    public IActionResult GetAll() { ... }

    [HttpGet("{id}")]                            // GET /api/products/5
    public IActionResult GetById(int id) { ... }

    [HttpPost]                                   // POST /api/products
    public IActionResult Create([FromBody] Product product) { ... }

    [HttpPut("{id}")]                            // PUT /api/products/5
    public IActionResult Update(int id, Product product) { ... }
}
```

---

### 6. Minimal APIs (No Controller)

C#

```
// In Program.cs
app.MapGet("/api/products", () => products);

app.MapGet("/api/products/{id}", (int id) => product);

app.MapPost("/api/products", (Product product) => Results.Created(...));
```

---

### Route Parameters & Constraints

C#

```
[HttpGet("{id:int:min(1)}")]           // Must be integer >= 1
public IActionResult GetById(int id) { ... }

[HttpGet("search/{name:alpha}/{page:int?}")]  // Optional page
public IActionResult Search(string name, int? page) { ... }
```

---

### Summary Table

|Type|Where Defined|Best For|Cleanliness|
|---|---|---|---|
|Conventional|Program.cs|MVC Views|Medium|
|Attribute + Tokens|Controller + Action|General|Good|
|Base Route + Action|Controller + Action|**Web APIs**|**Best**|
|Full Route on Action|Action only|Specific needs|Clear|
|Minimal APIs|Program.cs|Simple & Fast APIs|Excellent|

---

**Tip**: For **Web APIs**, use this pattern most of the time:

C#

```
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase { ... }
```

Would you like me to expand any section or add more examples (like Areas, Versioning, Route Groups)?