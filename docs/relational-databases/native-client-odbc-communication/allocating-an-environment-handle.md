---
description: Выделение дескриптора среды
title: Выделение маркера среды | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, environment handles
- ODBC applications, connections
- handles [SQL Server Native Client]
- environment handles [SQLNCLI]
ms.assetid: 15c1b428-ea6d-4672-894c-f0e289e2da3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88193ccd3e9f9393829fb9816b2149dadbeb0484
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756384"
---
# <a name="allocating-an-environment-handle"></a>Выделение дескриптора среды
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Прежде чем в приложении появится возможность вызвать какую-либо функцию ODBC, необходимо инициализировать среду ODBC и выделить дескриптор среды. Это — глобальный дескриптор контекста и заполнитель для других дескрипторов в ODBC. Это делается путем вызова **функцию SQLAllocHandle** с параметром *параметром handletype* , для которого задано значение SQL_HANDLE_ENV, а *инпусандле* — значение SQL_NULL_HANDLE.  
  
 После выделения дескриптора среды приложение должно установить атрибуты среды, чтобы указать, какая версия вызовов функций ODBC будет использоваться. Для использования ODBC 3. функции *x* , вызовите [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) с параметром *Attribute* , для которого задано значение SQL_ATTR_ODBC_VERSION, а *ValuePtr* — значение SQL_OV_ODBC3.  
  
## <a name="see-also"></a>См. также:  
 [Взаимодействие с SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
