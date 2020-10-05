---
description: Шаг 6. Изменения отправлены на сервер (учебник по RDS)
title: Шаг 6. изменения отправляются на сервер (учебник по RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
author: rothja
ms.author: jroth
ms.openlocfilehash: 9929177baaf1efd486cb9f628034158b370badc7
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91722865"
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>Шаг 6. Изменения отправлены на сервер (учебник по RDS)
Если объект **набора записей** редактируется, любые изменения (т. е. добавленные, измененные или удаленные строки) могут быть отправлены обратно на сервер.  
  
> [!NOTE]
>  Поведение RDS по умолчанию можно вызвать неявно с помощью объектов ADO и поставщика Microsoft OLE DB удаленного взаимодействия. Запросы могут возвращать **наборы записей**, а измененные **наборы записей**могут обновлять источник данных. В этом руководстве не выполняется вызов RDS с объектами ADO, но вот как это будет выглядеть, если:  
  
```vb
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=https://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **Часть A** Предположим, что в этом случае вы использовали только [RDS. Элемент управления](../../reference/rds-api/datacontrol-object-rds.md) данными и объект **набора записей** теперь связаны с **RDS. Элемент управления**. Метод [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) обновляет источник данных любыми изменениями в объекте **набора записей** , если свойства [сервера](../../reference/rds-api/server-property-rds.md) и [соединения](../../reference/rds-api/connect-property-rds.md) по-прежнему заданы.  
  
```vb
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "https://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **Часть B** Кроме того, можно обновить сервер с помощью объекта [RDSServer.](../../reference/rds-api/datafactory-object-rdsserver.md) DataObject, указав соединение и объект **набора записей** .  
  
```vb
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **Это последняя часть руководства.**  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>См. также  
 [Поставщик службы удаленного взаимодействия Microsoft OLE DB (поставщик служб ADO)](../appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [Руководство по RDS](./rds-tutorial.md)   
 [Учебник по RDS (VBScript)](./rds-tutorial-vbscript.md)