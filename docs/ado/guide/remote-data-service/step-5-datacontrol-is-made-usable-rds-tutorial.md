---
description: Шаг 5. DataControl теперь можно использовать (учебник по RDS)
title: Шаг 5. Использование элемента управления "Управление доступом" (руководство по RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 6453043514657b03e72eeea032451f647f6c2eec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036094"
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>Шаг 5. DataControl теперь можно использовать (учебник по RDS)
Возвращаемый объект **набора записей** доступен для использования. Вы можете просматривать, перемещать или изменять его так же, как любой другой **набор записей**. Действия, которые можно выполнять с **набором записей** , зависят от среды. Visual Basic и Visual C++ имеют визуальные элементы управления, которые могут напрямую или косвенно использовать **набор записей** с помощью включения элемента управления данными.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Например, при отображении веб-страницы в Microsoft Internet Explorer может потребоваться отобразить данные объекта **набора записей** в визуальном элементе управления. Визуальные элементы управления на веб-странице не могут напрямую обращаться к объекту **Recordset** . Однако они могут получить доступ к объекту **Recordset** через [RDS. Элемент управления](../../reference/rds-api/datacontrol-object-rds.md). **RDS. Элемент управления** данных может использоваться визуальным элементом управления, если его свойство [саурцерекордсет](../../reference/rds-api/recordset-sourcerecordset-properties-rds.md) имеет значение Object **Recordset** .  
  
 Для объекта визуального элемента управления параметр **DATASRC** должен иметь значение **RDS. Элемент управления** данных и его свойство **DATAFLD** , установленное в поле объекта **набора записей** (столбец).  
  
 В этом руководстве задайте свойство **саурцерекордсет** :  
  
```vb
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>См. также:  
 [Шаг 6. изменения, отправляемые на сервер (учебник по RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [Учебник по RDS (VBScript)](./rds-tutorial-vbscript.md)