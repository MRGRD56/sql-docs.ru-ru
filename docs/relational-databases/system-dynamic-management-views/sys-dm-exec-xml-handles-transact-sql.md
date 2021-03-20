---
description: sys.dm_exec_xml_handles (Transact-SQL)
title: sys.dm_exec_xml_handles (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_xml_handles
- dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_xml_handles dynamic management function
ms.assetid: a873ce0f-6955-417a-96a1-b2ef11a83633
author: pmasl
ms.author: pelopes
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5a0169418710234983772a8623efdd0d9ff337cf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750224"
---
# <a name="sysdm_exec_xml_handles-transact-sql"></a>sys.dm_exec_xml_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Возвращает сведения об активных дескрипторах, открытых **sp_xml_preparedocument**.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dm_exec_xml_handles (session_id | 0 )  
```  
  
## <a name="arguments"></a>Аргументы  
 *session_id* | 0,0  
 Идентификатор сеанса. Если указан параметр *session_id* , эта функция возвращает сведения об дескрипторах XML в указанном сеансе.  
  
 Если указано значение 0, функция возвращает сведения обо всех дескрипторах XML во всех сеансах.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Идентификатор сеанса, содержащего данный XML-документ.|  
|**document_id**|**int**|Идентификатор XML-документа, возвращаемый **sp_xml_preparedocument**.|  
|**namespace_document_id**|**int**|Идентификатор внутреннего маркера, используемый для документа связанного пространства имен, переданного в качестве третьего параметра для **sp_xml_preparedocument**. NULL, если документ пространства имен отсутствует.|  
|**sql_handle**|**varbinary (64)**|Дескриптор текста кода SQL, в котором был определен дескриптор.|  
|**statement_start_offset**|**int**|Число символов в выполняемом в данный момент пакете или хранимой процедуре, в которой происходит вызов **sp_xml_preparedocument** . Можно использовать вместе с **sql_handle**, **statement_end_offset** и функцией динамического управления **sys.dm_exec_sql_text** , чтобы получить текущую выполняемую инструкцию для запроса.|  
|**statement_end_offset**|**int**|Число символов в выполняемом в данный момент пакете или хранимой процедуре, в которой происходит вызов **sp_xml_preparedocument** . Можно использовать вместе с **sql_handle**, **statement_start_offset** и функцией динамического управления **sys.dm_exec_sql_text** , чтобы получить текущую выполняемую инструкцию для запроса.|  
|**creation_time**|**datetime**|Отметка времени вызова **sp_xml_preparedocument** .|  
|**original_document_size_bytes**|**bigint**|Размер непроанализированного XML-документа в байтах.|  
|**original_namespace_document_size_bytes**|**bigint**|Размер непроанализированного документа пространства имен XML в байтах. NULL, если документ пространства имен отсутствует.|  
|**num_openxml_calls**|**bigint**|Число вызовов инструкции OPENXML с данным дескриптором документа.|  
|**row_count**|**bigint**|Число строк, возвращенных всеми предыдущими вызовами инструкции OPENXML для данного дескриптора документа.|  
|**dormant_duration_ms**|**bigint**|Число миллисекунд, прошедших с момента последнего вызова инструкции OPENXML. Если функция OPENXML не была вызвана, функция возвращает миллисекунды с момента вызова **sp_xml_preparedocumen** t.|  
  
## <a name="remarks"></a>Remarks  
 Время существования **sql_handles** , используемое для получения текста SQL, который выполнил вызов **sp_xml_preparedocument** в динамическом режиме кэшированный план, используемый для выполнения запроса. Если текст запроса в кэше недоступен, извлечь данные с помощью сведений, возвращаемых этой функцией, невозможно. Это может произойти при выполнении множества больших пакетов.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение VIEW SERVER STATE на сервер, чтобы просмотреть все сеансы или идентификаторы сеансов, которыми не владеет участник. Участник всегда может просмотреть данные для своего текущего идентификатора сеанса.      
  
## <a name="examples"></a>Примеры  
 В следующем примере выбираются все активные дескрипторы.  
  
```  
SELECT * FROM sys.dm_exec_xml_handles(0);  
```  
  
## <a name="see-also"></a>См. также:  
 <br>[Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
 <br>[Динамические административные представления и функции, связанные с выполнением (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-SQL)](../system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../system-stored-procedures/sp-xml-removedocument-transact-sql.md)


 
  
  
