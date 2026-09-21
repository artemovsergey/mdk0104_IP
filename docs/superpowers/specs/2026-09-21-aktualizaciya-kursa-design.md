# Актуализация курса МДК 01.04 (mdk0104_IP) — дизайн

**Дата:** 2026-09-21
**Репозиторий:** https://github.com/artemovsergey/mdk0104_IP (локально: `C:\Users\prep\Desktop\Репозитории\mdk0104_IP`)
**Статус:** утверждено к реализации

## 1. Контекст

Репозиторий `mdk0104_IP` — курс «МДК 01.04 Системное программирование» (специальность 09.02.07 «Информационные системы и программирование», направление ИП).

Состав: 6 разделов, **25 лекций, 33 практические работы, 5 самостоятельных работ** (63 файла `course/**/*.md`), `tutorial/` (PDF: Основы Git, Tutorial for Markdown), `readme.md`, `examen.md` (25 вариантов задач на C#).

Содержание обновлялось 2025-10…11; задания экзамена переведены на C# в 2026-05. Материалы ссылаются на .NET 6/7/8 и Visual Studio; в аудиториях Visual Studio нет, актуальная платформа — .NET 10.

Соседний курс `mdk0102` актуализирован 2026-09-21: .NET 10, VS Code + C# Dev Kit, README с блоком «Текущий семестр» (плейсхолдеры), spec + план в `docs/superpowers/`. Данный дизайн повторяет процесс для `mdk0104_IP`.

## 2. Цель

Привести материалы курса в соответствие с 2026/27 учебным годом: .NET 10 (net10.0), VS Code + C# Dev Kit + `dotnet` CLI, актуальные версии NuGet-пакетов, PostgreSQL 16 в разделе EF Core, README без персональных и внутренних данных, устранение расхождений между README и файлами.

## 3. Область работ

**Входит:**

- Обновление версий платформы и окружения во всех материалах (.NET 10, VS Code, `dotnet` CLI, PostgreSQL 16).
- Актуализация команд установки пакетов и примеров кода (замена `Install-Package` на `dotnet add package`).
- Перевод примеров Раздела 5 (EF Core) с SQL Server на PostgreSQL (Npgsql).
- README: переименование в `README.md`, структура по образцу mdk0102, блок «Текущий семестр» с плейсхолдерами, синхронизация КТП с файлами, исправление опечаток и расхождений.
- Исправление имён файлов и заголовков (опечатки, пропущенная нумерация).
- Вычитка `examen.md` на устаревшие упоминания.
- Верификация примеров кода эталонными проектами на net10.0 (console, EF + PostgreSQL, ASP.NET Core, файловые библиотеки).

**Не входит:**

- Изменение состава курса (25/33/5) и добавление новых тем.
- Курс `mdk0104_IB` (отдельная задача).
- Замена библиотек (EPPlus, iText7, PdfSharp, Open XML SDK, Hangfire, Serilog остаются; обновляются версии и API).
- Docker и тестовые контейнеры (в колледже Docker не используется).
- Изменение `tutorial/` (PDF).

## 4. Целевое окружение

- .NET SDK 10.0.300 (LTS), целевая платформа `net10.0`.
- VS Code + C# Dev Kit + `dotnet` CLI (Visual Studio не используется как основная среда; .NET 10 не поддерживается старыми версиями VS).
- PostgreSQL 16 (локальная установка); строка подключения в примерах: `Host=localhost;Port=5432;Database=shopdb;Username=postgres;Password=postgres` с примечанием «подставьте свой пароль».
- Git, NuGet.

## 5. Целевые версии (проверено на nuget.org, 2026-09-21)

| Компонент | Версия |
|---|---|
| .NET SDK / net10.0 | 10.0.300 |
| Npgsql.EntityFrameworkCore.PostgreSQL | 10.0.3 |
| Microsoft.EntityFrameworkCore.Design / Tools | 10.0.12 |
| Hangfire / Hangfire.AspNetCore | 1.8.25 |
| Hangfire.InMemory (замена Hangfire.MemoryStorage) | 1.0.0 |
| FluentValidation / FluentValidation.DependencyInjectionExtensions | 12.1.1 |
| Serilog.AspNetCore | 10.0.0 |
| Serilog.Sinks.Console / Sinks.File | 6.1.1 / 7.0.0 |
| iText7 / PdfSharp | 9.7.0 / 6.2.4 |
| EPPlus / DocumentFormat.OpenXml | 8.7.0 / 3.5.1 |
| PostgreSQL | 16 |

## 6. Изменения по задачам

1. **Лекция 1** — .NET 5–10, LTS .NET 10, установка SDK, VS Code + C# Dev Kit.
2. **Раздел 1** — П1 (`net8.0` → `net10.0`, отладка через VS Code), П4 (решение через `dotnet new sln`), П5 и СР1 (`net6.0` → `net10.0`).
3. **Лекции 17, 20, 25** — VS Code вместо Visual Studio; публикация через `dotnet publish`.
4. **Раздел 4** — П13 (.NET 10), П14 Open XML 3.5.1, П15 EPPlus 8.7.0 (+ лицензия), П16 iText7 9.7.0 / PdfSharp 6.2.4, `dotnet add package`.
5. **Раздел 5** — все строки подключения и примеры перевести на PostgreSQL (Npgsql); П18 scaffold; П25 уровни изоляции PostgreSQL; установка PostgreSQL 16.
6. **Раздел 6** — П30 Hangfire.InMemory; П31 FluentValidation 12.1.1 + `AddValidatorsFromAssemblyContaining`; П33 Serilog 10.0.0; ревизия П26–29, 32 и лекций 18–24.
7. **Имена и заголовки** — `git mv` опечаток («стратегрии», «Разработка система…», «запиcь»), заголовок Лекции 7.
8. **README** — переименование, структура по образцу mdk0102, плейсхолдеры, КТП 25/33/5, расхождения и опечатки.
9. **Экзамен** — вычитка `examen.md`.
10. **Эталонные проекты** — ConsoleApp, EfShop (PostgreSQL), DocsLib, AspShop; фиксация рабочих примеров.
11. **Финальная верификация** — grep-чек-лист, сверка README ↔ 63 файла, сборка эталонов.
12. **Merge и push** — ветка `feature/actualize-2026` → `master`, push origin (+ gogs при доступности).

## 7. Принципы и ограничения

- Русский язык материалов и коммитов; правки точечные, существующий стиль и шаблоны сохраняются.
- Visual Studio не упоминается как среда (исключение — стандартные комментарии `.gitignore`).
- Git: `git add -A`; префикс `docs:`; commit после каждой задачи; push только при `GITHUB_TOKEN`/`GH_TOKEN`.
- Код в материалах должен компилироваться на net10.0; непроверенный код не вставляется.

## 8. Верификация

- Эталонные проекты в `C:\Users\prep\AppData\Local\Temp\opencode\mdk0104-ref`: ConsoleApp (П1), EfShop (П18–25: миграции + CRUD на локальном PostgreSQL), DocsLib (П14–16), AspShop (П26–33).
- grep-чек-лист: `net6.0`, `net7.0`, `net8.0`, `.NET 6/7/8`, `Visual Studio`, `Install-Package`, `UseSqlServer`, `Server=.`, `MemoryStorage`, `AddFluentValidation`, `FluentValidation.AspNetCore`.
- Сверка README с фактическими файлами (63 позиции); проверка заголовков; размеры файлов > 0.

## 9. Риски

- Пароль локального PostgreSQL может отличаться — проверяется при эталонных прогонах (в примерах — `Password=postgres`, с примечанием).
- EPPlus 8 сменил API лицензирования — проверяется в эталонном проекте, пример П15 правится по факту.
- Возможные ненайденные устаревшие места закрываются финальным grep-проходом.
