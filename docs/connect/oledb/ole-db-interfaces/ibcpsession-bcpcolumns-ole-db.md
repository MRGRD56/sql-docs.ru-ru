---
title: IBCPSession::BCPColumns (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как метод IBCPSession::BCPColumns задает количество полей, привязываемых к столбцам в таблице SQL Server в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPColumns (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColumns method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cae0f35bc45c17a9d2518814a7cbfd1e514411db
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748764"
---
# <a name="ibcpsessionbcpcolumns-ole-db"></a>IBCPSession::BCPColumns (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Задает количество полей для привязки к столбцам в таблице [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT BCPColumns(   
      DBCOUNTITEM nColumns);  
```  
  
## <a name="remarks"></a>Remarks  
 Этот метод совершает внутренний вызов метода [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) для установки значений по умолчанию для полей данных. Эти значения по умолчанию получаются из информации о столбце SQL Server, которую внутренним образом возвращает поставщик, когда имя таблицы указывается через [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
> [!NOTE]  
>  Данный метод можно вызывать только после того, как вызван метод **BCPInit** с допустимым именем файла.  
  
 Этот метод следует вызывать только в случае, когда планируется использовать нестандартный формат пользовательского файла. Дополнительную информацию об описании стандартного формата пользовательского файла см. в справке по методу **BCPInit** .  
  
 После вызова метода **BCPColumns** следует вызвать метод **BCPColFmt** для каждого столбца в пользовательском файле, чтобы полностью описать нестандартный формат файла.  
  
## <a name="arguments"></a>Аргументы  
 *nColumns*[in]  
 Общее число полей в пользовательском файле. Даже если предполагается массовое копирование данных из пользовательского файла в таблицу SQL Server и не предполагается копирование всех полей в пользовательском файле, аргументу *nColumns* следует присвоить значение, равное общему числу полей в пользовательском файле. Затем с помощью свойства **BCPColFmt** можно указать поля, которые нужно пропустить.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно.  
  
 E_FAIL  
 Произошла ошибка, связанная с поставщиком. Подробные сведения можно получить при помощи интерфейса [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 Непредвиденный вызов метода. Например, перед вызовом этого метода не был вызван метод **BCPInit** . Это значение возвращается также, если данный метод был вызван несколько раз для операции массового копирования.  
  
 E_OUTOFMEMORY  
 Недостаточно памяти.  
  
## <a name="see-also"></a>См. также:  
 [IBCPSession (OLE DB)](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Выполнение операций массового копирования](../../oledb/features/performing-bulk-copy-operations.md)  
  
