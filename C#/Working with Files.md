# Работа с фалами

- Получить путь к корневой папки проекта можно вот так
```csharp
string path = new DirectoryInfo(Environment.CurrentDirectory).Parent.Parent.FullName + @"\Resource\Square\Inside Radius.png");
```