# Add `appsettings.json` to console app

1. Install NuGet packages:
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.Binder
- Microsoft.Extensions.Configuration.Json
- Microsoft.Extensions.Configuration.EnvironmentVariables

In Visual Studio use Manage NuGet Packages window.<br>
Or just type `dotnet add package Microsoft.Extensions.Configuration`

2. Add `appsettings.json` to project.

3. Add properties for appsetting.json in project file. 

```xml
Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.Extensions.Configuration" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Configuration.Binder" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Configuration.Json" Version="5.0.0" />
  </ItemGroup>

  <ItemGroup>
    <None Update="appsettings.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
  </ItemGroup>

</Project>
```
4. To start use Configuration manager just add few lines of code to `Program.cs` file.

```csharp
using System;
using System.IO;
using Microsoft.Extensions.Configuration;
namespace ReadAppsettings
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder();
            BuildConfig(builder);
            var name = builder.Build().GetValue<string>("Name");
            Console.WriteLine(name);

        }

        static void BuildConfig(IConfigurationBuilder builder)
        {
            builder.SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json", optional:false, reloadOnChange:true);
        }
    }
}
```
5. Add Key-Value pairs to  `appsettings.json` file to be able read from it.

```Json
{
	"Name":"John Snow"
}
```

