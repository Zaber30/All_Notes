The Request Lifecycle

Here is the precise order of operations inside `$app->handleRequest()`:

1. **Incoming Request**: Captured from the browser.
2. **Global Middleware**: Runs on _every single request_ (e.g., checking for Maintenance Mode, trimming strings, validating CSRF tokens).
3. **Route Matching**: The router finds which controller wants this URL.
4. **Route Middleware**: Runs middleware assigned only to that specific route (e.g., checking if the user is logged in via `auth`).
5. **The Controller**: Executes your actual business logic **only** if all previous middleware layers called `return $next($request);`