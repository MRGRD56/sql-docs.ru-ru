---
title: Получение сведений о пакете Python
description: Узнайте, как получить сведения об установленных пакетах Python, включая версии и расположения установки, в Службах машинного обучения SQL Server.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/03/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 29ed535b25d364ffa552aba20b10b446e0561cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352520"
---
# <a name="get-python-package-information"></a>Получение сведений о пакете Python

[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этой статье описывается, как получить сведения об установленных пакетах Python, включая версии и расположения установки, в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) и [кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Примеры сценариев Python показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end
::: moniker range="=sql-server-2017"
В этой статье описывается, как получить сведения об установленных пакетах Python, включая версии и расположения установки, в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md). Примеры сценариев Python показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этой статье описывается, как получить сведения об установленных пакетах Python, включая версии и расположения установки, в [Службах машинного обучения управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Примеры сценариев Python показывают, как получить сведения о пакете, например путь установки и версия.
::: moniker-end

## <a name="default-python-library-location"></a>Расположение библиотеки Python по умолчанию

При установке машинного обучения с помощью SQL Server на уровне экземпляра создается отдельная библиотека пакетов для каждого устанавливаемого языка. Библиотека экземпляров является защищенной папкой, зарегистрированной в SQL Server.

Все скрипты или код, выполняемые в базе данных на SQL Server, должны загружать функции из библиотеки экземпляров. SQL Server не может получить доступ к пакетам, установленным в других библиотеках. Это относится и к удаленным клиентам: любой код Python, выполняющийся в контексте вычислений сервера, может использовать только пакеты, установленные в библиотеке экземпляров.
Для защиты серверных ресурсов библиотека экземпляров по умолчанию может быть изменена только администратором компьютера.

::: moniker range="=sql-server-2017"
Путь по умолчанию к двоичным файлам для Python:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Предполагается, что экземпляр SQL по умолчанию — MSSQLSERVER. Если SQL Server устанавливается как определяемый пользователем именованный экземпляр, вместо него используется указанное имя.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Путь по умолчанию к двоичным файлам для Python:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES`

Предполагается, что экземпляр SQL по умолчанию — MSSQLSERVER. Если SQL Server устанавливается как определяемый пользователем именованный экземпляр, вместо него используется указанное имя.
::: moniker-end

Включите внешние скрипты, выполнив следующие команды SQL:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH override;
```

::: moniker range="=azuresqldb-mi-current"
> [!IMPORTANT]
> В Управляемом экземпляре SQL Azure выполнение команд sp_configure и RECONFIGURE приводит к перезагрузке сервера SQL для активации параметров RG. В результате сервер будет недоступен в течение нескольких секунд.
::: moniker-end

Выполните приведенную ниже инструкцию SQL, чтобы проверить библиотеку по умолчанию для текущего экземпляра. Этот пример возвращает список папок, включенных в переменную `sys.path` Python. Список содержит текущий каталог и путь к стандартной библиотеке.

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

Дополнительные сведения о переменной `sys.path` и о том, как она используется для задания пути поиска для модулей, см. в разделе [Путь поиска модуля](https://docs.python.org/2/tutorial/modules.html#the-module-search-path).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> Не пытайтесь установить пакеты Python непосредственно в библиотеке пакетов SQL с помощью **pip** или аналогичных методов. Вместо этого используйте **sqlmlutils** для установки пакетов в экземпляре SQL. Дополнительные сведения см. в статье [Установка пакетов Python с помощью sqlmlutils](install-additional-python-packages-on-sql-server.md).
::: moniker-end

## <a name="default-microsoft-python-packages"></a>Пакеты Microsoft Python по умолчанию

Приведенные ниже пакеты Python устанавливаются вместе со Службами машинного обучения SQL Server, если во время установки выбран компонент Python.

| Пакеты | Версия |  Описание |
| ---------|---------|--------------|
| [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.4.7 | Используется для удаленных контекстов вычислений, потоковой передачи, параллельного выполнения функций rx для импорта и преобразования, моделирования, визуализации и анализа данных. |
| [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.4.7 | Добавляет алгоритмы машинного обучения в Python. |

Сведения о том, какая версия Python включена, приведены в разделе [Версии Python и R](../sql-server-machine-learning-services.md#versions).

### <a name="component-upgrades"></a>Обновление компонентов

По умолчанию пакеты Python обновляются с помощью пакетов обновления и накопительных пакетов обновления. Дополнительные пакеты и обновление полных версий компонентов Python можно применять только путем обновления продукта или привязки поддержки Python к Microsoft Machine Learning Server.

Дополнительные сведения см. в разделе [Обновление компонентов R и Python в SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-python-packages"></a>Пакеты Python с открытым кодом по умолчанию

При выборе языка Python во время установки устанавливается дистрибутив Anaconda 4.2 (через Python 3.5). В дополнение к библиотекам кода Python стандартная установка включает примеры данных, модульные тесты и примеры сценариев.

> [!IMPORTANT]
> Никогда не следует вручную перезаписывать версию Python, установленную программой установки SQL Server, более новыми версиями в Интернете. Пакеты Microsoft Python основаны на конкретных версиях Anaconda. Изменение установки может привести к дестабилизации.

## <a name="list-all-installed-python-packages"></a>Просмотр всех установленных пакетов Python

Приведенный ниже пример сценария отображает список всех пакетов Python, установленных в экземпляре SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128)));
```

## <a name="find-a-single-python-package"></a>Поиск одного пакета Python

Если вы установили пакет Python и хотите убедиться, что он доступен для конкретного экземпляра SQL Server, можно выполнить хранимую процедуру, которая ищет этот пакет и отображает соответствующие сообщения.

Например, следующий код ищет пакет `scikit-learn`.
Если пакет найден, код выводит версию пакета.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
pkg_name = "scikit-learn"
try:
    version = pkg_resources.get_distribution(pkg_name).version
    print("Package " + pkg_name + " is version " + version)
except:
    print("Package " + pkg_name + " not found")
'
```

Результат:

```text
STDOUT message(s) from external script: Package scikit-learn is version 0.20.2
```

<a name="bkmk_SQLPythonVersion"></a>
## <a name="view-the-version-of-python"></a>Просмотр версии Python

Следующий пример кода возвращает версию Python, установленную в экземпляре SQL Server.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import sys
print(sys.version)
'
```

## <a name="next-steps"></a>Дальнейшие действия

::: moniker range="=sql-server-2017"
+ [Установка пакетов с инструментами Python](install-python-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Установка пакетов Python с помощью sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end