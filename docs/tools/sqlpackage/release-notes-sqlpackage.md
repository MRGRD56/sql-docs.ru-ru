---
title: Заметки о выпуске DacFx и SqlPackage
description: Заметки о выпуске Microsoft sqlpackage.
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: ef52bf5f98b948312dcc36acfd438cd9281b25dd
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890684"
---
# <a name="release-notes-for-sqlpackageexe"></a>Заметки о выпуске SqlPackage.exe

**[Скачать последнюю версию](sqlpackage-download.md)**

В этой статье перечислены функции и исправления, предоставляемые выпущенными версиями SqlPackage.exe.

## <a name="187-sqlpackage"></a>sqlpackage 18.7

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2157201)|10 марта 2021 г.|18,7|15.0.5084.2|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2157203)|10 марта 2021 г.| 18,7|15.0.5084.2|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2157202)|10 марта 2021 г.| 18,7|15.0.5084.2|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2157302)|10 марта 2021 г.| 18,7|15.0.5084.2|

### <a name="features"></a>Компоненты
| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Извлечение больших данных из службы хранилища Azure и публикация их в ней. Дополнительные сведения см. в разделе [SqlPackage для больших данных](sqlpackage-for-azure-synapse-analytics.md). |
| Azure Synapse Analytics | Поддержка безопасности на уровне строк (встроенная функция с табличным значением, политика безопасности, предикат безопасности)  |
| Azure Synapse Analytics | Поддержка классификации рабочих нагрузок |
| SQL Azure для пограничных вычислений | Поддержка заданий внешней потоковой передачи |
| SQL Azure для пограничных вычислений | Добавлены параметры таблицы и базы данных для хранения данных. |
| Импорт | Добавлено 2 новых свойства параметров индекса для операции импорта. *DisableIndexesForDataPhase* (отключение индексов перед импортом данных в SQL Server, значение по умолчанию — true) и *RebuildIndexesOfflineForDataPhase* (перестроение индексов в автономном режиме после импорта данных в SQL Server, значение по умолчанию — false) |
| Ведение журнала | Добавлено свойство для всех операций (HashObjectNamesInLogs), которое преобразует все имена объектов в хэш-строку в сообщениях журнала. |
| Производительность | Улучшения производительности импорта и экспорта, включая дополнительное ведение журнала для упрощения выявления дополнительных узких мест. |
| SQLCMD | Добавлено свойство для развертывания и Сравнения схем (DoNotEvaluateSqlCmdVariables), которое указывает, будут ли переменные SQLCMD заменены значениями. |



### <a name="fixes"></a>Исправления
| Компонент | Сведения |
| :------ | :------ | 
| Развертывание | Значение по умолчанию MAXDOP изменено с 0 на 8 для [Azure SQL](https://techcommunity.microsoft.com/t5/azure-sql/changing-default-maxdop-in-azure-sql-database/ba-p/1538528), изменение значения по умолчанию модели схемы в DacFx | 
| Сравнение схем | Хранимые процедуры, использующие ключевые слова OUT и OUTPUT, не учитываются в качестве разницы |
| Развертывание | Дополнительная проверка для маркеров больших данных |
| Сборка и развертывание | Полная очистка модели схемы для внешних темпоральных таблиц в целях обеспечения окончательной согласованности DACPAC.  |
| Сборка и развертывание | Добавление обработки и исправления ошибок для непограничных вычислений 150 RE. |
| Импорт и развертывание | Значение последовательности восстановлено во время развертывания |
| Развертывание | Исправлена проблема, из-за которой изменение параметра сжатия для кластеризованного индекса приводило к повторному созданию таблицы вместо выполнения инструкции alter index. |
| Развертывание | Исправлена проблема, из-за которой кластеризованный индекс columnstore удалялся и создавался повторно при изменении столбца таблицы. |
| Развертывание | Исправлены внешние пользователи, которые удалялись и создавались повторно во время развертывания. |
| Сравнение схем | Исправлена ошибка сравнения схем с заданием внешней потоковой передачи. |
| Импорт | Возникло исключение пустой ссылки при включении параметра окружения ReliableDdlEnabled при создании сценариев для отчета о развертывании.|
| Развертывание | Исправлена проблема, из-за которой шаги развертывания, содержащие системное управление версиями, создавались в неправильном порядке. |
| Развертывание | Исправлена проблема, когда происходил сбой при обновлении сравнения схем или развертывании DACPAC из-за целевого объекта, содержащего темпоральные таблицы. |
| Развертывание | Повторно присваивает начальное значение идентификатора после развертывания на основе предыдущего последнего значения целевого объекта. |

### <a name="known-issues"></a>Известные проблемы
| Компонент | Сведения | Обходной путь |
| :------ | :------ |:------ |
| Развертывание | Функция управления рабочими нагрузками Azure Synapse Analytics (группы и классификаторы рабочих нагрузок) пока не поддерживается | Недоступно |
| Развертывание | В сценарии добавочного развертывания, когда пользователь удаляет временную таблицу вместе с зависимыми объектами (функциями, хранимыми процедурами и т. д.), развертывание может завершиться сбоем. Порядок создания скрипта пытается отключить для таблицы SYSTEM_VERSIONING, что необходимо для ее удаления, однако создаваемый порядок шагов является неверным. [Рабочий элемент](https://github.com/microsoft/azuredatastudio/issues/14655) | Создайте скрипт развертывания и перенесите шаг System_Versioning OFF, поставив его непосредственно перед удалением таблицы, после чего запустите скрипт. |

## <a name="186-sqlpackage"></a>sqlpackage 18.6

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 сентября 2020 г.|18.6|15.0.4897.1|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143659)|18 сентября 2020 г.| 18.6|15.0.4897.1|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143497)|18 сентября 2020 г.| 18.6|15.0.4897.1|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2143496)|18 сентября 2020 г.| 18.6|15.0.4897.1|

### <a name="features"></a>Компоненты
| Компонент | Сведения |
| :------ | :------ |
| Платформа | Обновление sqlpackage для поддержки версии .NET Core 3.1. |
| Always Encrypted | Добавлена поддержка импорта и экспорта в безопасном анклаве для SQL Server 2019. |
| Развертывание | Добавлена поддержка пропуска таблиц с поддержкой отслеживания измененных данных при экспорте из Базы данных SQL Azure. |
| Развертывание | Добавлена поддержка параметра индекса OPTIMIZE_FOR_SEQUENTIAL_KEY в Базе данных SQL Azure. |
| Развертывание | Добавлена поддержка столбцов идентификаторов для Azure Synapse Analytics | 
| Справка | Возможность вывода версии sqlpackage в справке (/?) и поддержка параметра /version. | 

### <a name="fixes"></a>Исправления
| Компонент | Сведения |
| :------ | :------ | 
| Развертывание | Исправлен неверный скрипт развертывания, создаваемый при выборе управляемого экземпляра SQL Azure в качестве целевого пользователя, не являющегося sysadmin.  | 
| Развертывание | Исправлена загрузка участников развертывания при выполнении действий сценария. | 
| Справка | Вывод корректного затраченного времени в sqlpackage, когда операция длится больше 1 дня. | 
| Развертывание | Исправлена регистрация DACPAC при развертывании для .NET Core. | 
| Развертывание | Исправлена обработка параметра /accessToken (/at) в sqlpackage на .NET Core. | 
| Развертывание | Инструкции ALTER TABLE разрешены в хранимых процедурах как инструкции не верхнего уровня. | 
| Развертывание | Исправлена проверка материализованных представлений в Azure Synapse Analytics, которая теперь не учитывает регистр | 

### <a name="known-issues"></a>Известные проблемы
| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Функция управления рабочими нагрузками Azure Synapse Analytics (группы и классификаторы рабочих нагрузок) пока не поддерживается | 

## <a name="1851-sqlpackage"></a>18.5.1 sqlpackage

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2134206)|24 июня 2020 г.|18.5.1|15.0.4826.1|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2134312)|24 июня 2020 г.| 18.5.1|15.0.4826.1|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2134311)|24 июня 2020 г.| 18.5.1|15.0.4826.1|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2134310)|24 июня 2020 г.| 18.5.1|15.0.4826.1|

### <a name="fixes"></a>Исправления
| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Исправлена регрессия, появившаяся в версии 18.5, в результате которой возникает ошибка "Неправильный синтаксис около «тип»" при развертывании пакета DACPAC или импорт пакета BACPAC с пользователем с внешним входом в локальную среду | 

## <a name="185-sqlpackage"></a>18.5 sqlpackage

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2128142)|28 апреля 2020 г.|18.5|15.0.4769.1|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2128145)|28 апреля 2020 г.| 18.5|15.0.4769.1|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2128144)|28 апреля 2020 г.| 18.5|15.0.4769.1|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2128143)|28 апреля 2020 г.| 18.5|15.0.4769.1|

### <a name="features"></a>Компоненты
| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Классификация конфиденциальности данных теперь поддерживается для SQL Server 2008 и более поздних версий, Базы данных SQL Azure и Azure Synapse Analytics |
| Развертывание | Добавлена поддержка Azure Synapse Analytics для ограничений таблицы |
| Развертывание | Добавлена поддержка Azure Synapse Analytics для упорядоченного кластеризованного индекса columnstore |
| Развертывание | Добавлена поддержка внешнего источника данных (для Oracle, Teradata, MongoDB/CosmosDB, ODBC, кластера больших данных) и внешней таблицы для кластера больших данных SQL Server 2019. |
| Развертывание | Добавлен экземпляр базы данных SQL для пограничных вычислений в качестве поддерживаемого выпуска. |
| Развертывание | Поддержка имен серверов управляемого экземпляра в форме \<server>.\<dnszone>.database.windows.net |
| Развертывание | Добавлена поддержка команды Copy в Azure Synapse Analytics |
| Развертывание | Добавлен параметр развертывания IgnoreTablePartitionOptions во время публикации, чтобы избежать повторного создания таблицы при изменении функции секционирования в таблице для Azure Synapse Analytics |
| .NET Core | Добавлена поддержка Microsoft.Data.SqlClient в версии .NET Core sqlpackage. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления
| Fix | Сведения |
| :-- | :------ |
| Развертывание | Исправлен синтаксический анализ пути JSON в качестве выражения. |
| Развертывание | Исправлено создание инструкций GRANT для разрешений AlterAnyDatabaseScopedConfiguration и AlterAnySensitivityClassification. |
| Развертывание | Исправлена проблема, при которой разрешение на внешние скрипты не распознается. |
| Развертывание | Исправление для встроенного свойства — неявное добавление свойства не должно отображаться в различиях, но явные упоминания должны отображаться в скрипте. |
| Развертывание | Устранена проблема, при которой изменение таблицы, на которую ссылается материализованное представление (MV), приводит к созданию инструкций ALTER VIEW, не поддерживаемых для материализованных представлений в Azure Synapse Analytics |
| Развертывание | Устранена ошибка публикации при добавлении столбца в таблицу с данными для Azure Synapse Analytics |
| Развертывание | Сценарий исправления обновления должен переместить данные в новую таблицу при изменении типа столбца распределения (сценарий потери данных) для Azure Synapse Analytics |
| ScriptDom | Устранена ошибка ScriptDom, при которой не удавалось распознать встроенные ограничения, определенные после встроенного индекса. |
| ScriptDom | Исправлено отсутствие закрывающей скобки ScriptDom SYSTEM_TIME в пакетной инструкции. |
| Always Encrypted | Исправлена ошибка, при которой таблицу #tmpErrors не удавалось удалить, если sqlpackage повторно подключается, а временная таблица уже исчезла, так как временная таблица исчезает после разрыва соединения. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы
| Компонент | Сведения |
| :------ | :------ |
| Развертывание |  В версии 18.5 появилась регрессия, в результате которой возникает ошибка "Неправильный синтаксис около «тип»" при развертывании пакета DACPAC или импорт пакета BACPAC с пользователем с внешним входом в локальную среду. Решение — использовать sqlpackage 18.4, и оно будет исправлено в следующей версии sqlpackage. | 
| .NET Core | Импорт пакетов BACPAC с конфиденциальной классификацией завершается сбоем с ошибкой "Внутренняя неустранимая ошибка подключения" из-за этой [известной проблемы](https://github.com/dotnet/SqlClient/issues/559) в Microsoft.Data.SqlClient. Эта проблема будет устранена в следующем выпуске sqlpackage. |
| &nbsp; | &nbsp; |

## <a name="1841-sqlpackage"></a>sqlpackage версии 18.4.1

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2113703)|13 декабря 2019 г.|18.4.1|15.0.4630.1|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2113705)|13 декабря 2019 г.| 18.4.1|15.0.4630.1|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2113331)|13 декабря 2019 г.| 18.4.1|15.0.4630.1|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2113704)|13 декабря 2019 г.| 18.4.1|15.0.4630.1|

### <a name="fixes"></a>Исправления
| Fix | Сведения |
| :-- | :------ |
| ScriptDom |  Регрессия разбора ScriptDom была введена в версии 18.3.1, в которой RENAME неправильно обрабатывается как токен наивысшего уровня, что приводит к сбою анализа.
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы 

| Компонент | Сведения |
| :------ | :------ |
| Развертывание |  Регрессия была введена в версии 18.4.1, и это привело к проблеме: "Ссылка на объект не устанавливается на экземпляр объекта". Ошибка при развертывании DACPAC-файла или импорте BACPAC-файла с пользователем с внешним именем входа. Решение — использовать sqlpackage 18.4, и оно будет исправлено в следующей версии sqlpackage. | 
| &nbsp; | &nbsp; |

## <a name="184-sqlpackage"></a>18.4 sqlpackage

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2108813)|29 октября 2019 г.|18.4|15.0.4573.2|
|macOS .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2108815)|29 октября 2019 г.| 18.4|15.0.4573.2|
|Linux .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2108814)|29 октября 2019 г.| 18.4|15.0.4573.2|
|Windows .NET Core |[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2109019)|29 октября 2019 г.| 18.4|15.0.4573.2|

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Добавлена поддержка развертывания в Azure Synapse Analytics (общедоступная версия). | 
| Платформа | sqlpackage .NET Core (общедоступная версия) для macOS, Linux и Windows. | 
| Безопасность | Удалено подписывание кода SHA1. |
| Развертывание | Добавлена поддержка новых выпусков базы данных Azure: GeneralPurpose, BusinessCritical, Hyperscale. |
| Развертывание | Добавлена поддержка Управляемого экземпляра для пользователя и групп Azure Active Directory. |
| Развертывание | Поддержка параметра /AccessToken для sqlpackage в .NET Core. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы 

| Компонент | Сведения |
| :------ | :------ |
| ScriptDom |  Регрессия разбора ScriptDom была введена в версии 18.3.1, в которой RENAME неправильно обрабатывается как токен наивысшего уровня, что приводит к сбою анализа. Эта проблема будет устранена в следующем выпуске sqlpackage. | 
| &nbsp; | &nbsp; |

### <a name="known-issues-for-net-core"></a>Известные проблемы в .NET Core

| Компонент | Сведения |
| :------ | :------ |
| Импорт |  Для импорта BACPAC-файлов со сжатыми файлами более 4 ГБ может потребоваться версия sqlpackage .NET Core.  Такое поведение обусловлено тем, как .NET Core создает заголовки ZIP, которые, хотя и являются допустимыми, не могут быть прочитаны с помощью полной версии sqlpackage .NET Framework. | 
| Развертывание | Параметр /p:Storage=File не поддерживается. В .NET Core поддерживается только память. | 
| Always Encrypted | sqlpackage .NET Core не поддерживает столбцы Always Encrypted. | 
| Безопасность | sqlpackage .NET Core не поддерживает параметр /ua для многофакторной проверки подлинности. | 
| Развертывание | Старые файлы DACPAC и BACPAC версии 2, использующие сериализацию данных JSON, не поддерживаются. |
| &nbsp; | &nbsp; |

## <a name="1831-sqlpackage"></a>sqlpackage 18.3.1

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2102893)|13 сентября 2019 г.|18.3.1|15.0.4538.1|
|macOS .NET Core (предварительная версия)|[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2102894)|13 сентября 2019 г.| 18.3.1|15.0.4538.1|
|Linux .NET Core (предварительная версия)|[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2102978)|13 сентября 2019 г.| 18.3.1|15.0.4538.1|
|Windows .NET Core (предварительная версия)|[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2102979)|13 сентября 2019 г.| 18.13.1|15.0.4538.1|

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Добавлена поддержка развертывания в Azure Synapse Analytics (предварительная версия). | 
| Развертывание | Добавлен параметр /p:DatabaseLockTimeout=(INT32 '60') для sqlpackage. | 
| Развертывание | Добавлен параметр /p:LongRunningCommandTimeout=(INT32) для sqlpackage. |
| Экспорт и извлечение | Добавлен параметр /p:TempDirectoryForTableData=(STRING) для sqlpackage. |
| Развертывание | Разрешена загрузка участников развертывания из дополнительных расположений. Участники развертывания будут загружены из того же каталога, что и развертываемый целевой файл DACPAC, каталог Extensions относительно двоичного файла sqlpackage.exe и параметр /p:AdditionalDeploymentContributorPaths=(STRING), добавленный к sqlpackage, где можно указать дополнительные расположения каталогов. |
| Развертывание | Добавлена поддержка OPTIMIZE_FOR_SEQUENTIAL_KEY. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Развертывание | Исправлено игнорирование автоматических индексов, чтобы они не удалялись при развертывании. | 
| Always Encrypted | Исправлена обработка Always Encrypted столбцов varchar. | 
| Сборка и развертывание | Исправлена ошибка по разрешению метода nodes() для наборов столбцов XML.| 
| ScriptDom | Исправлены дополнительные случаи, в которых строка URL интерпретировалась как токен наивысшего уровня. | 
| График | Исправлена ошибка формирования TSQL для ссылок на псевдостолбцы в ограничениях.  | 
| Экспорт | Создание случайных паролей, соответствующих требованиям к сложности. | 
| Развертывание | Исправление позволяет учитывать время ожидания команды при извлечении ограничений. | 
| .NET Core (предварительная версия) | Исправление вывода журнала диагностики в файл. | 
| .NET Core (предварительная версия) | Использование потоковой передачи, чтобы экспортировать табличные данные для поддержки больших таблиц. | 
| &nbsp; | &nbsp; |

## <a name="182-sqlpackage"></a>sqlpackage 18.2

|Платформа|Скачивание|Дата выпуска|Версия|Сборка
|:---|:---|:---|:---|:---|
|Windows|[Установщик MSI](https://go.microsoft.com/fwlink/?linkid=2087429)|15 апреля 2019 г.|18.2|15.0.4384.2|
|macOS .NET Core (предварительная версия)|[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2087247)|15 апреля 2019 г. | 18.2 |15.0.4384.2|
|Linux .NET Core (предварительная версия)|[ZIP-файл](https://go.microsoft.com/fwlink/?linkid=2087431)|15 апреля 2019 г. | 18.2 |15.0.4384.2|

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| График | Добавлена ​​поддержка графовой таблицы для ограничения границ и предложений ограничения границ. |
| Развертывание | Включено правило проверки модели с поддержкой 32 столбцов для ключей индекса для SQL Server 2016 и выше. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Развертывание | Исправлено реконструирование базы данных RTM SQL Server 2016 вследствие неподдерживаемого указания запроса. |
| Развертывание | Исправлен порядок развертывания операторов изменения автозакрытия, которые выполняются перед созданием операторов файловой группы. |
| ScriptDom | Исправлена ​​регрессия разбора ScriptDom, в которой строка URL интерпретировалась как токен наивысшего уровня. |
| Развертывание | Исправлено ​​исключение пустой ссылки при разборе оператора добавления индекса изменения таблицы. | 
| Сравнение схем | Исправлено сравнение схем для пустых значений материализованных вычисляемых столбцов, постоянно отображающихся как разные.|
| &nbsp; | &nbsp; |

## <a name="181-sqlpackage"></a>sqlpackage 18.1

Дата выпуска: &nbsp; 1 февраля 2019 г.  
Сборка: &nbsp; 15.0.4316.1  
Предварительный выпуск.

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Добавлена поддержка параметров сортировки UTF8. |
| Развертывание | Включены некластеризованные индексы columnstore в индексированном представлении. |
| Платформа | Перемещено в .NET Core 2.2. | 
| Сравнение схем | Настроено использование памяти резервного хранилища для сравнения схем в .NET Core. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Производительность | Исправлена производительность для использования модуля оценки унаследованной кратности для запросов реконструирования. | 
| Производительность | Исправлена ​​значительная проблема производительности сравнения схем при генерации скрипта. | 
| Сравнение схем | Исправлена логика обнаружения смещения схемы для игнорирования определенных сеансов расширенных событий (xevent). |
| График | Исправлен порядок импорта графовых таблиц. | 
| Экспорт | Исправлен экспорт внешних таблиц с разрешениями объекта. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы

Этот выпуск включает в себя кроссплатформенные предварительные сборки sqlpackage, предназначенные для .NET Core 2.2. Sqlpackage можно запустить в macOS и Linux.

| Известная проблема | Сведения |
| :---------- | :------ |
| Развертывание | В .NET Core участники сборки и развертывания не поддерживаются. | 
| Развертывание | В .NET Core старые файлы DACPAC и BACPAC, использующие сериализацию данных JSON, не поддерживаются. | 
| Развертывание | В .NET Core файлы DACPACS (например, master.dacpac), на которые существуют ссылки, могут не разрешаться из-за проблем с чувствительными к регистру файловыми системами. Правильным решением будет написание имени файла, на который ссылаются, прописными буквами (например, MASTER.BACPAC). |
| &nbsp; | &nbsp; |

## <a name="180-sqlpackage"></a>sqlpackage 18.0

Дата выпуска: &nbsp; 24 октября 2018 г.  
Сборка: &nbsp; 15.0.4200.1

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Развертывание | Добавлена ​​поддержка уровня совместимости базы данных 150. | 
| Развертывание | Добавлена поддержка Управляемого экземпляра. | 
| Производительность | Добавлен параметр командной строки MaxParallelism для указания степени параллелизма операций с базой данных. | 
| Безопасность | Добавлен параметр командной строки AccessToken для указания маркера проверки подлинности при подключении к SQL Server. | 
| Импорт | Добавлена поддержка потоковой передачи типов данных BLOB и CLOB для импорта. | 
| Развертывание | Добавлена ​​поддержка скалярного параметра UDF INLINE. | 
| График | Добавлена поддержка для синтаксиса MERGE графовой таблицы. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| График | Исправлен неразрешенный псевдостолбец для графовых таблиц. |
| Развертывание | Исправлено создание базы данных с файловыми группами, оптимизированными для операций в памяти, при использовании оптимизированных для операций в памяти таблиц. |
| Развертывание | Исправлено включение расширенных свойств во внешние таблицы. |
| &nbsp; | &nbsp; |

## <a name="178-sqlpackage"></a>sqlpackage 17.8

Дата выпуска: &nbsp; 22 июня 2018 г.  
Сборка: &nbsp; 14.0.4079.2

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Диагностика | Улучшены сообщения об ошибках при сбое подключения, в том числе сообщение об исключении SqlClient. |
| Развертывание | Включена поддержка сжатия индексирования одной секции индексов для импорта и экспорта. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Развертывание | Исправлена ​​проблема реконструирования для наборов столбцов XML в SQL 2017 и более поздних версиях. | 
| Развертывание | Исправлена ​​ошибка, из-за которой сценарии уровня совместимости базы данных 140 пропускались для базы данных SQL Azure. |
| &nbsp; | &nbsp; |

## <a name="1741-sqlpackage"></a>sqlpackage 17.4.1

Дата выпуска: &nbsp; 25 января 2018 г.  
Сборка: &nbsp; 14.0.3917.1

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Импорт и экспорт | Добавлен параметр командной строки ThreadMaxStackSize для анализа Transact-SQL с большим количеством вложенных операторов. |
| Развертывание | Поддержка параметров сортировки каталога базы данных. | 
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Импорт | Исправлены ошибки при импорте BACPAC-файла Базы данных SQL Azure в локальный экземпляр, так как _главные ключи базы данных без пароля не поддерживаются этой версией SQL Server_. |
| График | Исправлена неразрешенная ошибка псевдостолбца для графовых таблиц. |
| Сравнение схем | Исправлена проверка подлинности SQL для сравнения схем. | 
| &nbsp; | &nbsp; |

## <a name="1740-sqlpackage"></a>sqlpackage 17.4.0

Дата выпуска: &nbsp; 12 декабря 2017 г.  
Сборка: &nbsp; 14.0.3881.1

### <a name="features"></a>Компоненты

| Компонент | Сведения |
| :------ | :------ |
| Развертывание |  Добавлена ​​поддержка _политики временного хранения_ в SQL 2017+ и базе данных SQL Azure. | 
| Диагностика | Добавлен параметр командной строки /DiagnosticsFile:"C:\Temp\sqlpackage.log " для указания пути к файлу для сохранения диагностической информации. | 
| Диагностика | Добавлен параметр командной строки /Diagnostics для записи диагностической информации в консоль. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Исправления

| Fix | Сведения |
| :-- | :------ |
| Развертывание | Не блокируйте при обнаружении уровня совместимости базы данных, который не удается распознать. Вместо этого предполагается использовать последнюю базу данных SQL Azure или локальную платформу. |
| &nbsp; | &nbsp; |
