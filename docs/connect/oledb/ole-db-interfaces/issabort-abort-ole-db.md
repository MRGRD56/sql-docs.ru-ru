---
title: ISSAbort::Abort (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как метод ISSAbort::Abort отменяет текущий набор строк и все пакетные команды, связанные с текущей командой, в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAbort::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5739727f82779f2ef32d7dd8359c2c3d47f0cad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738754"
---
# <a name="issabortabort-ole-db"></a>ISSAbort::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Отменяет текущий набор строк и любые пакетные команды, ассоциированные с текущей командой.  
  
Интерфейс `ISSAbort`, доступ к которому обеспечивает драйвер OLE DB для SQL Server, предоставляет метод `ISSAbort::Abort`, используемый для отмены текущего набора строк, а также любых команд, находящихся в одном пакете с командой, первоначально создавшей этот набор строк, и еще не завершивших выполнение.  
  
 Интерфейс`ISSAbort` является специфичным для драйвера OLE DB для SQL Server. Доступ к нему можно получить с помощью метода `QueryInterface` объекта `IMultipleResults`, возвращенного методом `ICommand::Execute` или `IOpenRowset::OpenRowset`.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT Abort(void);  
```  
  
## <a name="remarks"></a>Remarks  
 Если команда, выполнение которой прерывается, принадлежит хранимой процедуре, выполнение этой хранимой процедуры (и любых вызвавших ее процедур, а также командного пакета, из которого производился вызов процедуры) будет прервано. Если сервер в это время передавал клиенту результирующий набор, эта передача будет прекращена. Если клиенту не требуется использовать результирующий набор, вызывается метод `**`ISSAbort::Abort` before releasing the rowset will speed up the rowset release, but if there is an open transaction and XACT_ABORT is ON, the transaction will be rolled back when `ISSAbort::Abort`.  
  
 После того как метод `ISSAbort::Abort` вернет результат S_OK, связанный с ним интерфейс `IMultipleResults` становится непригодным к использованию и вплоть до освобождения в ответ на вызовы любых методов (кроме методов, определенных в интерфейсе `IUnknown`) возвращает результат DB_E_CANCELED. Если из интерфейса `IRowset` до вызова метода `IMultipleResults` был получен интерфейс `Abort`, он также входит в непригодное к использованию состояние и в ответ на любые вызовы методов возвращает результат DB_E_CANCELED (кроме методов, определенных для интерфейсов `IUnknown` и `IRowset::ReleaseRows`), пока не будет освобожден успешным вызовом метода `ISSAbort::Abort`.  
  
> [!NOTE]  
>  Начиная с версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], если состояние сервера XACT_ABORT имеет значение ON, вызов метода `ISSAbort::Abort` при подключении к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] прекращает все транзакции, явные и неявные, и совершает их откат. Более ранние версии [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не прекратят текущих транзакций.  
  
## <a name="arguments"></a>Аргументы  
 Нет.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод `ISSAbort::Abort` возвращает значение S_OK, если было прервано выполнение программного пакета, и DB_E_CANTCANCEL в противном случае. Если выполнение программного пакета уже было прервано ранее, возвращается DB_E_CANCELED.  
  
 DB_E_CANCELED  
 Выполнение пакета уже было прервано.  
  
 DB_E_CANTCANCEL  
 Выполнение пакета не было прервано.  
  
 E_FAIL  
 Произошла ошибка, связанная с поставщиком. Подробные сведения можно получить при помощи интерфейса [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 Непредвиденный вызов метода. Например, объект находится в состоянии зомби, потому что метод `ISSAbort::Abort` уже был вызван.  
  
 E_OUTOFMEMORY  
 Ошибка, связанная с нехваткой памяти.  
  
