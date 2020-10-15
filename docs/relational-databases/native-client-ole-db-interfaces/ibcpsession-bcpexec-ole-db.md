---
description: 'IBCPSession:: BCPExec (поставщик собственного клиента OLE DB)'
title: 'IBCPSession:: BCPExec (поставщик собственного клиента OLE DB) | Документация Майкрософт'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPExec (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPExec method
ms.assetid: 0f4ebb63-cf03-4e53-846e-6c3021cde007
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8c27132d76b38f66c7b73251e6ce6ffe0339d4c8
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92081663"
---
# <a name="ibcpsessionbcpexec-native-client-ole-db-provider"></a>IBCPSession:: BCPExec (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Выполняет операцию массового копирования.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT BCPExec(   
      DBROWCOUNT *pRowsCopied);  
```  
  
## <a name="remarks"></a>Remarks  
 Метод **BCPExec** копирует данные из пользовательского файла в таблицу базы данных или наоборот, в зависимости от значения параметра *eDirection*, используемого в методе [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
 Перед вызовом **BCPExec**вызовите метод **BCPInit** , передав ему допустимое имя файла пользователя. Несоблюдение этого правила приведет к ошибке. Единственное исключение — использование запроса для операции массового копирования из базы данных. В этом случае указывается имя таблицы NULL в методе **BCPInit** , а затем задается запрос, использующий параметр BCP_OPTION_HINTS.  
  
 Метод **BCPExec** — единственный метод массового копирования, который с большой вероятностью может ожидать выполнения в течение любого периода времени. Таким образом, это единственный метод массового копирования, который поддерживает асинхронный режим. Для использования асинхронного режима задайте значение VARIANT_TRUE для свойства сеанса поставщика SSPROP_ASYNCH_BULKCOPY перед вызовом метода **BCPExec** . Это свойство доступно в наборе свойств DBPROPSET_SQLSERVERSESSION. Чтобы проверить завершение операции, вызовите метод **BCPExec** с теми же параметрами. Если массовое копирование еще не завершено, метод **BCPExec** возвращает DB_S_ASYNCHRONOUS. Он также возвращает в аргументе *pRowsCopied* состояние счетчика строк, переданных на сервер или полученных от него. Строки, отправленные на сервер, не фиксируются до тех пор, пока не будет достигнут конец пакета.  
  
## <a name="arguments"></a>Аргументы  
 *pRowsCopied*[out]  
 Указатель на значение типа DWORD. Метод **BCPExec** записывает в значение DWORD количество успешно скопированных строк. Если для аргумента *pRowsCopied* задано значение NULL, то он пропускается методом **BCPExec** .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно.  
  
 E_FAIL  
 Произошла ошибка, связанная с поставщиком. Подробные сведения можно получить при помощи интерфейса [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 Непредвиденный вызов метода. Например, перед вызовом этого метода не был вызван метод **BCPInit** . Также возникает, если операция была прервана с использованием параметра BCP_OPTION_ABORT, а затем был вызван метод **BCPExec** .  
  
 E_OUTOFMEMORY  
 Недостаточно памяти.  
  
 DB_S_ENDOFROWSET  
 Операция массового копирования завершена, и вся передача всех данных выполнена.  
  
 DB_S_ASYNCHRONOUS  
 Текущий пакет строк скопирован. Вновь вызовите метод **BCPExec** , чтобы передать следующий пакет.  
  
 DB_S_ERRORSOCCURRED  
 Во время операции массового копирования произошли ошибки, и некоторые строки могли быть не скопированы. Количество ошибок все еще меньше минимально допустимого числа ошибок.  
  
## <a name="see-also"></a>См. также:  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [Выполнение операций массового копирования](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
