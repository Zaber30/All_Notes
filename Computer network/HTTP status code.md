| **Category**            | **Status Code** | **Meaning**                                   |
| ----------------------- | --------------: | --------------------------------------------- |
| **1xx – Informational** |         **100** | Continue                                      |
|                         |         **101** | Switching Protocols                           |
|                         |         **103** | Early Hints                                   |
| **2xx – Success**       |         **200** | OK                                            |
|                         |         **201** | Created                                       |
|                         |         **202** | Accepted                                      |
|                         |         **204** | No Content                                    |
| **3xx – Redirection**   |         **301** | Moved Permanently                             |
|                         |         **302** | Found (Temporary Redirect)                    |
|                         |         **303** | See Other                                     |
|                         |         **304** | Not Modified                                  |
|                         |         **307** | Temporary Redirect                            |
|                         |         **308** | Permanent Redirect                            |
| **4xx – Client Error**  |         **400** | Bad Request                                   |
|                         |         **401** | Unauthorized (Authentication Required/Failed) |
|                         |         **403** | Forbidden (Permission Denied)                 |
|                         |         **404** | Not Found                                     |
|                         |         **405** | Method Not Allowed                            |
|                         |         **408** | Request Timeout                               |
|                         |         **409** | Conflict                                      |
|                         |         **415** | Unsupported Media Type                        |
|                         |         **422** | Unprocessable Content (Validation Error)      |
|                         |         **429** | Too Many Requests                             |
| **5xx – Server Error**  |         **500** | Internal Server Error                         |
|                         |         **501** | Not Implemented                               |
|                         |         **502** | Bad Gateway                                   |
|                         |         **503** | Service Unavailable                           |
|                         |         **504** | Gateway Timeout                               |