Yes. Below are the **official C# naming conventions** recommended by Microsoft and followed in most professional C# and ASP.NET Core projects.

|Code Element|Naming Convention|Example|
|---|---|---|
|Namespace|**PascalCase**|`MyApplication.Services`|
|Class|**PascalCase**|`UserService`|
|Struct|**PascalCase**|`StudentRecord`|
|Record|**PascalCase**|`ProductInfo`|
|Interface|**PascalCase** with `I` prefix|`IUserRepository`|
|Enum|**PascalCase**|`UserRole`|
|Enum Member|**PascalCase**|`Administrator`, `PendingApproval`|
|Method (Function)|**PascalCase**|`GetUser()`, `CalculateTotal()`|
|Property|**PascalCase**|`FirstName`, `TotalPrice`|
|Event|**PascalCase**|`UserCreated`|
|Delegate|**PascalCase**|`LogHandler`|
|Constructor|**Same as Class (PascalCase)**|`UserService()`|
|Constant (`const`)|**PascalCase**|`MaxUsers`|
|Public Field|**PascalCase** _(rarely used)_|`TotalCount`|
|Private Field|**_camelCase**|`_userRepository`, `_count`|
|Protected Field|**_camelCase**|`_logger`|
|Static Field|**_camelCase**|`_instance`|
|Local Variable|**camelCase**|`userName`, `totalPrice`|
|Method Parameter|**camelCase**|`userId`, `fileName`|
|Generic Type Parameter|**PascalCase** with `T` prefix|`T`, `TKey`, `TValue`, `TResult`|
|File Name|**Same as Type Name (PascalCase)**|`UserService.cs`|
|Extension Method Class|**PascalCase**|`StringExtensions`|
|Extension Method|**PascalCase**|`ToTitleCase()`|
|Attribute Class|**PascalCase**|`AuthorizeAttribute`|
|Attribute Usage|**PascalCase** (omit `Attribute` suffix)|`[Authorize]`|

---

## Naming Styles

### PascalCase

Every word starts with a capital letter.

```
FirstNameUserServiceCalculateTotal
```

---

### camelCase

First word starts with a lowercase letter; each following word starts with a capital letter.

```
firstNameuserServicetotalPrice
```

---

### _camelCase

Private/protected fields begin with an underscore followed by camelCase.

```
_firstName_userRepository_logger
```

---

## Professional Example

```
namespace MyApplication.Services;public interface IUserRepository{    User GetById(int userId);}public class UserService{    private readonly IUserRepository _userRepository;    private const int MaxUsers = 100;    public string ServiceName { get; set; }    public UserService(IUserRepository userRepository)    {        _userRepository = userRepository;    }    public User GetUser(int userId)    {        User currentUser = _userRepository.GetById(userId);        return currentUser;    }}
```

### Naming used in the example

|Identifier|Convention|
|---|---|
|`MyApplication.Services`|PascalCase (namespace)|
|`IUserRepository`|PascalCase + `I` prefix (interface)|
|`UserService`|PascalCase (class)|
|`_userRepository`|`_camelCase` (private field)|
|`MaxUsers`|PascalCase (constant)|
|`ServiceName`|PascalCase (property)|
|`GetUser()`|PascalCase (method)|
|`userId`|camelCase (parameter)|
|`currentUser`|camelCase (local variable)|