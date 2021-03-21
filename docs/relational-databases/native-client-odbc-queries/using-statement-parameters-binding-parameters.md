---
title: Параметры привязки | Документация Майкрософт
description: Узнайте, как привязать каждый маркер параметра в инструкции SQL к переменной в приложении перед выполнением инструкции.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, parameters
- parameters [SQL Server Native Client], ODBC
- statements [ODBC], parameters
- binding parameters [SQL Server Native Client]
- parameter markers [ODBC]
- unbound parameter markers
- SQLBindParameter function
- ODBC applications, parameters
- bound parameter markers [SQL Server Native Client]
ms.assetid: d6c69739-8f89-475f-a60a-b2f6c06576e2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3727d8e83174c6938d7aa871770bbb3d22020ef6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752804"
---
# <a name="using-statement-parameters---binding-parameters"></a>Использование параметров инструкции — привязка параметров
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Каждый маркер параметра в инструкции SQL должен быть сопоставлен переменной в приложении (привязан к ней), прежде чем можно выполнить инструкцию. Это делается путем вызова функции [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) . **SQLBindParameter** описывает переменную программы (адрес, тип данных C и т. д.) для драйвера. Она также определяет маркер параметра, указывая его порядковое значение, а затем описывая характеристики представляемого им объекта SQL (тип данных SQL, точность и т. д.).  
  
 Маркеры параметров могут быть привязаны или повторно привязаны в любое время перед выполнением инструкции. Привязка параметра действует до тех пор, пока не происходит одно из следующих событий.  
  
-   Вызов [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) с параметром *Option* , равным SQL_RESET_PARAMS освобождает все параметры, привязанные к маркеру инструкции.  
  
-   Вызов **SQLBindParameter** с *параметернумбер* , равным порядковому номеру привязанного параметра, автоматически освобождает предыдущую привязку.  
  
 Приложение также может привязать параметры к массивам переменных программы для пакетной обработки инструкции SQL. Существует два типа привязки массивов.  
  
-   Привязка на уровне столбца выполняется, если каждый отдельный параметр привязан к собственному массиву переменных.  
  
     Привязка на уровне столбца задается путем вызова [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) с *атрибутом* , для которого задано значение SQL_ATTR_PARAM_BIND_TYPE, а *ValuePtr* — значение SQL_PARAM_BIND_BY_COLUMN.  
  
-   Привязка на уровне строки выполняется, если все параметры в инструкции SQL привязаны в виде блока к массиву структур, которые содержат отдельные переменные для параметров.  
  
     Привязка на уровне строки задается путем вызова **SQLSetStmtAttr** с *атрибутом* , для которого задано значение SQL_ATTR_PARAM_BIND_TYPE, а *ValuePtr* задает размер структуры, содержащей переменные программы.  
  
 Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента отправляет на сервер символьные или двоичные строковые параметры, он дополняет значения длиной, указанной в параметре *ColumnSize* **SQLBindParameter** . Если приложение ODBC 2. x указывает 0 для *ColumnSize*, драйвер устанавливает значение параметра в точность типа данных. Точность равна 8000 при соединении с сервером [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и 255 при соединении с предыдущими версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *ColumnSize* находится в байтах для столбцов типа Variant.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает определение имен для параметров хранимых процедур. В ODBC 3.5 также появилась поддержка именованных параметров, используемых при вызове хранимых процедур [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Эта поддержка может использоваться для следующих действий.  
  
-   Вызов хранимой процедуры и предоставление значений для подмножества параметров, заданных для хранимой процедуры.  
  
-   Указание параметров в приложении не в той последовательности, в какой они были заданы при создании хранимой процедуры.  
  
 Именованные параметры поддерживаются только при использовании [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции **EXECUTE** или escape-последовательности ODBC CALL для выполнения хранимой процедуры.  
  
 Если для параметра хранимой процедуры задано значение **SQL_DESC_NAME** , все параметры хранимых процедур в запросе также должны устанавливаться **SQL_DESC_NAME**.  Если литералы используются в вызовах хранимых процедур, где параметры имеют **SQL_DESC_NAME** заданы, литералы должны использовать формат *"имя* = *значение*", где *Name* — имя параметра хранимой процедуры (например, @p1 ). Дополнительные сведения см. в разделе [Привязка параметров по имени (именованные параметры)](../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md).  
  
## <a name="see-also"></a>См. также:  
 [Использование параметров инструкции](../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)  
  
