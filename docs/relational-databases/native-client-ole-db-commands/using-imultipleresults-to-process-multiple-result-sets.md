---
description: Использование IMultipleResults для обработки нескольких результирующих наборов в SQL Server Native Client
title: IMultipleResults, несколько результирующих наборов
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- IMultipleResults interface
- multiple-rowset results
ms.assetid: 754d3f30-7d94-4b67-8dac-baf2699ce9c6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3d3d02a04d0fbe6f83d67c1da22022b3c070d32
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749004"
---
# <a name="using-imultipleresults-to-process-multiple-result-sets-in-sql-server-native-client"></a>Использование IMultipleResults для обработки нескольких результирующих наборов в SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Потребители используют интерфейс **IMultipleResults** для обработки результатов, возвращаемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнением команды поставщика OLE DB собственного клиента. Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB собственного клиента отправляет команду для выполнения, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет инструкции и возвращает все результаты.  
  
 Клиент должен обработать все результаты выполнения команды. Так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнение команды поставщика OLE DB собственного клиента может создавать объекты с несколькими наборами строк в качестве результатов, используйте интерфейс **IMultipleResults** , чтобы убедиться, что получение данных приложением завершает циклическое обращение, инициированное клиентом.  
  
 Следующая инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] формирует несколько наборов строк, при этом некоторые содержат данные строк из таблицы **OrderDetails**, а некоторые — результаты предложения COMPUTE BY:  
  
```sql
SELECT OrderID, FullPrice = (UnitPrice * Quantity), Discount,  
    Discounted = UnitPrice * (1 - Discount) * Quantity  
FROM OrderDetails  
ORDER BY OrderID  
COMPUTE  
    SUM(UnitPrice * Quantity), SUM(UnitPrice * (1 - Discount) * Quantity)  
    BY OrderID  
```  
  
 Если потребитель выполняет команду, содержащую этот текст, и запрашивает набор строк в качестве интерфейса возвращаемых результатов, возвращается только первый набор строк. Потребитель может обработать все строки в возвращенном наборе строк. Но если свойство источника данных DBPROP_MULTIPLECONNECTIONS имеет значение VARIANT_FALSE, а для соединения не включен режим MARS, никакие другие команды не могут быть выполнены в объекте сеанса ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик собственного клиента OLE DB не будет создавать другое подключение), пока команда не будет отменена. Если режим MARS не включен для соединения, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик собственного клиента OLE DB возвращает ошибку DB_E_OBJECTOPEN, если DBPROP_MULTIPLECONNECTIONS VARIANT_FALSE и возвращает E_FAIL при наличии активной транзакции.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента также возвращает DB_E_OBJECTOPEN при использовании потоковых выходных параметров, и приложение не потребляет все возвращенные значения выходного параметра перед вызовом **IMultipleResults::** GetNext для получения следующего результирующего набора. Если режим MARS не включен и соединение занято выполнением команды, которая не создает набор строк, или если свойство источника данных DBPROP_MULTIPLECONNECTIONS имеет значение VARIANT_TRUE, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB собственного клиента создает дополнительные соединения для поддержки параллельных объектов команд, если только транзакция не активна, и в этом случае возвращается ошибка. Управление транзакциями и блокировками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] производит отдельно для каждого соединения. Если создано второе соединение, команда в отдельном соединении не использует общие блокировки. Необходимо соблюдать осторожность и убедиться, что одна команда не блокирует другую, удерживая блокировки строк, запрошенных другой командой. Если включен режим MARS, в одном соединении могут быть активными несколько команд, а если выполняются явные транзакции, все команды совместно используют общую транзакцию.  
  
 Потребитель может отменить команду с помощью метода [ISSAbort::Abort](../../relational-databases/native-client-ole-db-interfaces/issabort-abort-ole-db.md) или освободив все удерживаемые ссылки на объект команды и производный набор строк.  
  
 Использование интерфейса **IMultipleResults** во всех экземплярах позволяет потребителю получить все наборы строк, сформированные командой, и соответствующим образом определить, когда нужно отменить выполнение команды, чтобы освободить объект сеанса для других команд.  
  
> [!NOTE]  
>  При использовании курсоров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнение команды создает курсор [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает успех или ошибку создания курсора, поэтому обмен данными с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] завершается по выполнении команды. Следовательно, каждый вызов **GetNextRows** становится обменом данными. Таким образом, могут существовать несколько активных объектов команд, каждая из которых обрабатывает набор строк, являющийся результатом выборки из серверного курсора. Дополнительные сведения см. в статье [Наборы строк и курсоры SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md).  
  
## <a name="see-also"></a>См. также:  
 [Команды](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
