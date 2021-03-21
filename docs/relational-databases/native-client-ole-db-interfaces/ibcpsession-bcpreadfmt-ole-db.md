---
description: 'IBCPSession:: BCPReadFmt (поставщик собственного клиента OLE DB)'
title: 'IBCPSession:: BCPReadFmt (поставщик собственного клиента OLE DB) | Документация Майкрософт'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPReadFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPReadFmt method
ms.assetid: e2a12050-94e4-48a3-8a48-b780d646f116
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5c47673684c13f79bcf762e2c7fba7a2231012a9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749364"
---
# <a name="ibcpsessionbcpreadfmt-native-client-ole-db-provider"></a>IBCPSession:: BCPReadFmt (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Считывает сведения о формате для каждого столбца из файла форматирования.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT BCPReadFmt(   
      const wchar_t *pwszFormatFile);  
```  
  
## <a name="remarks"></a>Remarks  
 Метод **BCPReadFmt** используется для считывания данных из файла форматирования, указывающего формат данных в файле данных. Данный метод способен определить правильную версию файла форматирования. Он может автоматически определить, в каком формате находится файл форматирования — XML или формат текста по старому стилю, —и действовать соответствующим образом. Программа BCP поставщика OLE DB собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает версии файла форматирования 6.0 и следующие.  
  
 После того как метод **BCPReadFmt** считывает значения формата, он выполняет соответствующие вызовы методов [IBCPSession::BCPColumns](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) и [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md). Пользователю не требуется производить анализ файла форматирования и выполнять эти вызовы.  
  
 Чтобы сохранить файл форматирования, вызовите метод [IBCPSession::BCPWriteFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md). Вызовы метода **BCPReadFmt** могут ссылаться на сохраненные форматы. Кроме того, программа массового копирования (**bcp**) может сохранять определяемые пользователем форматы данных в файлах, на которые может ссылаться метод **BCPReadFmt** .  
  
 Значение **BCP_OPTION_DELAYREADFMT** для параметра *eOption* в [IBCPSession::BCPControl](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) изменяет поведение IBCPSession::BCPReadFmt.  
  
## <a name="arguments"></a>Аргументы  
 *pwszFormatFile*[in]  
 Путь и имя файла, содержащего значения формата для файла данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно.  
  
 E_FAIL  
 Произошла ошибка, связанная с поставщиком. Подробные сведения можно получить с помощью интерфейса [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_OUTOFMEMORY  
 Ошибка, связанная с нехваткой памяти.  
  
 E_UNEXPECTED  
 Непредвиденный вызов метода. Например, перед вызовом этого метода не был вызван метод [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
## <a name="see-also"></a>См. также:  
 [IBCPSession (OLE DB)](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [Выполнение операций массового копирования](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
