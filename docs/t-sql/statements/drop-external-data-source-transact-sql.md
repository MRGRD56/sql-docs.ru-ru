---
description: DROP EXTERNAL DATA SOURCE (Transact-SQL)
title: DROP EXTERNAL DATA SOURCE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 3f65a2f5-a6c6-4be5-8ca4-6057078fe10e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 797a416934de76f2154d084c8a4bf964cd611c91
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478505"
---
# <a name="drop-external-data-source-transact-sql"></a>DROP EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Удаляет источник внешних данных PolyBase.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Drop an external data source  
DROP EXTERNAL DATA SOURCE external_data_source_name  
[;]  
```  
  
## <a name="arguments"></a>Аргументы  
 *external_data_source_name*  
 Удаляемое имя источника внешних данных.  
  
## <a name="metadata"></a>Метаданные  
 Чтобы просмотреть список источников внешних данных, воспользуйтесь системным представлением sys.external_data_sources.  
  
```sql  
SELECT * FROM sys.external_data_sources;  
```  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения ALTER ANY EXTERNAL DATA SOURCE.  
  
## <a name="locking"></a>Блокировка  
 Принимает совмещенную блокировку на объекте EXTERNAL DATA SOURCE.  
  
## <a name="general-remarks"></a>Общие замечания  
 При удалении источника внешних данных внешние данные не удаляются.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-basic-syntax"></a>A. Использование основного синтаксиса  
  
```sql  
DROP EXTERNAL DATA SOURCE mydatasource;  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  

