# Auto Forge

Next.js App Router, TypeScript, Zod и Zustand. Архитектура — упрощённый FSD:
`app`, `widgets`, `entities`, `shared`. Пользовательские сценарии размещаются
в `widgets`, а предметные сущности — в `entities`.

## Запуск

```bash
npm ci
npm run dev
```

Приложение доступно на [localhost:3000](http://localhost:3000).

## Структура

```text
src/
├── app/                    # App Router и сборка приложения
│   ├── _providers/         # Провайдеры приложения
│   ├── favicon.ico
│   ├── globals.css         # Глобальные стили
│   ├── layout.tsx          # Корневой layout
│   ├── page.module.css     # Стили текущей стартовой страницы
│   └── page.tsx            # Главный маршрут
├── widgets/                # Крупные UI-блоки и пользовательские сценарии
├── entities/               # Предметные сущности
└── shared/                 # Общий код
    ├── api/                # Общие средства работы с API
    ├── config/             # Общая конфигурация
    ├── lib/                # Утилиты и общие хуки
    └── ui/                 # Общие UI-компоненты
```

Пустые папки сохранены в Git через `.gitkeep`. При появлении кода эти файлы можно
удалить. Статические файлы, доступные по URL, размещаются в `public/`.

### App

`src/app` содержит маршруты, layouts, metadata, обработчики маршрутов и композицию
widgets. Общие провайдеры размещаются в `_providers`; это приватная папка App Router.
Предметная логика размещается в `entities`, пользовательские сценарии — в `widgets`.

### Widgets

`src/widgets` содержит самостоятельные блоки интерфейса и пользовательские сценарии.
Widgets объединяют компоненты и данные из `entities` и используют общий код `shared`.

### Entities

`src/entities` содержит предметные сущности: их типы, Zod-схемы, Zustand stores,
запросы и UI-компоненты. Каждая сущность получает отдельную папку.

### Структура widget или entity

Сегменты создаются по мере надобности:

```text
widgets/ или entities/
└── <name>/
    ├── api/                # Запросы и серверные операции
    ├── lib/                # Внутренние утилиты
    ├── model/              # Типы, Zod-схемы, Zustand stores и логика
    ├── ui/                 # Компоненты и интерфейсы
    └── index.ts            # Публичные экспорты
```

### Shared

`shared` содержит код, пригодный для разных предметных областей. Типы, схемы,
запросы и компоненты конкретной сущности или widget хранятся в их собственных папках.

### Импорты

- Направление зависимостей: `app → widgets → entities → shared`.
- Каждый слой может напрямую использовать любой слой ниже него.
- Widgets используют свой внутренний код, `entities` и `shared`.
- Entities используют свой внутренний код и `shared`.
- Элементы одного слоя не импортируют друг друга; их композиция выполняется выше.
- Внешний код импортирует widget или entity через `@/widgets/<name>` или
  `@/entities/<name>` и соответствующий `index.ts`.
- Внутри widget или entity используются относительные импорты между сегментами.
- `shared` не импортирует `app`, `widgets` или `entities`.
- Для общего кода используются адресные импорты, например `@/shared/lib/...`.

Алиас `@/*` указывает на `src/*`. Компоненты остаются серверными по умолчанию;
`'use client'` добавляется в файлы, которым нужны клиентские hooks или browser API.

## Команды

| Команда                | Назначение               |
| ---------------------- | ------------------------ |
| `npm run dev`          | Сервер разработки        |
| `npm run build`        | Production-сборка        |
| `npm run start`        | Запуск Production-сборки |
| `npm run lint`         | Проверка ESLint          |
| `npm run format`       | Форматирование Prettier  |
| `npm run format:check` | Проверка форматирования  |

## Документация

- [Структура проекта Next.js](https://nextjs.org/docs/app/getting-started/project-structure)
- [Слои FSD](https://feature-sliced.design/docs/reference/layers)
- [Публичный API FSD](https://feature-sliced.design/docs/reference/public-api)
