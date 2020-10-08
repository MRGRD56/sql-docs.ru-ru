---
description: sp_help_spatial_geometry_index_xml (Transact-SQL)
title: sp_help_spatial_geometry_index_xml (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_spatial_geometry_index_xml_TSQL
- sp_help_spatial_geometry_index_xml
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index_xml procedure
ms.assetid: 9668ae6d-9ed5-418e-bb9a-9e7b66f7dd16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ee3abb8e5a06d972b63140007cb8002e32837df2
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810905"
---
# <a name="sp_help_spatial_geometry_index_xml-transact-sql"></a>sp_help_spatial_geometry_index_xml (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает имена и значения для указанного набора свойств пространственного индекса **Geometry** . Можно задать возврат основного набора свойств или всех свойств индекса.  
  
 Результаты возвращаются во фрагменте XML, который отображает имена и значения выбранных свойств.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ]'{ 0 | 1 }]   
     [ , [ @query_sample = ] 'query_sample' ]   
     [ ,.[ @xml_output = ] 'xml_output' ]   
```  
  
## <a name="arguments"></a>Аргументы  
 См. раздел [аргументы и свойства хранимых процедур пространственного индекса](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="properties"></a>Свойства  
 См. раздел [аргументы и свойства хранимых процедур пространственного индекса](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Разрешения  
 Пользователь должен быть членом роли **Public** . Необходимо разрешение READ ACCESS на сервере и объекте.  
  
## <a name="remarks"></a>Комментарии  
 Свойства, которые содержат значения NULL, не включаются в набор возвращаемых значений XML.  
  
## <a name="example"></a>Пример  
 В следующем примере используется `sp_help_spatial_geometry_index_xml` для исследования пространственного индекса **SIndx_SpatialTable_geometry_col2** , определенного в таблице **geometry_col** для данного примера запроса в ** \@ QS**. В этом примере основные свойства указанного индекса возвращаются в XML-фрагменте, в котором отображаются имя и значение выбранных свойств.  
  
 Затем в результирующем наборе выполняется [запрос XQuery](../../xquery/xquery-basics.md) , возвращающий определенное свойство.  
  
```  
DECLARE @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
DECLARE @x xml;  
EXEC sp_help_spatial_geometry_index_xml 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs, @x output;  
SELECT @x.value('(/Primary_Filter_Efficiency/text())[1]', 'float');  
```  
  
 Подобно [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md), эта хранимая процедура предоставляет более простой программный доступ к свойствам пространственного индекса и сообщает результирующий набор в формате XML.  
  
## <a name="requirements"></a>Требования  
  
## <a name="see-also"></a>См. также:  
 [Аргументы и свойства хранимых процедур пространственного индекса](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)   
 [Хранимые процедуры пространственного индекса](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Пространственные данные (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Основы XQuery](../../xquery/xquery-basics.md)   
 [Справочник по языку XQuery](../../xquery/xquery-language-reference-sql-server.md)  
  
