---
description: Свойство SQL
title: Свойство SQL | Документация Майкрософт
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SQL property [RDS]
ms.assetid: e0dabf23-a159-4fe5-a962-3df544a21f5c
author: rothja
ms.author: jroth
ms.openlocfilehash: c293915a847a150719754c1df0f9389be3efb863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052915"
---
# <a name="sql-property"></a>Свойство SQL
Указывает строку запроса, используемую для получения [набора записей](../ado-api/recordset-object-ado.md).  
  
 Свойство **SQL** можно задать во время разработки в [RDS. Теги объекта элемента управления](./datacontrol-object-rds.md) DataObject или во время выполнения в коде скрипта.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Design time: <PARAM NAME="SQL" VALUE="QueryString">  
Run time: DataControl.SQL = "QueryString"  
```  
  
#### <a name="parameters"></a>Параметры  
 *Строку*  
 **Строковое** значение, содержащее допустимый запрос данных SQL.  
  
 *DataControl*  
 Объектная переменная, представляющая **RDS. Объект элемента управления** .  
  
## <a name="remarks"></a>Remarks  
 Как правило, это инструкция SQL (с использованием диалекта сервера базы данных), например `"Select * from NewTitles"` . Чтобы гарантировать точное сопоставление и обновление записей, обновляемый запрос должен содержать поле, отличное от длинного бинарного поля или вычисленного поля.  
  
 Свойство **SQL** является необязательным, если пользовательский бизнес-объект на стороне сервера получает данные для клиента.  
  
## <a name="applies-to"></a>Применение  
 [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойства SQL (VBScript)](./sql-property-example-vbscript.md)   
 [Свойство Connect (RDS)](./connect-property-rds.md)   
 [Метод query (RDS)](./query-method-rds.md)   
 [Метод Refresh (RDS)](./refresh-method-rds.md)   
 [Метод SubmitChanges (служба удаленных рабочих столов)](./submitchanges-method-rds.md)