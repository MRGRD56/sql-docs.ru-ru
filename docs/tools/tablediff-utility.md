---
title: tablediff, программа
description: Используйте служебную программу tablediff для определения расхождений данных между двумя таблицами и устранения этих расхождений в топологии репликации.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- comparing data
- tablediff utility
- tables [SQL Server replication]
- table comparisons [SQL Server]
- command prompt utilities [SQL Server], tablediff
- troubleshooting [SQL Server replication], non-convergence
- non-convergence [SQL Server]
ms.assetid: 3c3cb865-7a4d-4d66-98f2-5935e28929fc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9054ebafe95fdfc1ab07e868f1d8e9233aee1795
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335944"
---
# <a name="tablediff-utility"></a>tablediff, программа
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Служебная программа **tablediff** используется для сравнения данных в двух таблицах на расхождение и особенно полезна для устранения несоответствия данных в топологии репликации. Эта программа может запускаться из командной строки или из пакетного файла и служит для выполнения следующих задач:  
  
-   Построчное сравнение исходной таблицы в экземпляре [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], выступающем в качестве издателя репликации, и целевой таблицы в одном или нескольких экземплярах [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], выступающих в качестве подписчиков репликации.  
  
-   Быстрое сравнение, сравнивающее только схемы и количество строк.  
  
-   Сравнение на уровне столбцов.  
  
-   Формирование скрипта [!INCLUDE[tsql](../includes/tsql-md.md)] для исправления несоответствий на целевом сервере и обеспечения конвергенции исходной и целевой таблиц.  
  
-   Запись результатов операции в файл вывода или в таблицу целевой базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
tablediff   
[ -? ] |   
{  
        -sourceserver source_server_name[\instance_name]  
        -sourcedatabase source_database  
        -sourcetable source_table_name   
    [ -sourceschema source_schema_name ]  
    [ -sourcepassword source_password ]  
    [ -sourceuser source_login ]  
    [ -sourcelocked ]  
        -destinationserver destination_server_name[\instance_name]  
        -destinationdatabase subscription_database   
        -destinationtable destination_table   
    [ -destinationschema destination_schema_name ]  
    [ -destinationpassword destination_password ]  
    [ -destinationuser destination_login ]  
    [ -destinationlocked ]  
    [ -b large_object_bytes ]   
    [ -bf number_of_statements ]   
    [ -c ]   
    [ -dt ]   
    [ -et table_name ]   
    [ -f [ file_name ] ]   
    [ -o output_file_name ]   
    [ -q ]   
    [ -rc number_of_retries ]   
    [ -ri retry_interval ]   
    [ -strict ]  
    [ -t connection_timeouts ]   
}  
```  
  
## <a name="arguments"></a>Аргументы  
 [ **-?** ]  
 Возвращает список поддерживаемых параметров.  
  
 **-sourceserver** _source_server_name_[ **\\** _instance\_name_]  
 Имя исходного сервера. Укажите *имя_исходного_сервера* для экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]по умолчанию. Укажите _имя_исходного_сервера_ **\\** _имя_экземпляра_ для именованного экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-sourcedatabase** _source_database_  
 Имя базы данных-источника.  
  
 **-sourcetable** _source_table_name_  
 Имя проверяемой исходной таблицы.  
  
 **-sourceschema** _source_schema_name_  
 Владелец схемы исходной таблицы. Владельцем таблицы по умолчанию считается dbo.  
  
 **-sourcepassword** _source_password_  
 Пароль для имени входа, используемого для подключения к исходному серверу с помощью проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  По возможности указывайте учетные данные во время выполнения. Если необходимо хранить учетные данные в файле скрипта, необходимо обеспечить его безопасность, чтобы предотвратить несанкционированный доступ.  
  
 **-sourceuser** _source_login_  
 Имя входа, используемое для подключения к исходному серверу с помощью проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Если *исходное_имя_входа* не указано, для соединения с исходным сервером используется проверка подлинности Windows. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-sourcelocked**  
 Исходная таблица блокируется в ходе сравнения при помощи табличных подсказок TABLOCK и HOLDLOCK.  
  
 **-destinationserver** _destination_server_name_[ **\\** _instance_name_]  
 Имя целевого сервера. Укажите *имя_целевого_сервера* для экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]по умолчанию. Укажите _имя_целевого_сервера_ **\\** _имя_экземпляра_ для именованного экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-destinationdatabase** _subscription_database_  
 Имя целевой базы данных.  
  
 **-destinationtable** _destination_table_  
 Имя целевой таблицы.  
  
 **-destinationschema** _destination_schema_name_  
 Владелец схемы целевой таблицы. Владельцем таблицы по умолчанию считается dbo.  
  
 **-destinationpassword** _destination_password_  
 Пароль для имени входа, используемого для подключения к целевому серверу с помощью проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  По возможности указывайте учетные данные во время выполнения. Если необходимо хранить учетные данные в файле скрипта, необходимо обеспечить его безопасность, чтобы предотвратить несанкционированный доступ.  
  
 **-destinationuser** _destination_login_  
 Имя входа, используемое для подключения к целевому серверу с помощью проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Если *целевое_имя_входа* не указано, для соединения с исходным сервером используется проверка подлинности Windows. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-destinationlocked**  
 Целевая таблица блокируется в ходе сравнения при помощи табличных указаний TABLOCK и HOLDLOCK.  
  
 **-b** _large_object_bytes_  
 Число байтов для сравнения столбцов, содержащих данные типа больших объектов, к которым относятся: **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** и **varbinary(max)** . *число_байтов_больших_объектов* по умолчанию имеет размер столбца. Любые данные, размер которых превышает значение *число_байтов_больших_объектов* , не учитываются при сравнении.  
  
 **-bf**  _number_of_statements_  
 Число инструкций [!INCLUDE[tsql](../includes/tsql-md.md)] для записи в текущий файл скрипта [!INCLUDE[tsql](../includes/tsql-md.md)] при использовании параметра **-f** . Когда число инструкций [!INCLUDE[tsql](../includes/tsql-md.md)] превышает значение *число_инструкций*, создается новый файл скрипта [!INCLUDE[tsql](../includes/tsql-md.md)] .  
  
 **-c**  
 Сравнение на уровне столбцов.  
  
 **-dt**  
 Удаление таблицы результатов, указанной в аргументе *имя_таблицы*, если она уже существует.  
  
 **-et** _table_name_  
 Имя создаваемой таблицы результатов. Если таблица уже существует, необходимо использовать аргумент **-DT** , иначе операция завершится ошибкой.  
  
 **-f** [ *имя_файла* ]  
 Формирует скрипт [!INCLUDE[tsql](../includes/tsql-md.md)] , по которому обеспечивается конвергенция таблицы на целевом сервере и таблицы на исходном сервере. Можно дополнительно указать имя и путь для создаваемого файла скрипта [!INCLUDE[tsql](../includes/tsql-md.md)] . Если параметр *имя_файла* не указан, файл скрипта [!INCLUDE[tsql](../includes/tsql-md.md)] создается в каталоге, в котором запущена данная программа.  
  
 **-o** _output_file_name_  
 Полное имя и путь файла вывода.  
  
 **-q**  
 Быстрое сравнение, сравнивающее только схемы и количество строк.  
  
 **-rc** _number_of_retries_  
 Количество попыток повтора программой неудачно завершившейся операции.  
  
 **-ri**  _retry_interval_  
 Интервал в секундах между повторными попытками.  
  
 **-strict**  
 Строгая проверка исходной и целевой схем.  
  
 **-t** _connection_timeouts_  
 Устанавливает время ожидания в секундах для соединений с исходным сервером и целевым сервером.  
  
## <a name="return-value"></a>Возвращаемое значение  
  
|Значение|Описание|  
|-----------|-----------------|  
|**0**|Успешно|  
|**1**|Критическая ошибка|  
|**2**|Различия таблиц|  
  
## <a name="remarks"></a>Remarks  
 Служебную программу **tablediff** нельзя использовать для обращения к серверам, отличным от [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Таблицы со столбцами с данными типа **sql_variant** не поддерживаются.  
  
 По умолчанию служебная программа **tablediff** поддерживает перечисленные ниже сопоставления типов данных между исходными и целевыми столбцами.  
  
|Тип данных источника|Тип данных назначения|  
|----------------------|---------------------------|  
|**tinyint**|**smallint**, **int** или **bigint**|  
|**smallint**|**int** или **bigint**|  
|**int**|**bigint**|  
|**timestamp**|**varbinary**|  
|**varchar(max)**|**text**|  
|**nvarchar(max)**|**ntext**|  
|**varbinary(max)**|**image**|  
|**text**|**varchar(max)**|  
|**ntext**|**nvarchar(max)**|  
|**image**|**varbinary(max)**|  
  
 Параметр **-strict** запрещает такое сопоставление и выполняет строгую проверку.  
  
 Исходная таблица при данном сравнении должна содержать хотя бы один столбец первичного ключа, столбец идентификаторов или столбец ROWGUID. При использовании параметра **-strict** целевая таблица также должна содержать столбец первичного ключа, столбец идентификаторов или столбец ROWGUID.  
  
 Скрипт [!INCLUDE[tsql](../includes/tsql-md.md)] , создаваемый для приведения целевой таблицы в состояние конвергенции, не включает следующие типы данных:  
  
-   **varchar(max)**  
  
-   **nvarchar(max)**  
  
-   **varbinary(max)**  
  
-   **timestamp**  
  
-   **xml**  
  
-   **text**  
  
-   **ntext**  
  
-   **image**  
  
## <a name="permissions"></a>Разрешения  
 Для сравнения таблиц на сравниваемые объекты таблиц необходимо иметь разрешения SELECT ALL.  
  
 Для использования параметра **-et** необходимо быть членом предопределенной роли базы данных db_owner или как минимум иметь разрешение CREATE TABLE в базе данных подписки и разрешение ALTER на схему конечного владельца на целевом сервере.  
  
 Для использования параметра **-dt** необходимо быть членом предопределенной роли базы данных db_owner или как минимум иметь разрешение ALTER на схему конечного владельца на целевом сервере.  
  
 Чтобы использовать параметр **-o** или **-f** , необходимо иметь разрешения на запись в указанный каталог файлов.  
  
## <a name="see-also"></a>См. также:  
 [Сравнение реплицируемых таблиц на предмет различий (программирование репликации)](../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)  
  
  
