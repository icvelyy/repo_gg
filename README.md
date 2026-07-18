# Мониторинг каталога в облаке
Программа представляет собой алгоритмы обхода дерева каталогов публичной папки Яндекс.Диска c целью быстрого выявления изменений и внесения изменения в облако через консоль посредством наличия OAuth-токена. 

Набор консольных инструментов для работы с Яндекс.Диском через официальный REST API:

1. ```sh meta.py``` — делает локальные снимки содержимого публичной папки на Диске и показывает, что изменилось между двумя запусками (новые/удалённые/изменённые файлы и папки).

2. ```sh YAtoken.py``` - хранит в себе OAuth-токен.

3. ```sh addToServer.py``` - скрипт для внесения изменений в директорию облака Яндекс.Диска (удаление/добавление/изменение файлов) посредством использования OAuth-токена.

## Содержание
- [Технологии](#технологии)
- [Использование](#использование)
- [Источники](#источники)

## Технологии
- Бэкэнд: [Python](https://www.python.org/)
- Инструменты: [Документация API Яндекс.Диска](https://yandex.ru/dev/disk-api/doc/ru/)

## Использование
Перед использованием следует отключить VPN.

Скачиваем репозиторий:

```sh
git clone <ссылка транстелесофт>
```

Устанавливаем зависимости и открываем ```sh meta.py ```:
```sh
cd <путь\до\папки\с\репозиторием>
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python meta.py
```

![картинка с деревом папок](https://github.com/{username}/{repository}/raw/{branch}/{path}/image.png)

На картинке представлена исходная директория папки на Яндекс.Диске

![картинка с лимитами](https://github.com/{username}/{repository}/raw/{branch}/{path}/image.png)

Здесь представлена таблица с поведением работы программы при разных значениях limit. Как видим, limit = 35 является самым эффективным по времени выполнения кода и по количеству произведенных API запросов.

Запускаем скрипт ```sh meta.py``` и вводим limit = 35.
```sh
введите значение limit:
35
limit = 35

4.9750947 сек. - время выполнения кода

первый локальный снимок сохранен в файл 'first_screen.json'

дерево элементов на диске:

/practice/
├── data/
│   ├── backups/
│   │   └── weekly/
│   │       ├── backup1.zip
│   │       └── backup2.zip
│   ├── dataset.csv
│   └── nonfig.json
├── docs/
│   ├── archive/
│   │   ├── old_report_v1.docx
│   │   └── old_report_v2.docx
│   ├── documentation/
│   │   └── Корелляционные плеяды.pdf
│   ├── notes.txt
│   └── report.docx
├── images/
│   ├── test/
│   ├── 72d5c648-21d4-4051-b190-1e440804e661.png
│   ├── be262ab7-2682-442f-8c10-8f9b4577cb0a.png
│   └── be262ab7-4382-442f-9c10-8f9b4577cb0a.png
└── misc/
    ├── file2.txt
    ├── file3.txt
    ├── file4.txt
    ├── file5.txt
    ├── file6.txt
    ├── file7.txt
    ├── file8.txt
    └── Readme.md
```

В файл ```sh first_screen.json``` сохраняются метаданные директории ```sh /practice```. Файлам присущи такие характеристики, как ```sh name```, ```sh type```, ```sh modified```, ```sh size```, ```sh md5```; папкам - ```sh name```, ```sh type```, ```sh modified```. 

Содержимое ```sh first_screen.json```:

``` sh
{
    "/data/backups/weekly/backup1.zip": {
        "name": "backup1.zip",
        "type": "file",
        "modified": "2026-07-17T10:46:20+00:00",
        "size": 201,
        "md5": "d0f3dfa35143696096f62f94b353d4b4"
    },
    "/data/backups/weekly/backup2.zip": {
        "name": "backup2.zip",
        "type": "file",
        "modified": "2026-07-17T10:46:21+00:00",
        "size": 201,
        "md5": "99b675856be82cd9271a1aa0f4819fb4"
    },
    "/data/dataset.csv": {
        "name": "dataset.csv",
        "type": "file",
        "modified": "2026-07-17T10:40:37+00:00",
        "size": 61,
        "md5": "9913544b9a0348d2df8b50c5b4a5d778"
    },
    "/data/nonfig.json": {
        "name": "nonfig.json",
        "type": "file",
        "modified": "2026-07-17T16:06:50+00:00",
        "size": 59,
        "md5": "a1b970bab14f2bfa26a3da33a4b9b450"
    },
    "/docs/archive/old_report_v1.docx": {
        "name": "old_report_v1.docx",
        "type": "file",
        "modified": "2026-07-17T10:45:19+00:00",
        "size": 13546,
        "md5": "e418683d53d0c5db4d6fa4e54ada4f31"
    },
    "/docs/archive/old_report_v2.docx": {
        "name": "old_report_v2.docx",
        "type": "file",
        "modified": "2026-07-17T10:45:19+00:00",
        "size": 13522,
        "md5": "8ba090060bea11069b9443ac7581deea"
    },
    "/docs/documentation/Корелляционные плеяды.pdf": {
        "name": "Корелляционные плеяды.pdf",
        "type": "file",
        "modified": "2026-07-17T10:44:57+00:00",
        "size": 357074,
        "md5": "430b719c9074f4cf16d7e4a9834b68fe"
    },
    "/docs/notes.txt": {
        "name": "notes.txt",
        "type": "file",
        "modified": "2026-07-17T15:11:47+00:00",
        "size": 198,
        "md5": "3402bb1b3d257c0763be403e8ba9db80"
    },
    "/docs/report.docx": {
        "name": "report.docx",
        "type": "file",
        "modified": "2026-07-17T15:11:57+00:00",
        "size": 13634,
        "md5": "e446bc28685f859f2ccbee37db9aadde"
    },
    "/images/72d5c648-21d4-4051-b190-1e440804e661.png": {
        "name": "72d5c648-21d4-4051-b190-1e440804e661.png",
        "type": "file",
        "modified": "2026-07-17T10:42:53+00:00",
        "size": 253130,
        "md5": "e45d95bd8c93ef05fac672df0cf846c9"
    },
    "/images/be262ab7-2682-442f-8c10-8f9b4577cb0a.png": {
        "name": "be262ab7-2682-442f-8c10-8f9b4577cb0a.png",
        "type": "file",
        "modified": "2026-07-17T10:42:52+00:00",
        "size": 11070,
        "md5": "9ebc1bee04e5382a824d6c777e99afd2"
    },
    "/images/be262ab7-4382-442f-9c10-8f9b4577cb0a.png": {
        "name": "be262ab7-4382-442f-9c10-8f9b4577cb0a.png",
        "type": "file",
        "modified": "2026-07-17T10:42:52+00:00",
        "size": 132729,
        "md5": "8baa5b8b45a5305ff47274598aa5468e"
    },
    "/misc/Readme.md": {
        "name": "Readme.md",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 93,
        "md5": "c66d14947e3ddd36b4b7c8644b6b9976"
    },
    "/misc/file2.txt": {
        "name": "file2.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 49,
        "md5": "7627f3479251b6e5fd10f8a721e5b326"
    },
    "/misc/file3.txt": {
        "name": "file3.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 49,
        "md5": "7321337c365326e125dd224bcbe94a79"
    },
    "/misc/file4.txt": {
        "name": "file4.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 49,
        "md5": "cbcc2856752568efb353d31c4d783eb0"
    },
    "/misc/file5.txt": {
        "name": "file5.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 49,
        "md5": "fb50e7b1429e3dc51c78335957bb53a8"
    },
    "/misc/file6.txt": {
        "name": "file6.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:06+00:00",
        "size": 49,
        "md5": "793c62e55b453f2c7f6b59a55c39b809"
    },
    "/misc/file7.txt": {
        "name": "file7.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:05+00:00",
        "size": 49,
        "md5": "8d20266a01d756d50410b9f0b6a6c278"
    },
    "/misc/file8.txt": {
        "name": "file8.txt",
        "type": "file",
        "modified": "2026-07-17T14:41:05+00:00",
        "size": 49,
        "md5": "bf9316696065092a515e5bc4692f4860"
    },
    "/data": {
        "name": "data",
        "type": "dir",
        "modified": "2026-07-17T10:39:55+00:00"
    },
    "/data/backups": {
        "name": "backups",
        "type": "dir",
        "modified": "2026-07-17T10:40:26+00:00"
    },
    "/data/backups/weekly": {
        "name": "weekly",
        "type": "dir",
        "modified": "2026-07-17T10:46:12+00:00"
    },
    "/docs": {
        "name": "docs",
        "type": "dir",
        "modified": "2026-07-17T10:40:00+00:00"
    },
    "/docs/archive": {
        "name": "archive",
        "type": "dir",
        "modified": "2026-07-17T17:10:23+00:00"
    },
    "/docs/documentation": {
        "name": "documentation",
        "type": "dir",
        "modified": "2026-07-17T20:51:07+00:00"
    },
    "/images": {
        "name": "images",
        "type": "dir",
        "modified": "2026-07-17T10:40:08+00:00"
    },
    "/images/test": {
        "name": "test",
        "type": "dir",
        "modified": "2026-07-17T17:08:58+00:00"
    },
    "/misc": {
        "name": "misc",
        "type": "dir",
        "modified": "2026-07-17T14:40:34+00:00"
    }
}
```

Далее для того, чтобы второй снимок был отличен от первого, внесем изменения в директорию ```sh /practice```. Для этого воспользуемся скриптом ```sh addToServer.py```. Он представляет собой 5 операций на выбор по работе с файлами/папками, описанные в результате выполнения кода ниже. Для примера создадим папку ```sh /practice/new_folder```:

```sh 
    Выберите действие (1-6):
    1 - Создать новую папку
    2 - Удалить файл/папку
    3 - Загрузить файл
    4 - Поменять расположение папки/файла
    5 - Переименовать файл/папку
    6 - Выход из программы
    1
    введите путь новой папки:
    /practice/new_folder
    дерево элементов на диске:

    /practice/
    ├── data/
    │   ├── backups/
    │   │   └── weekly/
    │   │       ├── backup1.zip
    │   │       └── backup2.zip
    │   ├── dataset.csv
    │   └── nonfig.json
    ├── docs/
    │   ├── archive/
    │   │   ├── old_report_v1.docx
    │   │   └── old_report_v2.docx
    │   ├── documentation/
    │   │   └── Корелляционные плеяды.pdf
    │   ├── notes.txt
    │   └── report.docx
    ├── images/
    │   ├── test/
    │   ├── 72d5c648-21d4-4051-b190-1e440804e661.png
    │   ├── be262ab7-2682-442f-8c10-8f9b4577cb0a.png
    │   └── be262ab7-4382-442f-9c10-8f9b4577cb0a.png
    ├── misc/
    │   ├── file2.txt
    │   ├── file3.txt
    │   ├── file4.txt
    │   ├── file5.txt
    │   ├── file6.txt
    │   ├── file7.txt
    │   ├── file8.txt
    │   └── Readme.md
    └── new_folder/ #созданная папка
```
И переместим папку ```sh /test``` из ```sh /practice/images/test``` в ```sh /practice/docs/archive/test```

```sh
    Выберите действие (1-6):
    1 - Создать новую папку
    2 - Удалить файл/папку
    3 - Загрузить файл
    4 - Поменять расположение папки/файла
    5 - Переименовать файл/папку
    6 - Выход из программы
    4
    введите путь к файлу:
    /practice/images/test
    введите новый путь к файлу/папке:
    /practice/docs/archive/test
    дерево элементов на диске:

    /practice/
    ├── data/
    │   ├── backups/
    │   │   └── weekly/
    │   │       ├── backup1.zip
    │   │       └── backup2.zip
    │   ├── dataset.csv
    │   └── nonfig.json
    ├── docs/
    │   ├── archive/
    │   │   ├── test/ #новое расположение папки /test
    │   │   ├── old_report_v1.docx
    │   │   └── old_report_v2.docx
    │   ├── documentation/
    │   │   └── Корелляционные плеяды.pdf
    │   ├── notes.txt
    │   └── report.docx
    ├── images/ #вложенной папки /test нет
    │   ├── 72d5c648-21d4-4051-b190-1e440804e661.png
    │   ├── be262ab7-2682-442f-8c10-8f9b4577cb0a.png
    │   └── be262ab7-4382-442f-9c10-8f9b4577cb0a.png
    ├── misc/
    │   ├── file2.txt
    │   ├── file3.txt
    │   ├── file4.txt
    │   ├── file5.txt
    │   ├── file6.txt
    │   ├── file7.txt
    │   ├── file8.txt
    │   └── Readme.md
    └── new_folder/
```

Стоит подметить, что при выполнении подобных операций редактирования важно прописывать весь путь к папке/файлу, начиная с ```sh /practice``` и заканчивая тем элементом, который нужно отредактировать.

Теперь возвращаемся к ```sh meta.py```. Запустим код еще раз с тем же параметром limit = 35.

```sh 
    введите значение limit:
    35
    limit = 35

    Удалённые файлы: {}
    Удалённые папки: {'/images/test': {'name': 'test', 'type': 'dir', 'modified': '2026-07-17T17:08:58+00:00'}}
    Новые файлы: {}
    Новые папки: {'/new_folder': {'name': 'new_folder', 'type': 'dir', 'modified': '2026-07-17T23:13:09+00:00'}, '/docs/archive/test': {'name': 'test', 'type': 'dir', 'modified': '2026-07-17T23:37:16+00:00'}}
    Изменённые файлы: {}
    Изменённые папки: {'/images', '/docs/archive', '/docs'}
    Количество осуществленных API-запросов: 11
    5.949321100000361 сек. - время выполнения кода

    второй локальный снимок сохранен в файл 'second_screen.json'

    наглядные изменения сохранены в файл 'changes.json'
```

При повторном запуске ```sh meta.py``` при изменениях в директории ```sh /practice``` скрипт зафиксировал изменения, произведенные нами с помощью ```sh addToServer.py```. Содержимое файлов ```sh changes.json``` и ```sh second_screen.json```:

```sh 

    changes.json:

    {
        "Удалённые файлы": {},
        "Удалённые папки": {
            "/images/test": {
                "name": "test",
                "type": "dir",
                "modified": "2026-07-17T17:08:58+00:00"
            }
        },
        "Новые файлы": {},
        "Новые папки": {
            "/new_folder": {
                "name": "new_folder",
                "type": "dir",
                "modified": "2026-07-17T23:13:09+00:00"
            },
            "/docs/archive/test": {
                "name": "test",
                "type": "dir",
                "modified": "2026-07-17T23:37:16+00:00"
            }
        },
        "Изменённые файлы": {},
        "Изменённые папки": [
            "/images",
            "/docs/archive",
            "/docs"
        ],
        "Количество осуществленных API-запросов": 11
    }

    second_screen.json:

    {
        "/data/backups/weekly/backup1.zip": {
            "name": "backup1.zip",
            "type": "file",
            "modified": "2026-07-17T10:46:20+00:00",
            "size": 201,
            "md5": "d0f3dfa35143696096f62f94b353d4b4"
        },
        "/data/backups/weekly/backup2.zip": {
            "name": "backup2.zip",
            "type": "file",
            "modified": "2026-07-17T10:46:21+00:00",
            "size": 201,
            "md5": "99b675856be82cd9271a1aa0f4819fb4"
        },
        "/data/dataset.csv": {
            "name": "dataset.csv",
            "type": "file",
            "modified": "2026-07-17T10:40:37+00:00",
            "size": 61,
            "md5": "9913544b9a0348d2df8b50c5b4a5d778"
        },
        "/data/nonfig.json": {
            "name": "nonfig.json",
            "type": "file",
            "modified": "2026-07-17T16:06:50+00:00",
            "size": 59,
            "md5": "a1b970bab14f2bfa26a3da33a4b9b450"
        },
        "/docs/archive/old_report_v1.docx": {
            "name": "old_report_v1.docx",
            "type": "file",
            "modified": "2026-07-17T10:45:19+00:00",
            "size": 13546,
            "md5": "e418683d53d0c5db4d6fa4e54ada4f31"
        },
        "/docs/archive/old_report_v2.docx": {
            "name": "old_report_v2.docx",
            "type": "file",
            "modified": "2026-07-17T10:45:19+00:00",
            "size": 13522,
            "md5": "8ba090060bea11069b9443ac7581deea"
        },
        "/docs/documentation/Корелляционные плеяды.pdf": {
            "name": "Корелляционные плеяды.pdf",
            "type": "file",
            "modified": "2026-07-17T10:44:57+00:00",
            "size": 357074,
            "md5": "430b719c9074f4cf16d7e4a9834b68fe"
        },
        "/docs/notes.txt": {
            "name": "notes.txt",
            "type": "file",
            "modified": "2026-07-17T15:11:47+00:00",
            "size": 198,
            "md5": "3402bb1b3d257c0763be403e8ba9db80"
        },
        "/docs/report.docx": {
            "name": "report.docx",
            "type": "file",
            "modified": "2026-07-17T15:11:57+00:00",
            "size": 13634,
            "md5": "e446bc28685f859f2ccbee37db9aadde"
        },
        "/images/72d5c648-21d4-4051-b190-1e440804e661.png": {
            "name": "72d5c648-21d4-4051-b190-1e440804e661.png",
            "type": "file",
            "modified": "2026-07-17T10:42:53+00:00",
            "size": 253130,
            "md5": "e45d95bd8c93ef05fac672df0cf846c9"
        },
        "/images/be262ab7-2682-442f-8c10-8f9b4577cb0a.png": {
            "name": "be262ab7-2682-442f-8c10-8f9b4577cb0a.png",
            "type": "file",
            "modified": "2026-07-17T10:42:52+00:00",
            "size": 11070,
            "md5": "9ebc1bee04e5382a824d6c777e99afd2"
        },
        "/images/be262ab7-4382-442f-9c10-8f9b4577cb0a.png": {
            "name": "be262ab7-4382-442f-9c10-8f9b4577cb0a.png",
            "type": "file",
            "modified": "2026-07-17T10:42:52+00:00",
            "size": 132729,
            "md5": "8baa5b8b45a5305ff47274598aa5468e"
        },
        "/misc/Readme.md": {
            "name": "Readme.md",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 93,
            "md5": "c66d14947e3ddd36b4b7c8644b6b9976"
        },
        "/misc/file2.txt": {
            "name": "file2.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 49,
            "md5": "7627f3479251b6e5fd10f8a721e5b326"
        },
        "/misc/file3.txt": {
            "name": "file3.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 49,
            "md5": "7321337c365326e125dd224bcbe94a79"
        },
        "/misc/file4.txt": {
            "name": "file4.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 49,
            "md5": "cbcc2856752568efb353d31c4d783eb0"
        },
        "/misc/file5.txt": {
            "name": "file5.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 49,
            "md5": "fb50e7b1429e3dc51c78335957bb53a8"
        },
        "/misc/file6.txt": {
            "name": "file6.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:06+00:00",
            "size": 49,
            "md5": "793c62e55b453f2c7f6b59a55c39b809"
        },
        "/misc/file7.txt": {
            "name": "file7.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:05+00:00",
            "size": 49,
            "md5": "8d20266a01d756d50410b9f0b6a6c278"
        },
        "/misc/file8.txt": {
            "name": "file8.txt",
            "type": "file",
            "modified": "2026-07-17T14:41:05+00:00",
            "size": 49,
            "md5": "bf9316696065092a515e5bc4692f4860"
        },
        "/data": {
            "name": "data",
            "type": "dir",
            "modified": "2026-07-17T10:39:55+00:00"
        },
        "/data/backups": {
            "name": "backups",
            "type": "dir",
            "modified": "2026-07-17T10:40:26+00:00"
        },
        "/data/backups/weekly": {
            "name": "weekly",
            "type": "dir",
            "modified": "2026-07-17T10:46:12+00:00"
        },
        "/docs": {
            "name": "docs",
            "type": "dir",
            "modified": "2026-07-17T10:40:00+00:00"
        },
        "/docs/archive": {
            "name": "archive",
            "type": "dir",
            "modified": "2026-07-17T17:10:23+00:00"
        },
        "/docs/archive/test": {
            "name": "test",
            "type": "dir",
            "modified": "2026-07-17T23:37:16+00:00"
        },
        "/docs/documentation": {
            "name": "documentation",
            "type": "dir",
            "modified": "2026-07-17T20:51:07+00:00"
        },
        "/images": {
            "name": "images",
            "type": "dir",
            "modified": "2026-07-17T10:40:08+00:00"
        },
        "/misc": {
            "name": "misc",
            "type": "dir",
            "modified": "2026-07-17T14:40:34+00:00"
        },
        "/new_folder": {
            "name": "new_folder",
            "type": "dir",
            "modified": "2026-07-17T23:13:09+00:00"
        }
    }
```

Как видим, алгоритм, реализованный в ```sh meta.py``` позволяет быстро мониторить изменения на Диске, а ```sh addToServer.py``` позволяет вносить изменения в папки/файлы для тестирования мониторинга изменений. 

## Источники
Документация API Яндекс.Диск - https://yandex.ru/dev/disk-api/doc/ru/