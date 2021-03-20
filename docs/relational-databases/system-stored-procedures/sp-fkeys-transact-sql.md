---
description: sp_fkeys (Transact-SQL)
title: sp_fkeys (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fkeys
- sp_fkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fkeys
ms.assetid: 18110444-d38d-4cff-90d2-d1fc6236668b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b8bc206b830331d8abacd9cf155d68db1444419a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750024"
---
# <a name="sp_fkeys-transact-sql"></a>sp_fkeys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о логическом внешнем ключе для текущей среды. Эта процедура показывает связь по внешнему ключу, включая отключенные внешние ключи.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
sp_fkeys [ @pktable_name = ] 'pktable_name'   
     [ , [ @pktable_owner = ] 'pktable_owner' ]   
     [ , [ @pktable_qualifier = ] 'pktable_qualifier' ]   
     { , [ @fktable_name = ] 'fktable_name' }   
     [ , [ @fktable_owner = ] 'fktable_owner' ]   
     [ , [ @fktable_qualifier = ] 'fktable_qualifier' ]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @pktable_name =] "*pktable_name*"  
 Имя таблицы с первичным ключом, применяемое для возврата сведений о каталоге. Аргумент *pktable_name* имеет тип **sysname** и значение по умолчанию NULL. Сопоставление по шаблону не поддерживается. Необходимо указать этот параметр, параметр *fktable_name* или оба.  
  
 [ @pktable_owner =] "*pktable_owner*"  
 Имя владельца таблицы (с первичным ключом), используемой для возврата сведений о каталоге. Аргумент *pktable_owner* имеет тип **sysname** и значение по умолчанию NULL. Сопоставление по шаблону не поддерживается. Если *pktable_owner* не указан, применяются правила видимости таблиц по умолчанию базовой СУБД.  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если текущему пользователю принадлежит таблица с указанным именем, возвращаются столбцы этой таблицы. Если параметр *pktable_owner* не указан и текущий пользователь не владеет таблицей с указанным *pktable_name*, процедура ищет таблицу с указанным *pktable_name* владельцем базы данных. Если такая таблица существует, возвращаются ее столбцы.  
  
 [ @pktable_qualifier =] "*pktable_qualifier*"  
 Имя квалификатора таблицы (с первичным ключом). Аргумент *pktable_qualifier* имеет тип sysname и значение по умолчанию NULL. Различные продукты СУБД поддерживают имена таблиц, состоящие из трех частей (*Qualifier.Owner.Name*). В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] квалификатор представляет собой имя базы данных. В некоторых СУБД он представляет имя сервера в среде базы данных, где находится таблица.  
  
 [ @fktable_name =] "*fktable_name*"  
 Имя таблицы (с внешним ключом), применяемое для возврата сведений о каталоге. Аргумент *fktable_name* имеет тип sysname и значение по умолчанию NULL. Сопоставление по шаблону не поддерживается. Необходимо указать этот параметр, параметр *pktable_name* или оба.  
  
 [ @fktable_owner =] "*fktable_owner*"  
 Имя владельца таблицы (с внешним ключом), применяемое для возврата сведений о каталоге. Аргумент *fktable_owner* имеет тип **sysname** и значение по умолчанию NULL. Сопоставление по шаблону не поддерживается. Если *fktable_owner* не указан, применяются правила видимости таблиц по умолчанию базовой СУБД.  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если текущему пользователю принадлежит таблица с указанным именем, возвращаются столбцы этой таблицы. Если параметр *fktable_owner* не указан и текущий пользователь не владеет таблицей с указанным *fktable_name*, процедура ищет таблицу с указанным *fktable_name* владельцем базы данных. Если такая таблица существует, возвращаются ее столбцы.  
  
 [ @fktable_qualifier =] "*fktable_qualifier*"  
 Имя квалификатора таблицы (с внешним ключом). Аргумент *fktable_qualifier* имеет тип **sysname** и значение по умолчанию NULL. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] квалификатор представляет собой имя базы данных. В некоторых СУБД он представляет имя сервера в среде базы данных, где находится таблица.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|PKTABLE_QUALIFIER|**sysname**|Имя квалификатора таблицы (с первичным ключом). Это поле может иметь значение NULL.|  
|PKTABLE_OWNER|**sysname**|Имя владельца таблицы (с первичным ключом). Это поле всегда возвращает значение.|  
|PKTABLE_NAME|**sysname**|Имя таблицы (с первичным ключом). Это поле всегда возвращает значение.|  
|PKCOLUMN_NAME|**sysname**|Имя первичных ключевых столбцов для каждого столбца таблицы TABLE_NAME. Это поле всегда возвращает значение.|  
|FKTABLE_QUALIFIER|**sysname**|Имя квалификатора таблицы (с внешним ключом). Это поле может иметь значение NULL.|  
|FKTABLE_OWNER|**sysname**|Имя владельца таблицы (с внешним ключом). Это поле всегда возвращает значение.|  
|FKTABLE_NAME|**sysname**|Имя таблицы (с внешним ключом). Это поле всегда возвращает значение.|  
|FKCOLUMN_NAME|**sysname**|Имя внешнего ключевого столбца для каждого столбца таблицы TABLE_NAME. Это поле всегда возвращает значение.|  
|KEY_SEQ|**smallint**|Порядковый номер столбца в первичном ключе, состоящем из нескольких столбцов. Это поле всегда возвращает значение.|  
|UPDATE_RULE|**smallint**|Действие, совершаемое над внешним ключом, когда операция SQL является операцией обновления.  Возможные значения:<br /> 0=CASCADE; каскадное изменение в соответствии с внешним ключом.<br /> 1=NO ACTION; отсутствие изменений при наличии внешнего ключа.<br />   2 = задать значение null <br /> 3 = задать значение по умолчанию |  
|DELETE_RULE|**smallint**|Действие, совершаемое над внешним ключом, когда операция SQL является операцией удаления. Возможные значения:<br /> 0=CASCADE; каскадное изменение в соответствии с внешним ключом.<br /> 1=NO ACTION; отсутствие изменений при наличии внешнего ключа.<br />   2 = задать значение null <br /> 3 = задать значение по умолчанию |  
|FK_NAME|**sysname**|Идентификатор внешнего ключа. Возвращает NULL, если не применим к источнику данных. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает имя ограничения FOREIGN KEY.|  
|PK_NAME|**sysname**|Идентификатор первичного ключа. Возвращает NULL, если не применим к источнику данных. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает имя ограничения PRIMARY KEY.|  
  
 Возвращенные результаты сортируются по столбцам FKTABLE_QUALIFIER, FKTABLE_OWNER, FKTABLE_NAME и KEY_SEQ.  
  
## <a name="remarks"></a>Remarks  
 Кодирование приложения, в состав которого входят таблицы с отключенными внешними ключами, можно выполнять следующим образом.  
  
-   Временно отключить проверку ограничений (ALTER TABLE NOCHECK или CREATE TABLE NOT FOR REPLICATION) при работе с таблицами, потом включить ее снова.  
  
-   Использовать триггеры кода приложения для принудительного выполнения связей.  
  
Если введено имя таблицы первичных ключей, а имя таблицы внешних ключей — NULL, то процедура sp_fkeys возвращает все таблицы, в которых есть внешний ключ к данной таблице. Если введено имя таблицы внешних ключей, а имя таблицы первичных ключей — NULL, то процедура sp_fkeys возвращает все таблицы, имеющие связь «первичный-внешний ключ» с внешними ключами в таблице внешних ключей.  
  
Хранимая процедура sp_fkeys равнозначна процедуре SQLForeignKeys в ODBC.  
  
## <a name="permissions"></a>Разрешения  
 Требуется `SELECT` разрешение на схему.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере выводится список внешних ключей для таблицы `HumanResources.Department` базы данных `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_fkeys @pktable_name = N'Department'  
    ,@pktable_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере выводится список внешних ключей для таблицы `DimDate` базы данных `AdventureWorksPDW2012`. Строки не возвращаются, так как не [!INCLUDE[ssDW](../../includes/ssdw-md.md)] поддерживает внешние ключи.  
  
```sql  
EXEC sp_fkeys @pktable_name = N'DimDate';  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры каталога &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_pkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-pkeys-transact-sql.md)  
  
  

