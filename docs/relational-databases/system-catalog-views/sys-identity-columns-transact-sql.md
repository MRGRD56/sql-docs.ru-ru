---
description: sys.identity_columns (Transact-SQL)
title: sys.identity_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- identity_columns
- sys.identity_columns
- sys.identity_columns_TSQL
- identity_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.identity_columns catalog view
ms.assetid: 97ee01e6-9c9e-4fd9-884b-68b4084669d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9b0c25f48bdf66711e582929f6da34b8b105b4b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465095"
---
# <a name="sysidentity_columns-transact-sql"></a>sys.identity_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждого столбца, который является столбцом идентификаторов.  
  
 Представление **sys.identity_columns** наследует строки из представления **sys. Columns** . Представление **sys.identity_columns** возвращает столбцы в представлении **sys. Columns** , а также столбцы **seed_value**, **increment_value**, **LAST_VALUE** и **is_not_for_replication** . Дополнительные сведения см. в разделе [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<columns inherited from sys.columns>**||Представление **sys.identity_columns** возвращает все столбцы в представлении **sys. Columns** . Оно также возвращает дополнительные столбцы, описанные далее. Описание столбцов, наследуемых **sys.identity_columns** представлением из **sys. Columns**, см. в разделе [sys. Columns &#40;&#41;Transact-SQL](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).|  
|**seed_value**|**sql_variant**|Начальное значение для столбца идентификаторов. Тип данных начального значения является тем же, что и тип данных самого столбца.|  
|**increment_value**|**sql_variant**|Значение приращения для столбца идентификаторов. Тип данных начального значения является тем же, что и тип данных самого столбца.|  
|**last_value**|**sql_variant**|Последнее значение, сформированное для данного столбца идентификаторов. Тип данных начального значения является тем же, что и тип данных самого столбца.|  
|**is_not_for_replication**|**bit**|Столбец идентификаторов объявлен как NOT FOR REPLICATION. **Примечание.** Этот столбец не применяется к Azure синапсе Analytics.|  
  
> [!NOTE]  
>  Сведения о том, как создать автоматически увеличивающееся числовое значение, которое может использоваться в нескольких таблицах или вызываться из приложений без ссылки на какие-либо таблицы, см. в разделе [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
