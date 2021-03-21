---
title: Команды, формирующие результаты с несколькими наборами строк (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как OLE DB Driver for SQL Server возвращает несколько наборов строк для пакетных инструкций SQL и когда хранимые процедуры реализуют пакетные инструкции SQL.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- OLE DB Driver for SQL Server, commands
- OLE DB Driver for SQL Server, multiple rowsets
- commands [OLE DB]
- multiple-rowset results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1389e2728ac52e6b61ddbd360cadfb99e473e146
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737805"
---
# <a name="commands-generating-multiple-rowset-results"></a>Команды, формирующие результаты с несколькими наборами строк
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server при выполнении инструкций [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может возвращать несколько наборов строк. Инструкции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] возвращают результаты, содержащие несколько наборов строк, в следующих случаях.  
  
-   Пакетные инструкции SQL представляются как единая команда.  
  
-   Хранимые процедуры реализуют пакет инструкций SQL.  
  
## <a name="batches"></a>Пакеты  
 OLE DB Driver for SQL Server распознает символ ";" (точка с запятой) как разделитель пакетов для инструкций SQL:  
  
```  
WCHAR*       wSQLString = L"SELECT * FROM Categories; "  
                          L"SELECT * FROM Products";  
```  
  
 Отправка нескольких инструкций SQL в одном пакете более эффективна, чем выполнение каждой инструкции SQL по отдельности. Отправка одного пакета уменьшает количество циклов приема-передачи данных с клиента на сервер.  
  
## <a name="stored-procedures"></a>Хранимые процедуры  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] возвращает результирующий набор для каждой инструкции в хранимой процедуре, поэтому большинство хранимых процедур [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] возвращает несколько результирующих наборов.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Обработка нескольких результирующих наборов с помощью интерфейса IMultipleResults](../../oledb/ole-db-commands/using-imultipleresults-to-process-multiple-result-sets.md)  
  
## <a name="see-also"></a>См. также:  
 [Команды](../../oledb/ole-db-commands/commands.md)  
  
  
