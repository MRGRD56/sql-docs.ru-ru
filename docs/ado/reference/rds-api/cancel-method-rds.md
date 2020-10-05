---
description: Метод Cancel (служба удаленных рабочих столов)
title: Метод Cancel (RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Cancel method [RDS]
ms.assetid: 560b5b3d-fba9-4275-8920-9c3e186134f7
author: rothja
ms.author: jroth
ms.openlocfilehash: 24f72c16e1c27d070bcc52fc29c6599cc11c737e
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91722695"
---
# <a name="cancel-method-rds"></a>Метод Cancel (служба удаленных рабочих столов)
Отменяет выполнение ожидающего асинхронного вызова метода.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
RDS.DataControl.Cancel  
```  
  
## <a name="remarks"></a>Remarks  
 При вызове **Cancel**параметр [ReadyState](./readystate-property-rds.md) автоматически устанавливается в значение **адкреадистателоадед**, а [набор записей](../ado-api/recordset-object-ado.md) будет пустым.  
  
## <a name="applies-to"></a>Применение  
 [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также  
 [Пример метода Cancel (VBScript)](./cancel-method-example-vbscript.md)   
 [Метод Cancel (ADO)](../ado-api/cancel-method-ado.md)   
 [Метод CancelBatch (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [Метод CancelUpdate (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [Метод CancelUpdate (RDS)](./cancelupdate-method-rds.md)   
 [Свойство ExecuteOptions (служба удаленных рабочих столов)](./executeoptions-property-rds.md)