---
title: Свойства сеанса — драйвер OLE DB для SQL Server | Документы Майкрософт
description: Узнайте, как OLE DB Driver for SQL Server интерпретирует свойства сеанса OLE DB, в том числе набор свойств, зависящих от поставщика.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- OLE DB Driver for SQL Server, sessions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326b0d09fbcf071d981db56440956393eda93e73
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755634"
---
# <a name="session-properties---ole-db-driver-for-sql-server"></a>Свойства сеанса — драйвер OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server интерпретирует свойства сеанса OLE DB следующим образом.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Драйвер OLE DB для SQL Server поддерживает все уровни изоляции транзакции с автоматической фиксацией, за исключением уровня хаоса, DBPROPVAL_TI_CHAOS.|  
  
 В специфичном для каждого поставщика множестве свойств DBPROPSET_SQLSERVERSESSION драйвер OLE DB для SQL Server определяет указанное ниже дополнительное свойство сеанса.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|SSPROP_QUOTEDCATALOGNAMES|Тип: VT_BOOL.<br /><br /> Ч/З Чтение/запись<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. В ограничении CATALOG допускаются заключенные в кавычки идентификаторы.<br /><br /> VARIANT_TRUE Заключенные в кавычки идентификаторы распознаются ограничением каталога для наборов строк схемы, предоставляющих поддержку распределенных запросов.<br /><br /> VARIANT_FALSE Заключенные в кавычки идентификаторы не распознаются ограничением каталога для наборов строк схемы, предоставляющих поддержку распределенных запросов.<br /><br /> Дополнительные сведения о наборах строк схемы, обеспечивающих поддержку распределенных запросов, см. в статье [Поддержка распределенных запросов в наборах строк схемы](../../oledb/ole-db/schema-rowsets-distributed-query-support.md).|  
|Свойство SSPROP_ALLOWNATIVEVARIANT|Тип: VT_BOOL.<br /><br /> Ч/З Чтение/запись<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. Определяет тип для извлеченных данных: DBTYPE_VARIANT или DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE: Возвращается тип столбца DBTYPE_SQLVARIANT, и в буфер помещается структура SSVARIANT.<br /><br /> VARIANT_FALSE: Возвращается тип столбца DBTYPE_VARIANT, и в буфер помещается структура VARIANT.|  
|SSPROP_ASYNCH_BULKCOPY|Для использования асинхронного режима задайте значение VARIANT_TRUE характерного для поставщика свойства SSPROP_ASYNCH_BULKCOPY сеанса до вызова метода BCPExec. Это свойство доступно в наборе свойств DBPROPSET_SQLSERVERSESSION.|  
  
## <a name="see-also"></a>См. также:  
 [Объекты источников данных (OLE DB)](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
