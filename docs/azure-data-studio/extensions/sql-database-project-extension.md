---
title: Расширение проектов Баз данных SQL
description: Установка и использование расширения "Проекты Баз данных SQL" для Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 12/15/2020
ms.openlocfilehash: 58375b6512c259d07dd25fb10d539421162997c0
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298727"
---
# <a name="sql-database-projects-extension-preview"></a>Расширение "Проекты Баз данных SQL" (предварительная версия)

Расширение "Проекты Баз данных SQL" (предварительная версия) предназначено для разработки баз данных SQL в среде разработки на основе проектов. 


## <a name="features"></a>Компоненты

- Создание нового пустого проекта или проекта на основе подключенной базы данных.
- Открытие проекта, созданного ранее в [Azure Data Studio](sql-database-project-extension-getting-started.md) или в [SQL Server Data Tools](../../ssdt/sql-server-data-tools.md).
- Редактирование проекта путем добавления или удаления его объектов (таблиц, представлений, хранимых процедур) и пользовательских сценариев.
- Упорядочение файлов и сценариев в папках.
- Добавление ссылок на системные базы данных или пользовательский пакет DACPAC.
- Создание одного проекта.
- Развертывание одного проекта.
- Загрузка сведений о подключении (проверка подлинности SQL Windows) и переменных SQLCMD из профиля развертывания.

Просмотрите это короткое десятиминутное видео, чтобы ознакомиться с расширением "Проекты Баз данных SQL" в Azure Data Studio:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Build-SQL-Database-Projects-Easily-in-Azure-Data-Studio/player?WT.mc_id=dataexposed-c9-niner]

## <a name="installation"></a>Установка

1. Откройте диспетчер расширений, чтобы получить доступ к расширениям.  Для этого щелкните значок расширений или выберите пункт **Расширения** в меню **Представление**.
2. Найдите расширение *Проекты Баз данных SQL*, целиком или частично введя имя расширения в поле поиска. Выберите доступное расширение и просмотрите сведения о нем.

   ![Установка расширения](media/sql-database-projects-extension/install-database-projects.png)

3. Выберите нужное расширение и **установите** его.
4. Выберите **Перезагрузить**, чтобы включить расширение (требуется только при первой установке расширения).
5. Нажмите значок файлов на панели действий или выберите **Обозреватель** в меню **Вид**. Доступно новое представление **Проекты**.

   > [!NOTE]
   > Пакет SDK для .NET Core необходим для функциональности сборки проекта. Если его не удастся обнаружить с помощью расширения, вам будет предложено установить пакет SDK для .NET Core.  Пакет SDK для .NET Core (версии 3.1 и выше) можно загрузить и установить по ссылке [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

   > [!NOTE]
   > Рекомендуется установить расширение [Сравнение схем](schema-compare-extension.md) наряду с расширением "Проекты Баз данных SQL", чтобы получить полноценный набор функций.

## <a name="net-core-sdk"></a>Пакет SDK для .NET Core
Пакет SDK для .NET Core необходим для функциональности сборки проекта. Если его не удастся обнаружить с помощью расширения, вам будет предложено установить пакет SDK для .NET Core.  Пакет SDK для .NET Core (версии 3.1 и выше) можно загрузить и установить по ссылке [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

Предыдущие установки пакета SDK для .NET Core могут иметь версию, которая ниже минимально необходимой и не поддерживает использование расширения "Проекты Баз данных SQL".  Если вы хотите [просмотреть установленные сейчас версии](https://docs.microsoft.com/dotnet/core/install/how-to-detect-installed-versions) пакета SDK для .NET, откройте терминал и выполните следующую команду.

```dotnetcli
dotnet --list-sdks
```

При наличии неподдерживаемых версий пакета SDK для .NET Core могут выдаваться сообщения об ошибках следующего вида:
- `error MSB4018: The "SqlBuildTask" task failed unexpectedly.`
- ` error MSB4018: System.TypeInitializationException: The type initializer for 'SqlSchemaModelStaticState' threw an exception. ---> System.IO.FileNotFoundException: Could not load file or assembly 'System.Runtime, Version=4.2.2.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'. The system cannot find the file specified. [c:\Users\ .sqlproj]_` (Приведенный по ссылке несуществующий файл имеет закрывающую квадратную скобку без парной открывающей.)

## <a name="known-limitations"></a>Известные ограничения

- Сейчас загрузка файлов в виде ссылки во вьюлет Azure Data Studio не поддерживается, однако файлы будут загружены в верхний уровень дерева, и сборка будет включать эти файлы, как ожидалось.
- Объекты SQLCLR в проекте не поддерживаются версией DacFx для .NET Core.
- Задачи (сборка или публикация) не определяются пользователем.
- Публикация целевых объектов, определенных DacFx.
- Поддержка среды WSL ограничена.

## <a name="workspace"></a>Рабочая область
Проекты базы данных SQL в Azure Data Studio содержатся в логической рабочей области.  Рабочая область служит для управления папками, видимыми в панели обозревателя, а также проектами, видимыми в области "Проект". Добавлять и удалять проекты рабочей области можно с помощью области "Проекты" в интерфейсе Azure Data Studio. При необходимости параметры рабочей области можно изменить вручную в файле `.code-workspace`.

В приведенном ниже примере файла `.code-workspace` массив `folders` содержит перечисление всех папок, добавленных в панель обозревателя, а массив `dataworkspace.projects` в `settings` — список всех проектов SQL, включенных в область "Проекты".

```json
{
    "folders": [
        {
            "path": "."
        },
        {
            "name": "WideWorldImportersDW",
            "path": "..\\WideWorldImportersDW"
        }
    ],
    "settings": {
        "dataworkspace.projects": [
            "AdventureWorksLT.sqlproj",
            "..\\WideWorldImportersDW\\WideWorldImportersDW.sqlproj"
        ]
    }
}
```

## <a name="next-steps"></a>Дальнейшие действия

- [Начало работы с расширением "Проекты Баз данных SQL"](sql-database-project-extension-getting-started.md)
- [Сборка и публикация проекта с помощью расширения "Проекты Баз данных SQL" для Azure Data Studio](sql-database-project-extension-build.md)
