Чтобы выводить SQL-выражения, которые выполняет Entity Framework в консоль, можно использовать механизм логирования `Microsoft.Extensions.Logging.Console`. Для этого потребуется настроить логирование в контексте данных. Для этого необходимо:

1. Добавить в проект NuGet пакет `Microsoft.Extensions.Logging.Console`.
2. В контексте данных переопределить метод `OnConfiguring` для настройки логирования.

```cs
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Logging;

public class YourDbContext : DbContext
{
    public YourDbContext(DbContextOptions<YourDbContext> options)
        : base(options)
    {
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Настройка логирования SQL-запросов
        optionsBuilder
            .UseLoggerFactory(LoggerFactory.Create(builder =>
            {
                builder.AddConsole();
            }))
            .EnableSensitiveDataLogging()
            .UseSqlServer("Ваша строка подключения");
    }
}
```

В этом примере используется `LoggerFactory.Create` для создания фабрики логирования, которая настроена на вывод логов в консоль. Метод `EnableSensitiveDataLogging` включает логирование чувствительных данных, что может быть полезно для отладки, но следует использовать с осторожностью из-за риска утечки данных.