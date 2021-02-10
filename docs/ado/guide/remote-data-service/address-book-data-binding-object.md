---
description: Объект привязки данных адресной книги
title: Адресная книга Data-Binding объект | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], data-binding object
- address book application scenario [ADO], data-binding object
ms.assetid: 080c1925-d453-4b89-92ac-c93591490518
author: rothja
ms.author: jroth
ms.openlocfilehash: 91489445aef2034ed273a6d13cc5d6ae02b7f519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036594"
---
# <a name="address-book-data-binding-object"></a>Объект привязки данных адресной книги
Приложение адресной книги использует [RDS. Объект элемента управления](../../reference/rds-api/datacontrol-object-rds.md) данными для привязки данных из базы данных SQL Server к визуальному объекту (в нашем примере это таблица DHTML) на HTML-странице клиента приложения. Логика программы VBScript, управляемой событиями, использует [RDS. Элемент управления](../../reference/rds-api/datacontrol-object-rds.md) для:  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
-   Выполните запрос к базе данных, отправьте обновления в базу данных и обновите сетку данных.  
  
-   Разрешить пользователям переходить к первой, следующей, предыдущей или последней записи в сетке данных.  
  
 Следующий код определяет **RDS. Компонент элемента управления** :  
  
```vb
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=DC1 Width=1 Height=1>  
   <PARAM NAME="SERVER" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=sqloledb;  
Initial Catalog=AddrBookDb;Integrated Security=SSPI;">  
</OBJECT>  
```  
  
 Тег объекта определяет **RDS. Компонент "элемент управления** " в программе. Тег содержит два типа параметров:  
  
-   , Связанные с тегом универсального объекта.  
  
-   Те, которые относятся к **RDS. Объект элемента управления** .  
  
## <a name="generic-object-tag-parameters"></a>Параметры тега универсального объекта  
 В следующей таблице описаны параметры, связанные с тегом объекта.  
  
|Параметр|Описание|  
|---------------|-----------------|  
|***ClassID** _|Уникальное, 128-разрядное число, определяющее тип внедренного объекта в системе. Этот идентификатор сохраняется в системном реестре локального компьютера. (Идентификаторы класса _ *RDS. Объект "элемент управления*" см [. в разделе RDS. Объект элемента управления](../../reference/rds-api/datacontrol-object-rds.md).)|  
|***Идентификатор***|Определяет идентификатор на уровне документа для внедренного объекта, который используется для его идентификации в коде.|  
  
## <a name="rdsdatacontrol-tag-parameters"></a>Клиент. Параметры тега элемента управления  
 В следующей таблице описаны параметры, относящиеся к **RDS. Объект элемента управления** . (Полный список **RDS. Параметры объекта элемента управления** данные и когда их следует реализовать, см [. в разделе RDS. Объект элемента управления](../../reference/rds-api/datacontrol-object-rds.md).)  
  
|Параметр|Описание|  
|---------------|-----------------|  
|[СЕРВЕРОМ](../../reference/rds-api/server-property-rds.md)|Если используется HTTP, то значением является имя компьютера сервера, которому предшествует `https://` .|  
|[CONNECT](../../reference/rds-api/connect-property-rds.md)|Предоставляет необходимые сведения о подключении для **RDS. Элемент управления** для подключения к SQL Server.|  
|[SQL](../../reference/rds-api/sql-property.md)|Задает или возвращает строку запроса, используемую для получения [набора записей](../../reference/ado-api/recordset-object-ado.md).|  
  
## <a name="see-also"></a>См. также:  
 [Кнопки команд адресной книги](./address-book-command-buttons.md)