---
title: Сбор данных для устранения неполадок с Python и R
description: Узнайте, как собирать данные, которые требуются для решения проблем со Службами машинного обучения SQL Server самостоятельно или через службу поддержки пользователей Майкрософт.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/31/2021
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 8744bc74846760da36c242dd2bb37d0b78ff3e00
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106099961"
---
# <a name="collect-data-to-troubleshoot-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Сбор данных для устранения неполадок с выполнением скриптов Python и R при использовании Служб машинного обучения SQL Server

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

В этой статье показано, как собирать данные, которые требуются для решения проблем, связанных со [Службами машинного обучения SQL Server](../sql-server-machine-learning-services.md). Эти данные могут быть полезны при решении проблем самостоятельно или с помощью службы поддержки пользователей Майкрософт.

## <a name="sql-server-version-and-edition"></a>Версия и выпуск SQL Server

SQL Server 2016 R Services — это первый выпуск SQL Server с интегрированной поддержкой языка R. SQL Server 2016 с пакетом обновления 1 (SP1) имеет несколько значительных усовершенствований, включая возможность запуска внешних сценариев. При использовании SQL Server 2016 рекомендуется установить пакет обновления 1 (SP1) или более поздней версии.

SQL Server 2017 и более поздних версий включают интеграцию с языком Python. В более ранних выпусках интеграция возможностей Python недоступна.

Информацию о том, как узнать версию и выпуск, см. в этой статье, где перечислены номера сборок для каждой [версии SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/783.sql-server-versions.aspx#Service_Pack_editions).

В зависимости от используемого выпуска SQL Server некоторые функции машинного обучения могут быть недоступны или ограничены. 

## <a name="r-language-and-tool-versions"></a>Версии языка и инструментов R

Как правило, версия Microsoft R, устанавливаемая при выборе компонента служб R или служб машинного обучения, определяется номером сборки SQL Server. При обновлении или исправлении SQL Server необходимо также обновить или исправить компоненты R.

Список выпусков и ссылки на загружаемые компоненты R см. в разделе [Установка компонентов машинного обучения без доступа к Интернету](../install/sql-ml-component-install-without-internet-access.md). На компьютерах с доступом к Интернету необходимая версия R определяется и устанавливается автоматически.

Обновить компоненты R Server можно и отдельно от ядра СУБД SQL Server, в процессе так называемой привязки. Версия R, используемая при выполнении кода R в SQL Server, может отличаться в зависимости от установленной версии SQL Server и от того, переведен ли сервер на последнюю версию R.

### <a name="determine-the-r-version"></a>Определение версии R

Простейший способ определить версию R — это получить свойства среды выполнения, выполнив следующую инструкцию:

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
       , @script = N'
# Transform R version properties to data.frame
OutputDataSet <- data.frame(
  property_name = c("R.version", "Revo.version"),
  property_value = c(R.Version()$version.string, Revo.version$version.string),
  stringsAsFactors = FALSE)
# Retrieve properties like R.home, libPath & default packages
OutputDataSet <- rbind(OutputDataSet, data.frame(
  property_name = c("R.home", "libPaths", "defaultPackages"),
  property_value = c(R.home(), .libPaths(), paste(getOption("defaultPackages"), collapse=", ")),
  stringsAsFactors = FALSE)
)
'
WITH RESULT SETS ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

> [!TIP]
> Если R Services не работает, попробуйте выполнить только часть скрипта R из RGui.

В качестве последнего средства можно открыть файлы на сервере и узнать установленную версию. Для этого необходимо найти файл rlauncher.config, чтобы узнать расположение среды выполнения R и текущего рабочего каталога. Рекомендуем создать и открыть копию этого файла, чтобы случайно не изменить свойства.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name\MSSQL\Binn\rlauncher.config`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Binn\rlauncher.config`

Чтобы получить версии R и RevoScaleR, откройте командную строку R или RGui, связанный с экземпляром.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instancename>\R_SERVICES\bin\x64\RGui.exe`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES\bin\x64\RGui.exe`

В консоли R при запуске отображаются сведения о версии. Например, следующая версия представляет конфигурацию SQL Server 2017 по умолчанию:

```console
*Microsoft R Open 3.3.3*

*The enhanced R distribution from Microsoft*

*Microsoft packages Copyright (C) 2017 Microsoft*

*Loading Microsoft R Server packages, version 9.1.0.*
```

## <a name="python-versions"></a>Версии Python

Существует несколько способов выяснить версию Python. Самый простой способ — выполнить эту инструкцию из Management Studio или любого другого средства для SQL-запросов:

```sql
-- Get Python runtime properties:
exec sp_execute_external_script
       @language = N'Python'
       , @script = N'
import sys
import pkg_resources
OutputDataSet = pandas.DataFrame(
                    {"property_name": ["Python.home", "Python.version", "Revo.version", "libpaths"],
                    "property_value": [sys.executable[:-10], sys.version, pkg_resources.get_distribution("revoscalepy").version, str(sys.path)]}
)
'
with WITH RESULT SETS (SQL keywords) ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

Если службы машинного обучения не работают, можно определить установленную версию Python, просмотрев файл pythonlauncher.config. Рекомендуем создать и открыть копию этого файла, чтобы случайно не изменить свойства.

1. Только для SQL Server 2017: `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog\pythonlauncher.config`
2. Получите значение для **PYTHONHOME**.
3. Получите значение текущего рабочего каталога.

> [!NOTE]
> Если и Python, и R установлена в SQL Server 2017, рабочий каталог и пул рабочих учетных записей будут общими для языков R и Python.

## <a name="are-multiple-instances-of-r-or-python-installed"></a>Установлены несколько экземпляров R или Python?

Проверьте, не установлено ли на компьютере больше одной копии библиотек R. Дублирование может произойти, если:

* Во время установки вы выбрали R Services (в базе данных) и R Server (изолированный).
* Вы установили Microsoft R Client в дополнение к SQL Server.
* Другой набор библиотек R был установлен с помощью инструментов R для Visual Studio, R Studio, Microsoft R Client или другой среды IDE R.
* На компьютере размещаются несколько экземпляров SQL Server и больше одного из них используют машинное обучение.

Те же условия относятся и к Python.

Если вы обнаружите, что у вас установлено несколько библиотек или сред выполнения, убедитесь, что вы получаете только ошибки, связанные с средами выполнения Python или R, которые используются экземпляром SQL Server.

## <a name="origin-of-errors"></a>Происхождение ошибок

Ошибки, отображаемые при попытке выполнить код R, могут поступать из любого из следующих источников:

* Ядро СУБД SQL Server, включая хранимую процедуру sp_execute_external_script
* Доверенная панель запуска SQL Server
* Другие компоненты платформы расширяемости, включая средства запуска R и Python и вспомогательные процессы
* Поставщики, такие как Microsoft Open Database Connectivity (ODBC)
* Язык R

В первый раз при работе со службой может быть трудно определить, из каких служб исходят те или иные сообщения. Рекомендуем записывать не только точный текст сообщения, но и контекст, в котором оно было показано. Проверьте, какое клиентское программное обеспечение вы используете для выполнения кода машинного обучения:

* Вы используете Management Studio? Внешнее приложение?
* Выполняете код R в удаленном клиенте или непосредственно в хранимой процедуре?

## <a name="sql-server-log-files"></a>Файлы журналов SQL Server

Получите последний ERRORLOG SQL Server. Полный набор журналов ошибок состоит из файлов, которые хранятся в следующем каталоге журналов по умолчанию:

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.SQL2016\MSSQL\Log\ExtensibilityLog`

> [!NOTE]
> Точное имя папки зависит от имени экземпляра.

## <a name="errors-returned-by-sp_execute_external_script"></a>Ошибки, возвращаемые sp_execute_external_script

Для получения полного текста возвращаемых ошибок, выполните команду sp_execute_external_script.

Чтобы отбросить проблемы с R или Python, можно выполнить скрипт, который запускает среду выполнения R или Python и передает данные туда и обратно.

**Для R**

```sql
exec sp_execute_external_script @language =N'R',  
@script=N'OutputDataSet<-InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

**Для Python**

```sql
exec sp_execute_external_script @language =N'Python',  
@script=N'OutputDataSet= InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

## <a name="errors-generated-by-the-extensibility-framework"></a>Ошибки платформы расширяемости

SQL Server создает отдельные журналы для сред выполнения внешних скриптов. Эти ошибки вызывают не Python или R. Их вызывают компоненты расширяемости в SQL Server, включая средства запуска конкретных языков и их вспомогательные процессы.

Эти журналы находятся в следующих расположениях по умолчанию:

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog`

> [!NOTE]
> Точное имя папки зависит от имени экземпляра. В зависимости от конфигурации папка может находиться на другом диске.

Например, с платформой расширяемости связаны следующие сообщения журнала:

* *Не удалось выполнить операцию LogonUser для пользователя MSSQLSERVER01*
  
  Это может означать, что рабочие учетные записи, выполняющие внешние скрипты, не могут получить доступ к экземпляру.

* *InitializePhysicalUsersPool Failed*
  
  Это сообщение может означать, что параметры безопасности не позволяют программе установки создать пул рабочих учетных записей, необходимых для выполнения внешних скриптов.

* *Не удалось инициализировать диспетчер контекста безопасности*

* *Не удалось инициализировать диспетчер сеансов безопасности*

## <a name="system-events"></a>Системные события

1. Откройте просмотр событий Windows и выполните поиск сообщений, содержащих строку *Launchpad*, в журнале **системных событий**.
2. Откройте файл ExtLaunchErrorlog и найдите строку *ErrorCode*. Проверьте сообщение, связанное с ErrorCode.

Например, следующие сообщения являются общими системными ошибками, связанными с платформой расширяемости SQL Server:

* *Службе панели запуска SQL Server (MSSQLSERVER) не удалось запуститься из-за следующей ошибки: <text>*

* *Служба не ответила на запрос запуска или контроля за отведенное время.*

* *Истекло время ожидания (120 000 мс) во время ожидания подключения службы панели запуска управления SQL Server (MSSQLSERVER).*

## <a name="dump-files"></a>Файлы дампа

Если вы знаете, как выполнять отладку, то сможете использовать файлы дампа для анализа сбоя в панели запуска.

1. Найдите папку, в которой находятся журналы начальной загрузки для SQL Server. Например, в SQL Server 2016 путь по умолчанию — C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log.
2. Откройте вложенную папку журналов начальной загрузки, относящуюся к расширяемости.
3. Если вам нужно отправить запрос в службу поддержки, добавьте все содержимое этой папки в ZIP-архив. Например, C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log\LOG\ExtensibilityLog.
  
Точное расположение зависит от конкретной системы и может находиться не на диске C. Не забудьте получить журналы для экземпляра, в котором установлено машинное обучение.

## <a name="configuration-settings"></a>Параметры конфигурации

В этом разделе перечислены дополнительные компоненты или поставщики, которые могут быть источником ошибок при выполнении скриптов R или Python.

### <a name="what-network-protocols-are-available"></a>Какие сетевые протоколы доступны?

Службам машинного обучения требуются следующие сетевые протоколы для внутреннего взаимодействия между компонентами расширяемости и для взаимодействия с внешними клиентами R или Python.

* Именованные каналы
* TCP/IP

Откройте диспетчер конфигурации SQL Server, чтобы определить, установлен ли протокол, и, если да, то включен ли он.

### <a name="security-configuration-and-permissions"></a>Конфигурация и разрешения безопасности

Для рабочих учетных записей:

1. На панели управления откройте раздел **Пользователи и группы** и выберите группу, которая используется для выполнения заданий внешних скриптов. По умолчанию это группа **SQLRUserGroup**.
2. Убедитесь, что группа существует и содержит хотя бы одну рабочую учетную запись.
3. В SQL Server Management Studio выберите экземпляр, в котором будут выполняться задания R или Python, выберите **Безопасность**, а затем определите, выполняется ли вход в SQLRUserGroup.
4. Проверьте разрешения для группы пользователей.

Для учетных записей отдельных пользователей:

1. Определите, поддерживает ли экземпляр проверку подлинности в смешанном режиме, только вход в SQL или только проверку подлинности Windows. Этот параметр влияет на требования к коду R или Python.
2. Для каждого пользователя, который должен выполнять код R, определите необходимый уровень разрешений в каждой базе данных, куда будут записываться объекты из R, где будут доступны данные или где будут создаваться объекты.
3. Чтобы включить выполнение скрипта, создайте необходимые роли или добавьте пользователей в следующие роли:

   - Все, кроме *db_owner*: требуется разрешение EXECUTE ANY EXTERNAL SCRIPT.
   - *db_datawriter*: для записи результатов из R или Python.
   - *db_ddladmin*: для создания новых объектов.
   - *db_datareader*: для чтения данных, используемых кодом R или Python.
4. Обратите внимание, изменились ли учетные записи запуска по умолчанию при установке SQL Server 2016.
5. Если пользователю необходимо установить новые пакеты R или использовать пакеты R, установленные другими пользователями, нужно включить управление пакетами в экземпляре, а затем назначить дополнительные разрешения. 

### <a name="what-folders-are-subject-to-locking-by-antivirus-software"></a>Какие папки блокируются антивирусными программами?

Антивирусная программа может блокировать папки, препятствия настройке функций машинного обучения и выполнению сценариев. Определите, подвергаются ли какие-либо папки в дереве SQL Server поиску вирусов.

Однако, если в экземпляре установлены несколько служб или компонентов, перечислить все возможные папки, используемые экземпляром, может быть непросто. Например, при добавлении новых компонентов новые папки необходимо идентифицировать и исключить.

Более того, некоторые компоненты создают новые папки в среде выполнения динамически. Например, новые каталоги в среде выполнения создают таблицы, хранимые процедуры и функции выполняющейся в памяти OLTP. Имена этих папок часто содержат идентификаторы GUID и не прогнозируются. Доверенная панель запуска SQL Server создает новые рабочие каталоги для заданий скриптов R и Python.

Поскольку исключить все папки, необходимые для процесса SQL Server и его функций, невозможно, рекомендуем исключить все дерево каталогов экземпляров SQL Server.

### <a name="is-the-firewall-open-for-sql-server-does-the-instance-support-remote-connections"></a>Открыт ли брандмауэр для SQL Server? Поддерживает ли экземпляр удаленные подключения?

1. Чтобы определить, поддерживает ли SQL Server удаленные подключения, см. раздел [Настройка удаленных подключений к серверу](../../database-engine/configure-windows/view-or-configure-remote-server-connection-options-sql-server.md).

2. Определите, было ли создано правило брандмауэра для SQL Server. В целях безопасности при установке по умолчанию удаленный клиент R или Python может не иметь возможности подключиться к экземпляру. Дополнительные сведения см. в раздел [Устранение неполадок с подключением к SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="see-also"></a>См. также раздел

[Устранение неполадок с машинным обучением в SQL Server](common-issues-external-script-execution.md)