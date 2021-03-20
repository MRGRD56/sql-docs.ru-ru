---
title: Коды возврата (поставщик собственного клиента OLE DB)
description: Сведения о кодах возврата, поддерживаемых для SQL Server Native Client OLE DB, включая часто встречающиеся DB_S_ERRORSOCCURRED значения HRESULT.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB error handling, return codes
- SQL Server Native Client OLE DB provider, errors
- failed function [OLE DB]
- successful function [OLE DB]
- S_FALSE
- IS_ERROR macro
- DB_S_ERRORSOCCURRED return
- return codes [OLE DB]
- S_OK
- FAILED macro
- errors [OLE DB], return codes
ms.assetid: 7f7457e9-fce4-400c-82e5-ee02e9e811c6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d665e65d8dae3a6b114486e77161077ad8c74e6b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738624"
---
# <a name="return-codes-native-client-ole-db-provider"></a>Коды возврата (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Для краткости можно сказать, что вызов функции-члена класса или структуры завершается успешно либо с ошибкой. Точнее сказать, вызов функции может оказаться успешным, но результат оказывается не таким, на какой рассчитывал разработчик приложения.  
  
 Дополнительные сведения о кодах возврата OLE DB см. в статье [Коды возврата (OLE DB)](/previous-versions/windows/desktop/ms725451(v=vs.85)).  
  
 Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функция-член поставщика собственного клиента OLE DB возвращает S_OK, функция прошла удачно.  
  
 Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функция-член поставщика собственного клиента OLE DB не возвращает S_OK, ошибка HRESULT OLE/COM-распаковки, а IS_ERROR макросы могут определить общую успешность или сбой функции.  
  
 Если сбой или IS_ERROR возвращает значение TRUE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребитель поставщика OLE DB собственного клиента гарантирует, что выполнение функции-члена завершилось ошибкой. Если сбой или IS_ERROR возвращают значение FALSE, а значение HRESULT не равно S_OK, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребитель поставщика OLE DB собственного клиента гарантирует, что функция была успешно выполнена в определенном смысле. Потребитель может получить подробную информацию о возврате данных об успешном выполнении с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственного клиента OLE DB интерфейсах ошибок поставщика. Кроме того, в случае, когда функция явно завершается неудачей (макрос FAILed возвращает значение TRUE), расширенные сведения об ошибке можно получить из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственного клиента OLE DB интерфейсы ошибок поставщика.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Потребители собственного клиента OLE DB поставщика обычно сталкиваются с возвратом DB_S_ERRORSOCCURRED "Success with Information". Функции-члены, возвращающие значение DB_S_ERRORSOCCURRED, обычно определяют один или несколько параметров, предоставляющих потребителю значения состояния. Информация, возвращаемая через параметры состояния, может быть единственной информацией о состоянии, доступной потребителю; поэтому для получения значений состояния, когда они доступны, потребители должны реализовать собственную логику приложения.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Функции члена поставщика собственного клиента OLE DB не возвращают код успешного выполнения S_FALSE. Все [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функции члена поставщика собственного клиента OLE DB всегда возвращают S_OK, чтобы указать на успешное выполнение.  
  
## <a name="see-also"></a>См. также:  
 [ошибки](../../relational-databases/native-client-ole-db-errors/errors.md)  
  
