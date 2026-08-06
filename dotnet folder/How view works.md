Browser Request
       │
       ▼
Controller
       │
return View()
       │
       ▼
Specific View
(Index.cshtml, Edit.cshtml, etc.)
       │
       ▼
_ViewStart.cshtml
       │
       ▼
Determines the Layout
(Layout = "_Layout")
       │
       ▼
_ViewImports.cshtml
(Applies namespaces & Tag Helpers)
       │
       ▼
_Layout.cshtml
       │
       ▼
@RenderBody()
       │
       ▼
Specific View Content
       │
       ▼
Final HTML
       │
       ▼
Browser


                 HomeController
                       │
                return View()
                       │
                       ▼
          Views/Home/Index.cshtml
                       │
                       ▼
              _ViewStart.cshtml
                       │
         Layout = "_Layout"
                       │
                       ▼
             _Layout.cshtml
                       │
            @RenderBody()
                       │
                       ▼
        Views/Home/Index.cshtml
                       │
                       ▼
             Final HTML Response


A clean folder structure looks like this:

```
Views
│
├── Home
│   ├── Index.cshtml
│   └── About.cshtml
│
├── Admin
│   ├── Dashboard.cshtml
│   ├── Users.cshtml
│   └── _ViewStart.cshtml
│
├── Customer
│   ├── Index.cshtml
│   ├── Orders.cshtml
│   └── _ViewStart.cshtml
│
├── Shared
│   ├── _Layout.cshtml
│   ├── _AdminLayout.cshtml
│   └── _CustomerLayout.cshtml
│
├── _ViewImports.cshtml
└── _ViewStart.cshtml
```