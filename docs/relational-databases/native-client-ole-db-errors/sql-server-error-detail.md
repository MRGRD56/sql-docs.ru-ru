---
description: Сведения об ошибке SQL Server Native Client
title: SQL Server сведения об ошибке (поставщик собственного клиента OLE DB)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, errors
- errors [OLE DB], error details
- IErrorRecords interface
- IErrorInfo interface
- OLE DB error handling, error details
- ISQLServerErrorInfo interface
ms.assetid: 51500ee3-3d78-47ec-b90f-ebfc55642e06
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eac31d0cec00655071aa84561c2f0f77f8e681ce
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738824"
---
# <a name="sql-server-native-client-error-detail"></a>Сведения об ошибке SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента определяет определяемый поставщиком интерфейс ошибок [ISQLServerErrorInfo](../native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). Интерфейс возвращает более подробные сведения об ошибке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и полезен, если выполнение команды или операции работы с наборами строк завершились с ошибкой.  
  
 Существует два способа получения доступа к интерфейсу **ISQLServerErrorInfo**.  
  
 Как показано в приведенном ниже образце кода, потребитель может вызвать метод **IErrorRecords::GetCustomerErrorObject**, чтобы получить указатель на интерфейс **ISQLServerErrorInfo**. (Нет необходимости получать интерфейс **ISQLErrorInfo**.) Интерфейсы **ISQLErrorInfo** и **ISQLServerErrorInfo** представляют собой пользовательские объекты ошибок OLE DB. При этом объект **ISQLServerErrorInfo** является интерфейсом для получения информации об ошибках на сервере, включая такие данные, как имя процедуры и номера строк.  
  
```  
// Get the SQL Server custom error object.  
if(FAILED(hr=pIErrorRecords->GetCustomErrorObject(  
   nRec, IID_ISQLServerErrorInfo,  
   (IUnknown**)&pISQLServerErrorErrorInfo)))  
```  
  
 Другой способ получить указатель на интерфейс **ISQLServerErrorInfo** состоит в вызове метода **QueryInterface** для уже полученного указателя на интерфейс **ISQLErrorInfo**. Обратите внимание на то, что так как интерфейс **ISQLServerErrorInfo** содержит надмножество информации, доступной из интерфейса **ISQLErrorInfo**, имеет смысл получить непосредственно интерфейс **ISQLServerErrorInfo** с помощью метода **GetCustomerErrorObject**.  
  
 Интерфейс **ISQLServerErrorInfo** предоставляет одну функцию-член [ISQLServerErrorInfo::GetErrorInfo](../../relational-databases/native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md). Функция возвращает указатель на структуру SSERRORINFO и указатель на буфер строк. Оба указателя ссылаются на память, которую потребитель должен освободить с помощью метода **IMalloc::Free**.  
  
 Элементы структуры SSERRORINFO обрабатываются потребителем следующим образом.  
  
|Участник|Description|  
|------------|-----------------|  
|*pwszMessage*|Сообщение об ошибке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Идентично строке, возвращаемой методом **IErrorInfo::GetDescription**.|  
|*pwszServer*|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для сеанса.|  
|*pwszProcedure*|При необходимости, имя процедуры, в которой произошла ошибка. Пустая строка в противном случае.|  
|*lNative*|Номер собственной ошибки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Идентичен значению, возвращаемому в параметре *plNativeError* метода **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|Состояние сообщения об ошибке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*bClass*|Серьезность сообщения об ошибке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Если применимо, номер строки хранимой процедуры, в которой возникла ошибка.|  
  
## <a name="see-also"></a>См. также:  
 [Ошибки](../../relational-databases/native-client-ole-db-errors/errors.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
