# Система управления библиотекой — готовый проект для отчета и публикации на GitHub

Проект разработан в рамках лабораторной работы №3: "Разработка структуры проекта информационной системы".
В этом репозитории содержится полностью рабочая базовая реализация системы управления библиотекой
с трехслойной архитектурой (DAL, BLL, API), а также диаграммы и инструкции по запуску и публикации на GitHub.

## Что в репозитории

Ключевые файлы и директории (корень: `library_system`):

- `api/` — Presentation Layer (REST API)
    - `endpoints.py` — HTTP endpoints (FastAPI)
- `bll/` — Business Logic Layer
    - `library_service.py` — основные операции (CRUD и выдача/возврат)
- `dal/` — Data Access Layer
    - `models.py` — модели SQLAlchemy
    - `database.py` — создание engine и сессий
- `main.py` — точка входа для запуска приложения
- `requirements.txt` — зависимости проекта
- `er_diagram.drawio` — файл диаграммы (DrawIO) для импорта/редактирования
- `er_diagram.puml` — PlantUML описание диаграммы (альтернатива)
- `generate_er_diagram_pydot.py` — скрипт генерации PNG диаграммы (при наличии graphviz)
- `REPORT.md` — полный отчёт по лабораторной работе
- `README.md` — этот файл

Сохраните также файл `library.db` после запуска, если хотите включить демонстрационные данные.

## ER-диаграмма (для вставки в отчет)

В репозитории есть `er_diagram.drawio`, который можно открыть в Draw.io (diagrams.net) и экспортировать в PNG/PDF:

1. Откройте https://app.diagrams.net/
2. File -> Open From -> Device -> выберите `er_diagram.drawio`

Альтернативно вы можете использовать `er_diagram.puml` (PlantUML) или `generate_er_diagram_pydot.py`.

Пример ER (содержимое диаграммы):

Entities и атрибуты:

- `books` (Книги): id (PK), title, isbn, author_id (FK), category_id (FK)
- `authors` (Авторы): id (PK), name
- `categories` (Категории): id (PK), name
- `readers` (Читатели): id (PK), name, email
- `loans` (Выдачи): id (PK), book_id (FK), reader_id (FK), loan_date, return_date

Связи:

- `authors 1 — * books` (автор пишет много книг)
- `categories 1 — * books` (категория содержит много книг)
- `books 1 — * loans` (книга может иметь много записей выдач)
- `readers 1 — * loans` (читатель может брать много книг)

## Быстрая инструкция по установке и запуску (Windows, PowerShell)

Рекомендуется работать в виртуальном окружении.

1) Откройте PowerShell и перейдите в папку проекта:

```powershell
cd "c:\Users\sopwa\OneDrive\Desktop\Skopin\Laba razrabotka 2\library_system"
```

2) Создайте виртуальное окружение и активируйте его:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Если PowerShell блокирует выполнение скриптов, разрешите выполнение для сессии:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
```

3) Установите зависимости:

```powershell
pip install -r requirements.txt
```

4) Инициализируйте базу данных и запустите приложение:

```powershell
python main.py
```

После запуска сервер доступен по адресу: http://127.0.0.1:8000

Откройте документацию API: http://127.0.0.1:8000/docs

Примечание: можно запускать через uvicorn напрямую (альтернативный вариант):

```powershell
python -m uvicorn api.endpoints:app --reload --host 127.0.0.1 --port 8000
```

## Примеры использования API

1) Добавить читателя (POST /readers/)

Request body (JSON):

```json
{ "name": "Иван Иванов", "email": "ivan@example.com" }
```

2) Добавить книгу (POST /books/)

```json
{ "title": "Программирование на Python", "author_id": 1, "category_id": 1, "isbn": "978-1-23456-789-7" }
```

3) Выдать книгу (POST /loans/)

```json
{ "book_id": 1, "reader_id": 1 }
```

4) Вернуть книгу (PUT /loans/{loan_id}/return)

## Как подготовить репозиторий для GitHub

Пример последовательности команд (в PowerShell):

```powershell
cd "c:\Users\sopwa\OneDrive\Desktop\Skopin\Laba razrabotka 2\library_system"
git init
echo ".venv/" > .gitignore
echo "library.db" >> .gitignore
git add .
git commit -m "Initial commit: library_system for lab 3"
# Затем создайте репозиторий на GitHub и добавьте remote
git remote add origin https://github.com/<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```

Обязательно включите в репозиторий `er_diagram.drawio` и/или экспортированную PNG/PDF версии диаграммы для отчёта.

## Структура проекта (полный список файлов)

```
library_system/
├── api/
│   ├── __init__.py
│   └── endpoints.py
├── bll/
│   ├── __init__.py
│   └── library_service.py
├── dal/
│   ├── __init__.py
│   ├── database.py
│   └── models.py
├── er_diagram.drawio
├── er_diagram.puml
├── generate_er_diagram_pydot.py
├── generate_er_diagram.py
├── main.py
├── requirements.txt
├── REPORT.md
└── README.md
```

## Примечания и рекомендации преподавателю

- В проекте используется SQLite для удобства — при необходимости можно заменить на PostgreSQL или MySQL, изменив `DATABASE_URL` в `dal/database.py`.
- Для развёртывания на сервере рекомендуется использовать `uvicorn` + `gunicorn` или контейнеризацию (Docker).

Если нужно, я могу:

- Сгенерировать PNG версии ER-диаграммы и добавить их в репозиторий
- Подготовить `requirements.txt` с зафиксированными версиями (если требуется)
- Подготовить краткую инструкцию для презентации (скриншоты, примеры запросов)

---

Автор: Ваша лабораторная работа №3 — система управления библиотекой

