---
description: Регистрация бизнес-объектов в клиенте для использования с DCOM
title: Регистрация бизнес-объектов на клиенте для использования с DCOM | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 75a21910-607f-463a-ae18-a17130dafb7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 51f71b24c1917a05255e2e1ceddd64096971f487
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91723185"
---
# <a name="registering-business-objects-on-the-client-for-use-with-dcom"></a>Регистрация бизнес-объектов в клиенте для использования с DCOM
Пользовательские бизнес-объекты должны гарантировать, что клиент может сопоставлять имя программы (ProgId) с идентификатором (CLSID), который может использоваться через DCOM. По этой причине ProgID DCOM-объекта должен находиться в реестре на стороне клиента и сопоставляться с ИДЕНТИФИКАТОРом класса для бизнес-объекта на стороне сервера. Для других поддерживаемых протоколов (HTTP, HTTPS и внутрипроцессный) это необязательно.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Например, при предоставлении бизнес-объекта на стороне сервера с именем Мибобж с конкретным ИДЕНТИФИКАТОРом класса, например "{00112233-4455-6677-8899-00AABBCCDDEE}", убедитесь, что в реестр на стороне клиента добавлены следующие записи:  
  
```console
[HKEY_CLASSES_ROOT]  
\MyBObj\Clsid\(Default) "{00112233-4455-6677-8899-00AABBCCDDEE}"  
```