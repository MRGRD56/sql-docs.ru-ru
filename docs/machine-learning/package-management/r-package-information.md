---
title: Получение сведений о пакете R
description: Узнайте, как получить сведения об установленных пакетах R в Службах машинного обучения SQL Server и SQL Server R Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/27/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3669b46fa88e8d59fbb3eeb5ff91bc8bb9542ce7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352537"
---
# <a name="get-r-package-information"></a>Получение сведений о пакете R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этой статье описывается, как получить сведения об установленных пакетах R в [Службах машинного обучения в SQL Server](../sql-server-machine-learning-services.md) и [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Примеры сценариев R показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end
::: moniker range="<=sql-server-2017"
В этой статье описывается, как получить сведения об установленных пакетах R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md). Примеры сценариев R показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этой статье описывается, как получить сведения об установленных пакетах R в [Службах машинного обучения Управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Примеры сценариев R показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end

## <a name="default-r-library-location"></a>Расположение библиотеки R по умолчанию

При установке машинного обучения с помощью SQL Server на уровне экземпляра создается отдельная библиотека пакетов для каждого устанавливаемого языка. В Windows библиотека экземпляров является защищенной папкой, зарегистрированной в SQL Server.

Все скрипты, выполняемые в базе данных на SQL Server, должны загружать функции из библиотеки экземпляров. SQL Server не может получить доступ к пакетам, установленным в других библиотеках. Это относится и к удаленным клиентам: любой скрипт R, выполняющийся в контексте вычислений сервера, может использовать только пакеты, установленные в библиотеке экземпляров.
Для защиты серверных ресурсов библиотека экземпляров по умолчанию может быть изменена только администратором компьютера.

::: moniker range="=sql-server-2016"
Путь по умолчанию к двоичным файлам для R:

`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`

Предполагается, что экземпляр SQL по умолчанию — MSSQLSERVER. Если SQL Server устанавливается как определяемый пользователем именованный экземпляр, вместо него используется указанное имя.
::: moniker-end

::: moniker range="=sql-server-2017"
Путь по умолчанию к двоичным файлам для R:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library`

Предполагается, что экземпляр SQL по умолчанию — MSSQLSERVER. Если SQL Server устанавливается как определяемый пользователем именованный экземпляр, вместо него используется указанное имя.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Путь по умолчанию к двоичным файлам для R:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\library`

Предполагается, что экземпляр SQL по умолчанию — MSSQLSERVER. Если SQL Server устанавливается как определяемый пользователем именованный экземпляр, вместо него используется указанное имя.
::: moniker-end

Выполните приведенную ниже инструкцию, чтобы проверить библиотеку по умолчанию для текущего экземпляра R:

```sql
EXECUTE sp_execute_external_script  
  @language = N'R',
  @script = N'OutputDataSet <- data.frame(.libPaths());'
WITH RESULT SETS (([DefaultLibraryName] VARCHAR(MAX) NOT NULL));
GO
```

## <a name="default-microsoft-r-packages"></a>Пакеты Microsoft R по умолчанию

::: moniker range="=sql-server-2016"

Приведенные ниже пакеты Microsoft R устанавливаются вместе с SQL Server R Services.

|Пакеты | Версия | Описание |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 8.0.3 | Используется для удаленных контекстов вычислений, потоковой передачи, параллельного выполнения функций rx для импорта и преобразования, моделирования, визуализации и анализа данных. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Используется для включения скрипта R в хранимые процедуры. |

::: moniker-end

::: moniker range="=sql-server-2017"

Приведенные ниже пакеты Microsoft R устанавливаются со Службами машинного обучения SQL Server, если во время установки был выбран компонент R.

|Пакеты | Версия | Описание |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.2 | Используется для удаленных контекстов вычислений, потоковой передачи, параллельного выполнения функций rx для импорта и преобразования, моделирования, визуализации и анализа данных. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Используется для включения скрипта R в хранимые процедуры. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 1.4.0 | Добавляет алгоритмы машинного обучения в R. | 
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Используется для написания инструкций многомерных выражений в R. |

::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"

Приведенные ниже пакеты Microsoft R устанавливаются со Службами машинного обучения SQL Server, если во время установки был выбран компонент R.

|Пакеты | Версия | Описание |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.4.7 | Используется для удаленных контекстов вычислений, потоковой передачи, параллельного выполнения функций rx для импорта и преобразования, моделирования, визуализации и анализа данных. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Используется для включения скрипта R в хранимые процедуры. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 9.4.7 | Добавляет алгоритмы машинного обучения в R. |
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Используется для написания инструкций многомерных выражений в R. |

::: moniker-end

### <a name="component-upgrades"></a>Обновление компонентов

По умолчанию пакеты R обновляются с помощью пакетов обновления и накопительных пакетов обновления. Дополнительные пакеты и обновление полных версий компонентов R можно применять только путем обновления продукта или привязки поддержки R к Microsoft Machine Learning Server.

::: moniker range="=sql-server-2016"
Кроме того, вы можете добавлять пакеты MicrosoftML и olapR в экземпляр SQL Server с помощью обновления компонента.
::: moniker-end

Дополнительные сведения см. в разделе [Обновление компонентов R и Python в SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-r-packages"></a>Пакеты R с открытым кодом по умолчанию

Поддержка R включает в себя версию с открытым кодом, позволяющую вызывать базовые функции R и устанавливать дополнительные пакеты с открытым кодом и пакеты сторонних производителей. Поддержка языка R включает такие базовые функции, как **base**, **stats**, **utils** и другие. В базовую установку R также входят многочисленные образцы наборов данных и стандартные инструменты R, такие как **RGui** (упрощенный интерактивный редактор) и **RTerm** (командная строка R).

Дистрибутив R с открытым исходным кодом, включенный в установку, — [Microsoft R Open (MRO)](https://mran.microsoft.com/open). MRO расширяет возможности базового R, включая дополнительные пакеты с открытым кодом, такие как [Intel Math Kernel Library](https://en.wikipedia.org/wiki/Math_Kernel_Library).

Сведения о том, какая версия R устанавливается вместе с той или иной версией SQL Server, приведены в разделе [Версии Python и R](../sql-server-machine-learning-services.md#versions).

> [!IMPORTANT]
> Никогда не следует вручную перезаписывать версию R, установленную программой установки SQL Server, более новыми версиями в Интернете. Пакеты Microsoft R основаны на конкретных версиях R. Изменение установки может привести к дестабилизации.

## <a name="list-all-installed-r-packages"></a>Просмотр всех установленных пакетов R

В приведенном ниже примере используется функция R `installed.packages()` в хранимой процедуре [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы вывести список пакетов, которые были установлены в библиотеке R_SERVICES для текущего экземпляра. Этот скрипт возвращает поля имени и версии пакета в файле описания.

```sql
EXECUTE sp_execute_external_script
  @language=N'R',
@script = N'str(OutputDataSet);
packagematrix <- installed.packages();
Name <- packagematrix[,1];
Version <- packagematrix[,3];
OutputDataSet <- data.frame(Name, Version);',
@input_data_1 = N'
  '
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Дополнительные сведения о необязательных полях и полях по умолчанию для поля с описанием пакета R см. в разделе [https://cran.r-project.org](https://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file).

## <a name="find-a-single-r-package"></a>Поиск одного пакета R

Если вы установили пакет R и хотите убедиться, что он доступен для конкретного экземпляра SQL Server, можно выполнить хранимую процедуру для загрузки пакета и возврата сообщений.

Например, следующая инструкция ищет и загружает пакет [glue](https://cran.r-project.org/web/packages/glue/), если он доступен.
Если пакет не удается найти или загрузить, возникает ошибка.

```sql
EXECUTE sp_execute_external_script  
  @language =N'R',
  @script=N'
require("glue")
'
```

Дополнительные сведения о пакете см. в разделе `packageDescription`.
Приведенная ниже инструкция возвращает сведения о пакете **MicrosoftML**.

```sql
EXECUTE sp_execute_external_script
  @language = N'R',
  @script = N'
print(packageDescription("MicrosoftML"))
'
```

## <a name="next-steps"></a>Дальнейшие действия

::: moniker range="<=sql-server-2017"
+ [Установка пакетов с инструментами R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Установка новых пакетов R с помощью sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end