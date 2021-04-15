---
title: Нехватка памяти с большими коллекциями XML-схем | Документация Майкрософт
description: Сведения о решениях для ситуаций нехватки памяти при загрузке или удалении большой коллекции схем XML.
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- out-of-memory conditions
- XML schema collections [SQL Server], large
ms.assetid: 29b9d839-aaaf-48fb-be17-840c751f36f1
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 3e9a422c35dc2e535472af0aa29e465167af556d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490782"
---
# <a name="large-xml-schema-collections-and-out-of-memory-conditions"></a>Большие коллекции схем XML и условия исчерпания памяти
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В ходе вызова встроенной функции XML_SCHEMA_NAMESPACE () на большой коллекции схем XML или при попытке удалить большую коллекцию схем XML может возникнуть условие исчерпания памяти. Далее приводятся решения, которыми можно воспользоваться в данных ситуациях.  
  
-   При небольшой системной загрузке используйте команду DROP_XML_SCHEMA_COLLECTION. При неудачном выполнении команды переведите базу данных в однопользовательский режим с помощью инструкции ALTER DATABASE и попытайтесь выполнить DROP XML SCHEMA COLLECTION снова. Если коллекция XML-схем присутствует в базе данных **master**, **model** или **tempdb**, то для перехода в однопользовательский режим потребуется перезапуск сервера.  
  
-   При вызове XML_SCHEMA_NAMESPACE можно попытаться получить одиночное пространство имен XML-схемы, попробовать сделать вызов позже, когда снизится нагрузка в системе, или же попытаться произвести вызов в однопользовательском режиме.  
  
## <a name="see-also"></a>См. также:  
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
