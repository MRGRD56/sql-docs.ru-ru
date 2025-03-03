---
title: Основные проблемы безопасности SQLXML
description: Изучите основные рекомендации по безопасности при использовании SQLXML для доступа к данным.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- security [SQLXML], about security
ms.assetid: 330cd2ff-d5d5-4c8e-8f93-0869c977be94
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9446926fd766c761dd4c473fb4eb0d935c04984b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491616"
---
# <a name="core-sqlxml-security-considerations"></a>Основные проблемы безопасности SQLXML
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Далее приведены рекомендации по безопасности при использовании SQLXML для доступа к данным.  
  
-   Поставщик SQLXMLOLEDB предоставляет свойство **стреамфлагс** , которое позволяет устанавливать флаги, указывающие, какие функции SQLXML должны быть включены или отключены для каждого конкретного экземпляра. При помощи этого свойства можно настраивать использование SQLXML, а также гарантировать, что работать будут только требуемые компоненты. Дополнительные сведения см. в разделе [SQLXMLOLEDB Provider &#40;SQLXML 4,0&#41;](../data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md).  
  
-   Когда возникают и возвращаются ошибки SQLXML, они могут содержать такие сведения о схеме базы данных, как имена таблиц, имена столбцов или сведения о типе. При обработке этих ошибок следует соблюдать осторожность с тем, чтобы сведения об установке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не стали доступными пользователям, когда в этом нет необходимости.  
  
-   SQLXML (при его использовании для выполнения запроса или отправки обновлений на [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) не устанавливает ограничений на объем данных, обмен которыми производится, и не выполняет каких-либо проверок размера полезных данных SQLXML перед их обработкой. При разработке приложения с использованием SQLXML следует самостоятельно позаботиться о наличии достаточного объема памяти в системе для обработки этих данных. Например, при запросе данных с сервера следует проверить, что на клиенте достаточно свободной памяти, чтобы принять их. Точно так же при загрузке данных на сервер необходимо проверить наличие достаточного объема свободной памяти на сервере для их обработки, а также достаточного объема свободного дискового пространства на сервере для хранения данных.  
  
-   SQLXML динамически формирует запросы [!INCLUDE[tsql](../../../includes/tsql-md.md)] и команды обновления и отправляет их на [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для выполнения. SQLXML выполняет запросы и обновляет сервер только таким образом. Результаты будут получены либо в виде потока (XML-данных), либо как набор строк.  
  
-   SQLXML (при получении результатов запроса) не предпринимает каких-либо действий, исходя из содержимого получаемых данных. Никакой дополнительной обработки на основе типа или содержимого данных не производится. Данные никогда не рассматриваются как код, с помощью которого нужно выполнять действия.  
  
-   При выполнении шаблонов XML SQLXML переводит запросы XPath и DBObject, содержащиеся в отправленном шаблоне, в команды [!INCLUDE[tsql](../../../includes/tsql-md.md)], которые затем выполняются на [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Только эти команды всегда затрагивают существующие данные. Команды, сформированные SQLXML, никогда не изменяют структуру базы данных. Пользователь должен явным образом дать команду на изменение структуры базы данных. Например, включив их в блок **SQL: Query** шаблона.  
  
-   При выполнении запросов DBObject и инструкций XPath над файлами сопоставлений SQLXML никак не меняет данные в базе данных.  
  
-   SQLXML может внести изменение в форматирование определенных данных, исходя из различий между моделями данных XML и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Например, различаются форматы задания времени. SQLXML попытается разрешить эти различия. В результате некоторая часть данных о точности может быть потеряна.  
  
-   SQLXML не ограничивает количество времени, которое требуется для обработки данных. Обработка будет выполняться до возникновения ошибки либо завершения обработки.  
  
-   SQLXML не выполняет запись в файловую систему. Если пользователям требуется сохранить данные, получаемые из базы данных, им придется выполнить сохранение в коде.  
  
-   SQLXML позволяет пользователям выполнять любой SQL-запрос к базе данных. Эту функциональность не следует делать доступной для небезопасного или неуправляемого источника, поскольку по сути это является открытием базы данных для любых пользователей.  
  
-   При выполнении диаграмм обновления SQLXML преобразует блоки **атрибута updg: Sync** в команды DELETE, Update и INSERT для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] экземпляра. Только эти команды всегда затрагивают существующие данные. Команды, сформированные SQLXML, никогда не изменяют базу данных. Пользователь должен явным образом дать команду на изменение структуры базы данных. Например, включив их в блок **SQL: Query** шаблона.  
  
-   При выполнении дельты SQLXML переводит дельту в команды DELETE, UPDATE и INSERT, выполняемые на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Только эти команды всегда затрагивают существующие данные. Команды, сформированные SQLXML, никогда не изменяют базу данных. Пользователь должен явным образом дать команду на изменение структуры базы данных. Например, включив их в блок **SQL: Query** шаблона.  
  
## <a name="see-also"></a>См. также  
 [Проблемы безопасности SQLXML 4.0](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
  
