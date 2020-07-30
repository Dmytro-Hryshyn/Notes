# Add authentication by manual (Blazor Server)

 1. Open `nugetpackege manager` add following packages:
	* Microsoft.AspNetCore.Identity.UI
	* Microsoft.AspNetCore.Identity.EntityFrameworkCore
	* Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
	* Microsoft.EntityFrameworkCore.SqlServer
	* Microsoft.EntityFrameworkCore.Tools

2. Create new class `ApplicationDbContext.cs` in `Data` folder.

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace BlazorApp.Data
{

    public class ApplicationDbContext : IdentityDbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

3. Open `Startup.cs` inside service configuration `ConfigureServices` add following code and add 
necessary using statemnets

```csharp
services.AddDbContext<ApplicationDbContext>(options =>
                options.UseSqlServer(
                    Configuration.GetConnectionString("DefaultConnection")));
            services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
                .AddEntityFrameworkStores<ApplicationDbContext>();

//add using statement later
services.AddScoped<AuthenticationStateProvider, RevalidatingIdentityAuthenticationStateProvider<IdentityUser>>();
```

4. Open `Startup.cs` inside method `Configure` add few methods after `app.UseRouting();` method

```csharp
//add to existing code
 app.UseAuthentication();
 app.UseAuthorization();

 app.UseEndpoints(endpoints =>
            {
                //end this 
                endpoints.MapControllers();

                //Should be in new project from creation
                endpoints.MapBlazorHub();
                endpoints.MapFallbackToPage("/_Host");
            });
```
5. Open file `app.razor` should looks like this:

```
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```

6. Inside `shared` folder create new file - `LoginDisplay.razor` with following code inside

```
<AuthorizeView>
    <Authorized>
        <a href="Identity/Account/Manage">Hello, @context.User.Identity.Name!</a>
        <form method="post" action="Identity/Account/LogOut">
            <button type="submit" class="nav-link btn btn-link">Log out</button>
        </form>
    </Authorized>
    <NotAuthorized>
        <a href="Identity/Account/Register">Register</a>
        <a href="Identity/Account/Login">Log in</a>
    </NotAuthorized>
</AuthorizeView>
```

7. Create `Areas` folder in Solution level
8. Create `Identity` inside of Areas (Areas/Identity)
9. Inside `Identity` folder create file `RevalidatingIdentityAuthenticationStateProvider.cs` with folowing code and right NameSpace

```csharp
using System;
using System.Security.Claims;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Server;
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;

//Change namespace according to your project name
namespace <Project Name>.Areas.Identity
{
    public class RevalidatingIdentityAuthenticationStateProvider<TUser>
        : RevalidatingServerAuthenticationStateProvider where TUser : class
    {
        private readonly IServiceScopeFactory _scopeFactory;
        private readonly IdentityOptions _options;

        public RevalidatingIdentityAuthenticationStateProvider(
            ILoggerFactory loggerFactory,
            IServiceScopeFactory scopeFactory,
            IOptions<IdentityOptions> optionsAccessor)
            : base(loggerFactory)
        {
            _scopeFactory = scopeFactory;
            _options = optionsAccessor.Value;
        }

        protected override TimeSpan RevalidationInterval => TimeSpan.FromMinutes(30);

        protected override async Task<bool> ValidateAuthenticationStateAsync(
            AuthenticationState authenticationState, CancellationToken cancellationToken)
        {
            // Get the user manager from a new scope to ensure it fetches fresh data
            var scope = _scopeFactory.CreateScope();
            try
            {
                var userManager = scope.ServiceProvider.GetRequiredService<UserManager<TUser>>();
                return await ValidateSecurityStampAsync(userManager, authenticationState.User);
            }
            finally
            {
                if (scope is IAsyncDisposable asyncDisposable)
                {
                    await asyncDisposable.DisposeAsync();
                }
                else
                {
                    scope.Dispose();
                }
            }
        }

        private async Task<bool> ValidateSecurityStampAsync(UserManager<TUser> userManager, ClaimsPrincipal principal)
        {
            var user = await userManager.GetUserAsync(principal);
            if (user == null)
            {
                return false;
            }
            else if (!userManager.SupportsUserSecurityStamp)
            {
                return true;
            }
            else
            {
                var principalStamp = principal.FindFirstValue(_options.ClaimsIdentity.SecurityStampClaimType);
                var userStamp = await userManager.GetSecurityStampAsync(user);
                return principalStamp == userStamp;
            }
        }
    }
}


```

10. Create folder `Pages` inside of Identity folder(Areas/Identity/Pages)
11. Create folder `Account` inside of Pages folder.
12. Create file `LogOut.cshtml` inside of Account folder (Areas/Identity/Pages/Account) with some code

```csharp
@page
@using Microsoft.AspNetCore.Identity
@attribute [IgnoreAntiforgeryToken]
@inject SignInManager<IdentityUser> SignInManager
@functions {
    public async Task<IActionResult> OnPost()
    {
        if (SignInManager.IsSignedIn(User))
        {
            await SignInManager.SignOutAsync();
        }

        return Redirect("~/");
    }
}

```
13. Create folder `Shared` inside of `Pages`(Areas/Identity/Pages/)
14. Create file `_LoginPartial.cshtml` with some code

```csharp

@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<ul class="navbar-nav">
@if (SignInManager.IsSignedIn(User))
{
    <li class="nav-item">
        <a  class="nav-link text-dark" asp-area="Identity" asp-page="/Account/Manage/Index" title="Manage">Hello @User.Identity.Name!</a>
    </li>
    <li class="nav-item">
        <form class="form-inline" asp-area="Identity" asp-page="/Account/Logout" asp-route-returnUrl="/" method="post">
            <button  type="submit" class="nav-link btn btn-link text-dark">Logout</button>
        </form>
    </li>
}
else
{
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="Identity" asp-page="/Account/Register">Register</a>
    </li>
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="Identity" asp-page="/Account/Login">Login</a>
    </li>
}
</ul>
```
15. Come back to `Startup.cs` file and add missing using statement ` services.AddScoped<AuthenticationStateProvider, RevalidatingIdentityAuthenticationStateProvider<IdentityUser>>();` for this method.

16 Inside of `appsettings.json` need to add ConnectionString like so:

```json

,"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=<DataBase Name>;Trusted_Connection=True;MultipleActiveResultSets=true"

```
We also can rename DataBase=<>

17. Open Package Manger Console using ctrl+q and
type `Add-Migration "YourName for migration"`
it should create migration, should no create an errors
18. Inside of Package Mangaer Console type `Update-DataBase`
19. Open `Shared` folder and open file `MainLayout.razor` add `<LoginDisplay></LoginDisplay>` in Main container

Thats it. Authentication has been created.