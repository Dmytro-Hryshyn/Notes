# Configuration

#### Конфигурационный файлы
В проекте **Blazor Server** существует своя иерархия конфигурационных фалов,
 ниже лежащие слои переписывают верхние слои.<br>
Умение работать с конфигурационными фалами **очень важно**,
 так как позволяет немного изменять настройки приложения без перекомпиляции, т.е в RunTime

- Chained configuration - базовая конфигурация на уровне проекта ниже лежащие конифгурационные фалы будут перезаписывать друг друга.
- appsettings.json - конфигурация для Production mode
- appsetting`<environment>`.json - Выбор режима отладки Develop or Production
- App secrets (secrets.json) - позволяет хранить файл который предназначен только для конкретной машины, он храниться на локальном компьютере,
 тут могут храниться подключения к базам данных для тестирования, база данных могут не относить к проекту совсем.

#### Как же начать этим позьваться

Чтоб использовать конфигурацию в файле класса нужно:
1. Добавить пространтсво имен `using Microsoft.Extensions.Configuration;`
2. Создать контруктор с параметром `IConfiguration`
 ```csharp
using Microsoft.Extensions.Configuration;

public class TestClass

{
    private readonly IConfiguration _config;

    public TestClass(IConfiguration config)
    {
        _config=config;
    }
    //Чтоб разделить логику чтения с файла Json создадим переменую
    int userAge=_config.GetValue<int>("User:age");
    Console.WriteLine(userAge);
}

```
Важно иметь валидный Json файл с обьектом User  и полем age.<p>



```json

{"User": { "Age":30} } 

```
Чтоб использовать конфигурационный файл в страницах нужно:
1. `@using Microsoft.Extensions.Configuration;` - добавить пространство имён
2. `@inject Iconfiguration config`
3. в секции код создать такую контрукцию:

```csharp
public TestClass(Iconfiguration config)
{
  protected ovveride OnInitialized()
    {
        base.OnInitialized()
        int value=config.GetValue<int>("User:age");
    }
}
```
