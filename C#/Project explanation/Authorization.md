# Add Authorization to Blazor Server

1. Lets get started with new Blazor Server Project.
- Open Visual Studio
- Create new project
- Select Blazor App. Name it as `BlazorApp` and `Authorization demo` for soulution name.Press `Create`. Make sure that `Blazor Server app` is selected.
- Change Authentication to `Individual User Account` and store user accounts in app [screen shot](https://github.com/Dmytro-Hryshyn/Knowledge-Base/blob/master/Images/Authentication.jpg)
- And press `Create`

2. Open  file `appsettings.json` in soulution explorer,  change Data Base name to somthing meaningful.
   I will set `DataBase=AuthozitionDB` it will change a name for DataBase.

```json
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=AuthorizationDB;Trusted_Connection=True;MultipleActiveResultSets=true"
```
- Press Ctrl+Q and start typing `Package Manager Console` and hit Enter. It will open a new window whit package manager. 
  Now we need to update DataBase. Just type `update-database` and press `enter`.

3. Open `Startup.cs` file. We need to add some service. Inside of `public void ConfigureServices` insert small block of code `AddRoles<IdentityRole>()` as shown below.
   

```csharp
 public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<ApplicationDbContext>(options =>
                options.UseSqlServer(
                    Configuration.GetConnectionString("DefaultConnection")));
            services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
                
                 //Insert block right here.
                .AddRoles<IdentityRole>()
              
                .AddEntityFrameworkStores<ApplicationDbContext>();
            services.AddRazorPages();
            services.AddServerSideBlazor();
            services.AddScoped<AuthenticationStateProvider, RevalidatingIdentityAuthenticationStateProvider<IdentityUser>>();
            services.AddSingleton<WeatherForecastService>();
        }
```

4. Create a new Blazor Page `SetUpRoles.razor` as an example.
```html
@page "/setuproles"
@using Microsoft.AspNetCore.Identity
@using Microsoft.Extensions.Configuration

@inject RoleManager<IdentityRole> roleManager
@inject UserManager<IdentityUser> userManager
@inject IConfiguration config
<h3>Set Up Role</h3>

<p>This page designed to assign role for users</p>

@code {
    protected override async Task OnParametersSetAsync()
    {
        await SetUpAuth();

    }
    private async Task SetUpAuth()
    {

        string[] roles = { "Administrator" };
        foreach (var role in roles)
        {
            var roleExist = await roleManager.RoleExistsAsync(role);

            if (roleExist == false)
            {
                await roleManager.CreateAsync(new IdentityRole(role));
            }
        }
        var user = await userManager.FindByEmailAsync(config.GetValue<string>("AdminUser"));
        if (user != null)
        {
            await userManager.AddToRoleAsync(user, "Administrator");
        }
    }
}

```

5. Open `appsettings.json` and lets add Amdin role to it
```json
  "AdminUser" : "prizm.drums@gmail.com"
```

6. Open a counter page.
-  Add this block of code below `@page` atribute `@attribute [Authorize]`. Access this page could only Autorized users.
-  Or `@attribute [Authorize(Roles ="Administrator")]` Page available only specific Role.

7. We can restrict rights for some specific part of page or component. As shown below if you are Admin you can see h1 tag `You are Admin` if not `You Are not Admin`
```html
 <AuthorizeView Roles="Administrator">
 <Authorized>
 <h1>You are Admin</h1>
 </Authorized>

<NotAuthorized>
<h1>You are not admin</h1>
</NotAuthorized>
 </AuthorizeView Roles="Administrator">
```
