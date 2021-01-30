---
title: sys.sp_rda_test_connection (Transact-SQL) | Документация Майкрософт
description: Узнайте, как использовать sys.sp_rda_test_connection для проверки подключения SQL Server к удаленному серверу Azure и сообщает о проблемах, которые могут препятствовать миграции данных.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_test_connection
- sys.sp_rda_test_connection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_test_connection stored procedure
ms.assetid: e2ba050c-d7e3-4f33-8281-c9b525b4edb4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e584604a0229e3e2e0c213b70d8fd4bca0321c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211717"
---
# <a name="syssp_rda_test_connection-transact-sql"></a>sys.sp_rda_test_connection (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Проверяет подключение SQL Server к удаленному серверу Azure и сообщает о проблемах, которые могут препятствовать миграции данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
EXECUTE sys.sp_rda_test_connection  
   @database_name = N'db_name',   
   @server_address = N'azure_server_fully_qualified_address',  
   @azure_username = N'azure_username',   
   @azure_password = N'azure_password',  
   @credential_name = N'credential_name'  
  
```  
  
## <a name="arguments"></a>Аргументы  
 @database_name = N '*db_name*'  
 Имя базы данных SQL Server с поддержкой растяжения. Этот параметр является необязательным.  
  
 @server_address = N '*azure_server_fully_qualified_address*'  
 Полный адрес сервера Azure.  
  
-   Если вы указали значение для **\@ database_name**, но указанная база данных не поддерживает Stretch, необходимо указать значение для **\@ server_address**.  
  
-   Если указать значение для **\@ database_name** и для указанной базы данных включено растяжение, то не нужно указывать значение для **\@ server_address**. Если указать значение для **\@ server_address**, хранимая процедура пропускает ее и использует существующий сервер Azure, уже связанный с базой данных с поддержкой Stretch.  
  
 @azure_username = N '*azure_username*  
 Имя пользователя для удаленного сервера Azure.  
  
 @azure_password = N '*azure_password*'  
 Пароль удаленного сервера Azure.  
  
 @credential_name = N '*credential_name*'  
 Вместо указания имени пользователя и пароля можно указать имя учетных данных, хранящихся в базе данных с поддержкой Stretch.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 В случае **успеха** sp_rda_test_connection возвращает ошибку 14855 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_SUCCEEDED) с уровнем серьезности EX_INFO и кодом возврата успешного выполнения.  
  
 В случае **сбоя** sp_rda_test_connection возвращает ошибку 14856 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_FAILED) с уровнем серьезности EX_USER и кодом возврата ошибки.  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|link_state|INT|Одно из следующих значений, соответствующее значениям для **link_state_desc**.<br /><br /> -0<br />-1<br />-2<br />-3<br />-4|  
|link_state_desc|varchar(32)|Одно из следующих значений, соответствующих приведенным выше значениям для **link_state**.<br /><br /> -ИСПРАВЕН<br />     Между SQL Server и удаленным сервером Azure используется работоспособность.<br />— ERROR_AZURE_FIREWALL<br />     Брандмауэр Azure препятствует связи между SQL Server и удаленным сервером Azure.<br />— ERROR_NO_CONNECTION<br />     SQL Server не удается установить подключение к удаленному серверу Azure.<br />— ERROR_AUTH_FAILURE<br />     Сбой проверки подлинности препятствует связи между SQL Server и удаленным сервером Azure.<br />-Ошибка<br />     Ошибка, не связанная с проверкой подлинности, проблемой подключения или проблемой брандмауэра, препятствует связи между SQL Server и удаленным сервером Azure.|  
|error_number|INT|Номер ошибки. Если ошибка отсутствует, это поле имеет значение NULL.|  
|error_message|nvarchar(1024)|Сообщение об ошибке. Если ошибка отсутствует, это поле имеет значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Требуются db_owner разрешения.  
  
## <a name="examples"></a>Примеры  
  
### <a name="check-the-connection-from-sql-server-to-the-remote-azure-server"></a>Проверка подключения SQL Server к удаленному серверу Azure  
  
```sql  
EXECUTE sys.sp_rda_test_connection @database_name = N'<Stretch-enabled database>'  
GO  
  
```  
  
 Результаты показывают, что SQL Server не удается подключиться к удаленному серверу Azure.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|2|ERROR_NO_CONNECTION|*\<connection-related error number>*|*\<connection-related error message>*|  
  
### <a name="check-the-azure-firewall"></a>Проверка брандмауэра Azure  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 Результаты показывают, что брандмауэр Azure не позволяет установить связь между SQL Server и удаленным сервером Azure.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|1|ERROR_AZURE_FIREWALL|*\<firewall-related error number>*|*\<firewall-related error message>*|  
  
### <a name="check-authentication-credentials"></a>Проверка учетных данных проверки подлинности  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 В результате сбоя проверки подлинности блокируется связь между SQL Server и удаленным сервером Azure.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|3|ERROR_AUTH_FAILURE|*\<authentication-related error number>*|*\<authentication-related error message>*|  
  
### <a name="check-the-status-of-the-remote-azure-server"></a>Проверка состояния удаленного сервера Azure  
  
```sql  
USE <SQL Server database>  
GO  
EXECUTE sys.sp_rda_test_connection   
    @server_address = N'<server name>.database.windows.net',   
    @azure_username = N'<user name>',   
    @azure_password = N'<password>'  
GO  
  
```  
  
 Результаты показывают, что подключение является работоспособным и можно включить Stretch Database для указанной базы данных.  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|0|HEALTHY|NULL|NULL|  
  
  
