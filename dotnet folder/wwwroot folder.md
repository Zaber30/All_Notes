While you can name your subfolders anything you like, the web development industry uses a standard structure inside `wwwroot`:

1. `css/` (Cascading Style Sheets)

- **Purpose:** Stores your application's design, layout, and styling files.
- **Common Files:** `site.css`, `bootstrap.min.css`.
- **Example:** The browser loads these to figure out your website's colors, fonts, and dark mode layouts.

2. `js/` (JavaScript)

- **Purpose:** Stores client-side scripts that run directly inside the user's browser.
- **Common Files:** `site.js`, `validation.js`, `jquery.min.js`.
- **Example:** Code that handles frontend button clicks, popup animations, or background API requests (AJAX/Fetch) without reloading the page. 
1. `images/` or `img/` (Visual Assets)

- **Purpose:** Holds all the graphic assets, icons, and illustrations for your website.
- **Common Files:** `logo.png`, `hero-background.jpg`, `favicon.ico` (the tiny icon on the browser tab). 

4. `lib/` (Third-Party Libraries)

- **Purpose:** Stores client-side packages and frameworks that you didn't write yourself.
- **Common Files:** Folders for Bootstrap, jQuery, or FontAwesome.
- **Note:** These are usually downloaded using frontend package managers like LibMan, npm, or a CDN link.
