---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
ms.assetid: 017301a2-4a23-4e68-82af-134f3d4892b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17942cc1e4c68c844e78a2f25398a7e175035d01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201316"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Включает и выключает функциональность селективного XML-индекса в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При вызове без параметров хранимая процедура возвращает 1, если селективный XML-индекс включен в определенной базе данных.  
  
> [!NOTE]  
>  Чтобы отключить селективный XML-индекс с помощью этой хранимой процедуры, необходимо перевести базу данных в простой режим восстановления с помощью [параметров ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
  
      sys.sp_db_selective_xml_index[[ @db_name = ] 'db_name'],   
[[ @action = ] 'action']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @ db_name = ] 'db_name'` Имя базы данных, для которой необходимо включить или отключить селективный XML-индекс. Если *db_name* имеет значение null, предполагается текущая база данных.  
  
`[ @action = ] 'action'` Определяет, следует ли включать или отключать индекс. Если передается другое значение, кроме on, "true", "OFF" или "false", возникнет ошибка.  
  
```  
  
Allowed values: 'on', 'off', 'true', 'false'  
```  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **1** , если СЕЛЕКТИВНЫЙ XML-индекс включен в конкретной базе данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-enable-selective-xml-index-functionality"></a>A. Включение функциональности селективного XML-индекса  
 В следующем примере включается селективный XML-индекс в текущей базе данных.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'on';  
GO  
```  
  
 В следующем примере включается селективный XML-индекс в базе данных AdventureWorks2012.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>Б. Отключение возможностей селективного XML-индекса  
 В следующем примере выключается селективный XML-индекс в текущей базе данных.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = NULL  
  , @action = N'off';  
GO  
```  
  
 В следующем примере выключается селективный XML-индекс в базе данных AdventureWorks2012.  
  
```  
EXECUTE sys.sp_db_selective_xml_index  
    @db_name = N'AdventureWorks2012'  
  , @action = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>В. Включен ли селективный XML-индекс  
 В следующем примере определяется наличие селективного XML-индекса. Возвращает 1, если селективный XML-индекс включен.  
  
```  
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
  
  
