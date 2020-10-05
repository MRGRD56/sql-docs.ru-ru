---
description: Разрешение запуска библиотеки DLL в DCOM
title: Включение запуска DLL на DCOM | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DLL on DCOM in RDS [ADO]
- DCOM in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: 5f1c2205-191c-4fb4-9bd9-84c878ea46ed
author: rothja
ms.author: jroth
ms.openlocfilehash: 220f4a8abfe37a12a7f0699b9aec8a634691cabe
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91723225"
---
# <a name="enabling-a-dll-to-run-on-dcom"></a>Разрешение запуска библиотеки DLL в DCOM
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Ниже показано, как включить для бизнес-объекта. dll использование DCOM и Microsoft службы IIS (HTTP) через службы компонентов.  
  
1.  Создайте новый пустой пакет в оснастке MMC "службы компонентов".  
  
     Используйте оснастку MMC "службы компонентов" для создания пакета и добавления библиотеки DLL в этот пакет. Это делает библиотеку DLL доступной через DCOM, но она удаляет специальные возможности через IIS. (При возврате реестра для DLL-файла **процедура INPROC** теперь пуста; Установка атрибута активации, описанного далее в этом разделе, добавляет значение в ключ **INPROC** .)  
  
2.  Установите бизнес-объект в пакет.  
  
     -или-  
  
     Импортируйте объект [RDSServer.](../../reference/rds-api/datafactory-object-rdsserver.md) DataObject в пакет.  
  
3.  Установите атрибут активации для пакета **в в процессе создателя** (приложение библиотеки).  
  
     Чтобы сделать библиотеку DLL доступной через DCOM и службы IIS на том же компьютере, необходимо задать атрибут активации компонента в оснастке MMC "службы компонентов". После присвоения атрибуту значения **в процессе создателя**вы заметите, что был добавлен ключ сервера **INPROC** в реестр, указывающий на суррогатную библиотеку служб компонентов.  
  
 Дополнительные сведения о службах компонентов (или Microsoft Transaction Service, если используется Windows NT) и о том, как выполнить эти действия, см. на веб-сайте Microsoft Transaction Server.