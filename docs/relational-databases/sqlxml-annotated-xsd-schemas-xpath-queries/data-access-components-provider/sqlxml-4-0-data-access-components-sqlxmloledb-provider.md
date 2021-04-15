---
title: Компоненты доступа к данным SQLXML 4.0
description: Сведения о компонентах доступа к данным в SQLXML 4,0-SQLXMLOLEDB provider, управляемых классах SQLXML и SQL Server Native Client (SQLNCLI11).
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- data access [SQLXML]
- data providers [SQLXML]
- SQLXML, data access components
- data providers [SQLXML], listed
- providers [SQLXML]
- providers [SQLXML], listed
ms.assetid: 5001e9fd-555c-4332-a57d-4d29a537454a
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec18fec4df37440f55e74080ae9c4eda61280d31
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491880"
---
# <a name="sqlxml-40-data-access-components---sqlxmloledb-provider"></a>Компоненты доступа к данным SQLXML 4.0 — поставщик SQLXMLOLEDB
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 включает в себя три поставщика данных, которые могут вставлять XML-данные и получать XML-данные из базы данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   SQLXMLOLEDB, поставщик  
  
     Предоставляет функциональность SQLXML 4.0 через объекты ADO.  
  
-   управляемые классы SQLXML  
  
     Предоставляет функциональность SQLXML внутри платформы [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. Дополнительные сведения см. в разделе [управляемые классы SQLXML](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md).  
  
-   Собственный клиент SQL Server (SQLNCLI11)  
  
     Отображает функции SQLXML 4.0 с помощью новой технологии доступа к данным, которая расширяет и дополняет текущие версии компонентов доступа к данным MDAC. SQLNCLI11 обеспечивает полную поддержку для функций, представленных в [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Дополнительные сведения см. в статье [Программирование SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md).  
  
## <a name="in-this-section"></a>в этом разделе  
 [Поставщик SQLXMLOLEDB &#40;SQLXML 4,0&#41;]()  
 Описывает поставщик SQLXMLOLEDB и демонстрирует его использование.  
  
