---
description: bcp_sendrow
title: bcp_sendrow | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_sendrow
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_sendrow function
ms.assetid: ddbdb4bd-ad4e-4bf1-9a75-656aa26ce10a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2e0b26253dc95402d6f5113d1248327e0f49a287
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749514"
---
# <a name="bcp_sendrow"></a>bcp_sendrow
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Отправляет строку данных из переменных программы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
RETCODE bcp_sendrow (  
    HDBC hdbc);  
```  
  
## <a name="arguments"></a>Аргументы  
 *hdbc*  
 Дескриптор соединения ODBC с поддержкой массового копирования.  
  
## <a name="returns"></a>Возвращаемое значение  
 SUCCEED или FAIL.  
  
## <a name="remarks"></a>Remarks  
 Функция **bcp_sendrow** формирует строку из переменных программы и отправляет ее в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Перед вызовом функции **bcp_sendrow** необходимо вызвать функцию [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) , чтобы указать переменные программы, содержащие данные строки.  
  
 Если **bcp_bind** вызывается с указанием типа данных Long, переменной длины, например параметра *eDataType* SQLTEXT и параметра *pData* , отличного от NULL, **bcp_sendrow** отправляет все значение данных точно так же, как и для любого другого типа данных. Однако, если в функции **bcp_bind** параметр *pData* имеет значение NULL, функция **bcp_sendrow** возвращает управление приложению сразу после того, как все указанные столбцы с данными отправляются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Затем приложение может повторно вызывать функцию [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md) , чтобы отправлять данные большого объема переменной длины в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]по одному фрагменту данных за раз. Дополнительные сведения см. в разделе [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md).  
  
 Если функция **bcp_sendrow** используется для массового копирования строк из переменных программы в таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , строки фиксируются только при вызове пользователем функции [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) или [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). Пользователь может вызывать функцию **bcp_batch** один раз для каждой из *n* строк или во время перерыва между периодами поступления данных. Если функция **bcp_batch** никогда не вызывается, то строки фиксируются при вызове функции **bcp_done** .  
  
 Дополнительные сведения о критическом изменении в разделе Выполнение операций с массовым копированием, начиная с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , см. в статье [осуществление операции копирования &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
