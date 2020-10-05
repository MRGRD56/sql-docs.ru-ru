---
description: Свойство SQL
title: Свойство SQL | Документация Майкрософт
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- SQL property [RDS]
ms.assetid: e0dabf23-a159-4fe5-a962-3df544a21f5c
author: rothja
ms.author: jroth
ms.openlocfilehash: 325f739e55bcb2b7d3e7440e9906fad341c01078
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91724200"
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
  
## <a name="remarks"></a>Комментарии  
 Как правило, это инструкция SQL (с использованием диалекта сервера базы данных), например `"Select * from NewTitles"` . Чтобы гарантировать точное сопоставление и обновление записей, обновляемый запрос должен содержать поле, отличное от длинного бинарного поля или вычисленного поля.  
  
 Свойство **SQL** является необязательным, если пользовательский бизнес-объект на стороне сервера получает данные для клиента.  
  
## <a name="applies-to"></a>Применение  
 [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также  
 [Пример свойства SQL (VBScript)](./sql-property-example-vbscript.md)   
 [Свойство Connect (RDS)](./connect-property-rds.md)   
 [Метод query (RDS)](./query-method-rds.md)   
 [Метод Refresh (RDS)](./refresh-method-rds.md)   
 [Метод SubmitChanges (служба удаленных рабочих столов)](./submitchanges-method-rds.md)