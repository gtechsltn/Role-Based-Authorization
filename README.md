# Role-Based Authorization
+ https://auth0.com/blog/role-based-authorization-for-aspnet-webapi/
+ https://coderethinked.com/role-based-authorization-in-asp-net-core/
<pre>
How to apply multiple roles
To apply an OR between the roles to a single action we have to separate the roles by a comma

[Authorize(Roles = "SuperUser, Admin")]
public IActionResult Time() => Content(new TimeOnly().ToLongTimeString());
The time action is accessible to users who have either SuperUser or Admin roles

To apply an AND between the roles, we stack the Authorize attribute on the action/controller.

[Authorize(Roles = "SuperUser")]
[Authorize(Roles = "Admin")]
public IActionResult Time() => Content(new TimeOnly().ToLongTimeString());
Now, the time action is accessible only to the users who have both SuperUser and Admin as their roles.
</pre>

## Roles
+ [Authorize(Roles = "Admin")]

### Way 1:
```
[Authorize(Roles = "SuperUser, Admin")]
```

### Way 2:
```
[Authorize(Roles = "SuperUser")]
[Authorize(Roles = "Admin")]
```
```
[HttpDelete]
[Route("{term}")]
[Authorize(Roles = "glossary-admin")]
public ActionResult Delete(string term)
{
  // ... existing code ...
}
```

## Policy
```
// Controllers/GlossaryController.cs

// ...existing code...

namespace Glossary.Controllers
{
  [ApiController]
  [Route("api/[controller]")]
  public class GlossaryController : ControllerBase
  {
    // ...existing code...

    [HttpPost]
    [Authorize(Policy = "CreateAccess")]    //👈 changed code
    public ActionResult Post(GlossaryItem glossaryItem)
    {
      // ...existing code...
    }

    [HttpPut]
    [Authorize(Policy = "UpdateAccess")]    //👈 changed code
    public ActionResult Put(GlossaryItem glossaryItem)
    {
      // ...existing code...
    }

    [HttpDelete]
    [Route("{term}")]
    [Authorize(Policy = "DeleteAccess")]    //👈 changed code
    public ActionResult Delete(string term)
    {
      // ...existing code...
    }
  }
}
```
