---
title: Системные хранимые процедуры XML | Документация Майкрософт
description: Сведения о системных хранимых процедурах XML, предоставляемых SQL Server, которые используются для записи запросов с помощью OPENXML.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- system stored procedures [SQL Server], XML
- sp_xml_removedocument
- OPENXML statement, system stored procedures
- sp_xml_preparedocument
- XML [SQL Server], system stored procedures
ms.assetid: e60c7f85-6823-4d28-93d6-b053d08cc830
author: rothja
ms.author: jroth
ms.openlocfilehash: f92b5bb8ec27dd9b15caf8bce013dbd5c3a4e66f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486623"
---
# <a name="xml-system-stored-procedures"></a>Системные хранимые процедуры XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  СУБД SQL Server предоставляет следующие системные хранимые процедуры, которые используются вместе с инструкцией OPENXML:  
  
-   [sp_xml_preparedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)  
  
-   [sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)  
  
 Чтобы составлять запросы с помощью OPENXML, нужно сначала создать внутреннее представление XML-документа, вызвав процедуру **sp_xml_preparedocument**. Эта хранимая процедура возвращает дескриптор внутреннего представления XML-документа. Затем этот дескриптор передается инструкции OPENXML. Инструкция OPENXML предоставляет представления наборов строк документа, основанные на выражениях XPaths. Точнее, это одна комбинация для строк и одна или несколько для столбцов.  
  
> [!NOTE]  
>  Дескриптор документа, возвращаемый процедурой **sp_xml_preparedocument** , действителен в течение сеанса.  
  
 Внутреннее представление XML-документа можно удалить из памяти вызовом системной хранимой процедуры **sp_xml_removedocument** .  
  
## <a name="see-also"></a>См. также:  
 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
  
  
