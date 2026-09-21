# План актуализации курса МДК 01.04 (репозиторий mdk0104_IP)

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Привести курс к 2026/27: .NET 10 + VS Code + C# Dev Kit, PostgreSQL 16 в EF Core, актуальные пакеты, README без персональных данных, исправленные расхождения.

**Architecture:** Только документация (Markdown). Код из практик проверяется в эталонных проектах (`C:\Users\prep\AppData\Local\Temp\opencode\mdk0104-ref`), проверенный код переносится в материалы.

**Tech Stack:** .NET SDK 10.0.300 (net10.0), Npgsql.EntityFrameworkCore.PostgreSQL 10.0.3, Microsoft.EntityFrameworkCore.Design/Tools 10.0.12, Hangfire/Hangfire.AspNetCore 1.8.25, Hangfire.InMemory 1.0.0, FluentValidation 12.1.1, Serilog.AspNetCore 10.0.0, Serilog.Sinks.Console 6.1.1, Serilog.Sinks.File 7.0.0, iText7 9.7.0, PdfSharp 6.2.4, EPPlus 8.7.0, DocumentFormat.OpenXml 3.5.1, PostgreSQL 16.

**Spec:** `docs/superpowers/specs/2026-09-21-aktualizaciya-kursa-design.md`

## Global Constraints

- Язык материалов и коммитов — русский; правки точечные, стиль и шаблоны файлов сохраняются.
- Проекты: `net10.0`; окружение — VS Code + C# Dev Kit + `dotnet` CLI. Visual Studio не упоминаем как среду (исключение: стандартные комментарии `.gitignore`).
- Строка подключения в примерах: `Host=localhost;Port=5432;Database=shopdb;Username=postgres;Password=postgres`. При первом появлении в файле добавить примечание: «если при установке PostgreSQL вы задали другой пароль — укажите его».
- Установка пакетов — только `dotnet add package` (никаких `Install-Package`).
- Git: `git add -A`; префикс `docs:`; ветка `feature/actualize-2026`; commit после каждой задачи.
- Push origin — только при `GITHUB_TOKEN`/`GH_TOKEN` (проверить перед push; если не задан — сообщить пользователю и остановиться).
- Верификация кода — только на net10.0; непроверенное в материалы не вставлять.

---

### Task 1: Лекция 1 — .NET 10

**Files:**
- `course/Раздел 1. Основы .NET и С#/Лекция 1. Введение в .NET.md`

- [ ] **Step 1:** «**.NET 5, 6, 7, 8... (2020 г. — н.в.):**» → «**.NET 5–10... (2020 г. — н.в.):**»
- [ ] **Step 2:** Абзац «Для всех **новых проектов** используйте последнюю стабильную версию **.NET 8** (или новее, когда она выйдет).» → «Для всех **новых проектов** используйте последнюю LTS-версию **.NET 10**. Установка: `winget install Microsoft.DotNet.SDK.10` или установщик с dotnet.microsoft.com. Рекомендуемая среда разработки — VS Code с расширением C# Dev Kit (работа через `dotnet` CLI).»
- [ ] **Step 3:** Контрольный вопрос 7: «.NET 5/6/7/8» → «.NET 5–10».
- [ ] **Step 4:** Проверка: `Select-String -Path <файл> -Pattern 'net[678]\.0|\.NET [678]'` — 0 совпадений как актуальных версий.
- [ ] **Step 5:** Commit `docs: лекция 1 — .NET 10 и VS Code`

### Task 2: Раздел 1 — .NET 10 и VS Code (П1, П4, П5, СР1)

**Files:**
- `course/Раздел 1. Основы .NET и С#/Практическая работа 1. Компиляция и запуск .NET приложения.md`
- `course/Раздел 1. Основы .NET и С#/Практическая работа 4. Структуры данных.md`
- `course/Раздел 1. Основы .NET и С#/Практическая работа 5. Работа с LINQ.md`
- `course/Раздел 1. Основы .NET и С#/Самостоятельная работа 1. Консольный калькулятор.md`

- [ ] **Step 1: П1.** `bin/Debug/net8.0` → `net10.0`; `dotnet new console --framework net8.0` → `net10.0`; «например, на `net7.0` или `net6.0`» → «например, на `net9.0` или `net8.0`»; `bin/Release/net8.0/win-x64/publish` → `net10.0`; «присоединиться к процессу через Visual Studio» → «запустить отладчик VS Code (F5)»; «окна Visual Studio, присоединенного к процессу» → «окна VS Code с активной сессией отладки»; вопрос «по сравнению с компиляцией через графическую IDE (например, Visual Studio)» → «(например, VS Code с C# Dev Kit)».
- [ ] **Step 2: П4.** «Создать решение Visual Studio с вышеуказанной структурой» → инструкция через CLI: `dotnet new sln -n Structures`, `dotnet new classlib`, `dotnet new console`, `dotnet sln add`. Проверить весь файл на net6/net8/VS.
- [ ] **Step 3: П5, СР1.** `<TargetFramework>net6.0</TargetFramework>` → `net10.0`; СР1 «Убедитесь, что установлен .NET 6.0 или выше» → «Убедитесь, что установлен .NET SDK 10». Комментарии `.gitignore` (Visual Studio) в СР1 не трогать.
- [ ] **Step 4:** Проверка: `Select-String -Path 'course\Раздел 1*' -Pattern 'net[678]\.0|\.NET [678]|Visual Studio'` — совпадения только в комментариях .gitignore.
- [ ] **Step 5:** Commit `docs: раздел 1 — .NET 10 и VS Code`

### Task 3: Лекции 17, 20, 25 — VS Code и публикация

**Files:**
- `course/Раздел 6. ASP Core/Лекция 17. Введение в ASP.NET Core.md`
- `course/Раздел 6. ASP Core/Лекция 20. Контроллеры и MinimalAPI.md`
- `course/Раздел 6. ASP Core/Лекция 25. Развертывание приложения.md`

- [ ] **Step 1: Л17.** «Установка инструментов (Visual Studio, .NET SDK)» → «Установка инструментов (VS Code + C# Dev Kit, .NET SDK 10)»; «Visual Studio или VS Code.» → «VS Code с расширением C# Dev Kit.»; «`Startup.cs` — конфигурация сервисов и Middleware (в .NET 6+ часто объединено с Program.cs)» → «(начиная с .NET 6 в одной точке входа — Program.cs)».
- [ ] **Step 2: Л20.** «Новый подход с .NET 6» → «Minimal API (появился в .NET 6, актуален в .NET 10)» — согласовать с контекстом абзаца.
- [ ] **Step 3: Л25.** «Публикация через Visual Studio» → «Публикация через `dotnet publish`»; «Через Visual Studio (`Publish`)» → «Через CLI: `dotnet publish -c Release -o ./publish`»; блок «Пример публикации через Visual Studio:» и шаги VS → «Пример публикации через `dotnet publish`:» + CLI-команды; «через GitHub Actions, Visual Studio или Azure CLI» → «через GitHub Actions, `dotnet publish` или Azure CLI».
- [ ] **Step 4:** Проверка: `Select-String -Path 'course\Раздел 6*' -Pattern 'Visual Studio|net[678]\.0|\.NET [678]'` — 0 (кроме допустимого).
- [ ] **Step 5:** Commit `docs: лекции 17, 20, 25 — VS Code и dotnet publish`

### Task 4: Раздел 4 — пакеты и окружение (П13–П17)

**Files:**
- `course/Раздел 4. Обработка файлов/Практическая работа 13. Обработка текстовых файлов.md`
- `course/Раздел 4. Обработка файлов/Практическая работа 14. Работа с Word.md`
- `course/Раздел 4. Обработка файлов/Практическая работа 15. Работа с Excel.md`
- `course/Раздел 4. Обработка файлов/Практическая работа 16. Работа с Pdf.md`
- `course/Раздел 4. Обработка файлов/Практическая работа 17. Сравнение производительности последовательного, параллельного и асинхронного кода на примере обработки данных.md`

- [ ] **Step 1: П13.** «(.NET Core или .NET 6/7/8 Console Application)» → «(.NET 10 Console Application)».
- [ ] **Step 2: П14.** Установка: `dotnet add package DocumentFormat.OpenXml --version 3.5.1`; убрать/обновить старые версии и упоминания Visual Studio.
- [ ] **Step 3: П15.** `dotnet add package EPPlus --version 8.7.0`; адаптировать инициализацию лицензии под EPPlus 8 (`ExcelPackage.License.SetNonCommercialPersonal(...)` — уточнить по докам пакета при верификации); добавить примечание о лицензии Polyform Noncommercial (учебное использование допустимо).
- [ ] **Step 4: П16.** `Install-Package itext7` → `dotnet add package itext7 --version 9.7.0`; PdfSharp 6.2.4: `dotnet add package PdfSharp --version 6.2.4`; сохранить пояснение про лицензии (AGPL/MIT).
- [ ] **Step 5: П17.** Ревизия на устаревшее (net6/8, VS, Install-Package).
- [ ] **Step 6:** Проверка: `Select-String -Path 'course\Раздел 4*' -Pattern 'net[678]\.0|\.NET [678]|Visual Studio|Install-Package'` — 0.
- [ ] **Step 7:** Commit `docs: раздел 4 — актуальные пакеты и dotnet add package`

### Task 5: Раздел 5 — PostgreSQL (Л15–16, П18–25, СР5)

**Files:** все 11 файлов `course/Раздел 5. Работа с базами данных. EF/`.

- [ ] **Step 1:** Все `optionsBuilder.UseSqlServer("Server=.;Database=ShopDb;Trusted_Connection=True;")` и `options.UseSqlServer(...)` → `UseNpgsql("Host=localhost;Port=5432;Database=shopdb;Username=postgres;Password=postgres")` (с примечанием про пароль при первом появлении).
- [ ] **Step 2: Л15.** Провайдеры: Npgsql как основной пример; добавить установку `dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 10.0.3` и `dotnet add package Microsoft.EntityFrameworkCore.Design --version 10.0.12`.
- [ ] **Step 3: Л16.** Таблицу провайдеров и примеры привести к PostgreSQL; «Пример для SQL Server» → «Пример для PostgreSQL»; SQLite оставить как альтернативу; пул соединений — пример для Postgres.
- [ ] **Step 4: П18.** Scaffold: `dotnet ef dbcontext scaffold "Host=localhost;Port=5432;Database=shopdb;Username=postgres;Password=postgres" Npgsql.EntityFrameworkCore.PostgreSQL -o Models`; добавить установку PostgreSQL 16 (winget/установщик) и примечание про пароль; CodeFirst/DatabaseFirst — на Postgres.
- [ ] **Step 5: П19–24.** Строки подключения → Postgres; если упоминается `dotnet ef` — добавить `dotnet tool install --global dotnet-ef --version 10.0.12`.
- [ ] **Step 6: П25.** Изоляция/блокировки: уровни PostgreSQL (READ COMMITTED по умолчанию, REPEATABLE READ, SERIALIZABLE), `SELECT … FOR UPDATE` через `FromSqlRaw`; теорию сохранить.
- [ ] **Step 7: СР5.** Строка подключения → Postgres; условие задания не менять.
- [ ] **Step 8:** Проверка: `Select-String -Path 'course\Раздел 5*' -Pattern 'UseSqlServer|Server=\.|Trusted_Connection|Microsoft\.EntityFrameworkCore\.SqlServer'` — 0.
- [ ] **Step 9:** Commit `docs: раздел 5 — PostgreSQL вместо SQL Server`

### Task 6: Раздел 6 — Hangfire, FluentValidation, Serilog

**Files:**
- `course/Раздел 6. ASP Core/Практическая работа 30. Фоновые задачи. Hangfire.md`
- `course/Раздел 6. ASP Core/Практическая работа 31. Валидация. FluentValidation.md`
- `course/Раздел 6. ASP Core/Практическая работа 33. Логгирование. Serilog.md`
- Ревизия: П26–29, 32; Лекции 18–24.

- [ ] **Step 1: П30.** `Hangfire.MemoryStorage` → `Hangfire.InMemory --version 1.0.0`; убрать `using Hangfire.MemoryStorage;`; конфигурация `config.UseInMemoryStorage()`; версии Hangfire/Hangfire.AspNetCore 1.8.25.
- [ ] **Step 2: П31.** `FluentValidation.AspNetCore` (устарел) → `FluentValidation --version 12.1.1` + `FluentValidation.DependencyInjectionExtensions --version 12.1.1`; `AddFluentValidation(fv => …)` → `AddValidatorsFromAssemblyContaining<Program>()`; при необходимости дополнить пример классом-валидатором.
- [ ] **Step 3: П33.** Пины версий: `Serilog.AspNetCore --version 10.0.0`, `Serilog.Sinks.Console --version 6.1.1`, `Serilog.Sinks.File --version 7.0.0`; проверить код на .NET 10.
- [ ] **Step 4:** Ревизия П26–29, 32 и Л18–24: `Select-String -Pattern 'net[678]\.0|\.NET [678]|Visual Studio|Install-Package|Startup\.cs'` — устаревшее поправить (в т.ч. JWT-пакеты в П29, Blazor в П28).
- [ ] **Step 5:** Проверка: `Select-String -Path 'course\Раздел 6*' -Pattern 'MemoryStorage|AddFluentValidation|FluentValidation\.AspNetCore|Install-Package'` — 0.
- [ ] **Step 6:** Commit `docs: раздел 6 — Hangfire.InMemory, FluentValidation 12, Serilog 10`

### Task 7: Имена файлов и заголовки

- [ ] **Step 1:** `git mv "course\Раздел 5. Работа с базами данных. EF\Практическая работа 25. Уровни изоляции, стратегрии работы с конкурентным доступом. Оптимистическая и пессимистическая блокировки.md" "…стратегии работы…"`
- [ ] **Step 2:** `git mv "course\Раздел 5. Работа с базами данных. EF\Самостоятельная работа 5. Разработка система учета заказов.md" "…Разработка системы учета заказов.md"`
- [ ] **Step 3:** СР4 «Чтение и запиcь файлов большого объема» — проверить, нет ли латинской `c` в «запиcь»; если есть — переименовать в «запись».
- [ ] **Step 4:** Лекция 7: заголовок внутри файла `# Лекция: Делегаты…` → `# Лекция 7. Делегаты, события, лямбда-выражения, LINQ`; заголовки исправленных файлов привести в соответствие именам.
- [ ] **Step 5:** Сверка: заголовки «Лекция N», «Практическая работа N», «Самостоятельная работа N» совпадают с номерами файлов.
- [ ] **Step 6:** Commit `docs: исправлены имена и заголовки файлов`

### Task 8: README

**Files:** `readme.md` → `README.md`

- [ ] **Step 1:** `git mv readme.md README.md`.
- [ ] **Step 2:** Переписать по образцу mdk0102: описание курса и специальности 09.02.07; «## Текущий семестр» — плейсхолдеры (группы, даты, ссылки, порядок сдачи — без персональных данных); «## Требования к окружению» (.NET SDK 10 (`winget install Microsoft.DotNet.SDK.10`), VS Code + C# Dev Kit, Git, PostgreSQL 16, pgAdmin; Docker не используем); «## Структура репозитория»; «## Календарно-тематический план» — 25 лекций / 33 практики / 5 СР, имена согласованы с файлами.
- [ ] **Step 3:** Исправить расхождения (Л6 «Работа со строками», Л7 «Делегаты…», П3 «Управление потоком выполнения», П4 «Структуры данных», П33 «Логгирование. Serilog») и опечатки («foreeach», «стратегрии», «Разработка система…»).
- [ ] **Step 4:** Проверка: перечень совпадает с файлами (63 позиции); нет «Московая», «prep.scc», «ИП23», «t.me», «max.ru».
- [ ] **Step 5:** Commit `docs: README — структура курса, окружение и КТП 2026/27`

### Task 9: Экзамен

**Files:** `examen.md`

- [ ] **Step 1:** Проверить 25 заданий на устаревшее (net6/8, Visual Studio, старые API), вычитать формулировки.
- [ ] **Step 2:** Commit (если правки) `docs: экзаменационные задания — вычитка`

### Task 10: Эталонные проекты (вне репозитория)

**Path:** `C:\Users\prep\AppData\Local\Temp\opencode\mdk0104-ref\`

- [ ] **Step 1: ConsoleApp.** `dotnet new console -n AreaCalculator --framework net10.0`; `dotnet build`; `dotnet run`.
- [ ] **Step 2: EfShop.** Console + Npgsql 10.0.3 + Design 10.0.12; DbContext с `UseNpgsql(...shopdb...)`; `dotnet ef migrations add Initial`; `dotnet ef database update`; CRUD-прогон; зафиксировать точную команду scaffold для П18.
- [ ] **Step 3: DocsLib.** Open XML 3.5.1, EPPlus 8.7.0 (рабочая инициализация лицензии), iText7 9.7.0, PdfSharp 6.2.4 — минимальные примеры компилируются/работают.
- [ ] **Step 4: AspShop.** Minimal API + Serilog 10.0.0 (+sinks), FluentValidation 12.1.1 + DI, Hangfire 1.8.25 + InMemory; `dotnet build`; краткий `dotnet run`.
- [ ] **Step 5:** Зафиксировать результаты; при расхождениях поправить материалы задач 4–6 и закоммитить `docs: правки по итогам верификации на net10.0`.

### Task 11: Финальная верификация

- [ ] **Step 1:** grep-чек-лист по всему репо: `net6\.0|net7\.0|net8\.0`, `\.NET [678]`, `Visual Studio`, `Install-Package`, `UseSqlServer|Server=\.|Trusted_Connection`, `MemoryStorage`, `AddFluentValidation`, `FluentValidation\.AspNetCore` — допустимы только .gitignore-комментарии и исторические упоминания в сравнительных таблицах (проверить каждое).
- [ ] **Step 2:** README ↔ фактический список файлов (63) построчно.
- [ ] **Step 3:** У всех файлов non-zero размер; заголовки соответствуют шаблонам.
- [ ] **Step 4:** Эталонные проекты собираются (`dotnet build`), EfShop CRUD прогоняется.
- [ ] **Step 5:** Commit `docs: финальная верификация курса`

### Task 12: Merge и push

- [ ] **Step 1:** `git checkout master; git merge feature/actualize-2026`
- [ ] **Step 2:** Проверить `$env:GITHUB_TOKEN`/`$env:GH_TOKEN`; если нет — сообщить пользователю и остановиться.
- [ ] **Step 3:** `git push origin master`; при доступности — `git push gogs master`.
- [ ] **Step 4:** Итоговый отчёт: коммиты, число изменённых файлов, статус push.
