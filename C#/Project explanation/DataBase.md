# Authentication Data Base

При создании проекта база данных еще не создана, чтоб её создать нужно открыть `appsettings.json`<br>

1. Находим `ConnectionStrings` меняем имя базы данных по желанию.
```json
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=<Имя баззы данных без угловых ковычек>;Trusted_Connection=True;MultipleActiveResultSets=true"

```
2. В Vusiual Sudio поисковике`Ctrl+Q` пишем  `Package Manager Console`

3. В окне консоли пишем `update-database` Visual Studio создаст базу данных с именем которые мы указали  `appsettings.json`