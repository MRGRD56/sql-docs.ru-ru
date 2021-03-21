---
description: bcp_collen
title: bcp_collen | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_collen
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_collen function
ms.assetid: faaf1f7a-81f2-4852-a178-56602c33673a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd081df01f461b98a64ea7185b4aefb03d2f8661
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749614"
---
# <a name="bcp_collen"></a>bcp_collen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Устанавливает длину данных в переменной программы для текущего массового копирования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
RETCODE bcp_collen (  
        HDBC hdbc,  
        DBINT cbData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Аргументы  
 *hdbc*  
 Дескриптор соединения ODBC с поддержкой массового копирования.  
  
 *cbData*  
 Длина данных в переменной программы, не включая длину признака длины или конца данных. Установка параметра *cbData* в значение SQL_NULL_DATA означает все строки, скопированные на сервер, которые содержат в столбце значение NULL. Установка в значение SQL_VARLEN_DATA означает, что для определения длины скопированных данных используется признак конца строки или другой метод. Если существует как признак длины, так и признак конца, система использует результат с меньшим количеством скопированных данных.  
  
 *idxServerCol*  
 Порядковый номер столбца таблицы, в которую копируются данные. Первый столбец имеет номер 1. Порядковый номер столбца возвращается функцией [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Возвращаемое значение  
 SUCCEED или FAIL.  
  
## <a name="remarks"></a>Remarks  
 Функция **bcp_collen** позволяет изменять для определенного столбца длину данных в переменной программы при копировании данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью функции [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
 Первоначальная длина данных определяется при вызове функции [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) . Если длина данных изменяется между вызовами функции **bcp_sendrow** и не используется ни одного префикса длины или признака конца, то для сброса длины можно вызвать **bcp_collen** . При следующем вызове функции **bcp_sendrow** используется длина, заданная функцией **bcp_collen**.  
  
 Для каждого столбца таблицы, длину данных которого нужно изменить, необходимо вызвать функцию **bcp_collen** .  
  
## <a name="see-also"></a>См. также:  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
