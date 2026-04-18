# REST API

## Основная информация

**ФИО:** Dragonyafa
**Группа:** ИСП-233
**Дата:** 18.04.2026

## Описание работы

Реализованы CRUD-операции для ресурса «Задача» (Task), добавлены дополнительные маршруты поиска, фильтрации, сортировки и статистики. Освоены инструменты тестирования: Swagger UI и REST Client в VS Code. Настроена политика CORS для взаимодействия с фронтендом. Изучены правильные HTTP-статусы и формат ответов, а также принципы использования DTO для защиты API.

## Структура проекта
```
TaskApi/
├── Controllers/
│ └── TasksController.cs 
├── Models/
│ ├── TaskItem.cs 
│ ├── CreateTaskDto.cs
│ └── UpdateTaskDto.cs 
├── Properties/
│ └── launchSettings.json 
├── img/
├── .gitignore
├── .editorconfig
├── README.md
├── requests.http 
├── TaskApi.csproj
├── Program.cs 
└── appsettings.json
```


## Список всех реализованных маршрутов

| Метод | URL | Описание |
|-------|-----|-----------|
| GET | /api/tasks | Получить все задачи (опционально фильтр по `completed`) |
| GET | /api/tasks/{id} | Получить задачу по ID |
| POST | /api/tasks | Создать новую задачу (принимает `CreateTaskDto`) |
| PUT | /api/tasks/{id} | Полностью обновить задачу (принимает `UpdateTaskDto`) |
| DELETE | /api/tasks/{id} | Удалить задачу |
| PATCH | /api/tasks/{id}/complete | Переключить статус выполнения (`IsCompleted`) |
| GET | /api/tasks/search?query=... | Поиск задач по заголовку или описанию (без учёта регистра) |
| GET | /api/tasks/priority/{level} | Получить задачи с указанным приоритетом (`Low`/`Normal`/`High`) |
| GET | /api/tasks/stats | Статистика: всего задач, выполнено, не выполнено, по приоритетам |
| GET | /api/tasks/sorted?by=...&desc=... | Сортировка по полю (`id`/`title`/`priority`/`createdat`) с направлением |

-----
## ASP.NET Core Controller-based API

| Аспект | ASP.NET Core Controllers |
|--------|--------------------------|
| Маршруты | `[HttpGet]` атрибут над методом |
| Группировка маршрутов | Класс-контроллер |
| Базовый URL | `[Route("api/[controller]")]` |
| Параметр пути | `(int id)` — параметр метода |
| Параметр запроса | `[FromQuery] bool? completed` |
| Тело запроса | `[FromBody] CreateTaskDto dto` |
| Ответ 200 | `return Ok(data)` |
| Ответ 201 | `return CreatedAtAction(...)` |
| Ответ 404 | `return NotFound(...)` |
| Ответ 204 | `return NoContent()` |
| Типизация данных | Строгая (C#) |
| Документация | Swagger — устанавливается отдельно |

## Главные выводы

1. REST — не протокол, а архитектурный стиль. Те же принципы работают с
любым языком и фреймворком
2. Контроллер в ASP.NET Core = Router в Express, только с автоматической
документацией и строгой типизацией.
3. DTO защищает API от некорректных данных: клиент передаёт только то, что
сервер разрешает принять.
4. Swagger UI позволяет тестировать API без Postman и без написания тестового
JavaScript-кода.
5. Правильные HTTP-статусы — часть «контракта» API. Клиент должен
понимать, что произошло, по коду ответа