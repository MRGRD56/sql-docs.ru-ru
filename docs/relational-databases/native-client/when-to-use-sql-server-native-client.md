---
title: Назначение
description: Решите, следует ли использовать SQL Server Native Client, который является одной из нескольких технологий, которые можно использовать для доступа к данным в SQL Server базе данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- SQLNCLI, about SQL Server Native Client
- data access [SQL Server Native Client], about SQL Server Native Client
ms.assetid: 08f18b36-209d-4cf7-9623-ebc61859a91d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9a42f83e52014ee5e2de83d853bca52b87fe3f86
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481045"
---
# <a name="when-to-use-sql-server-native-client"></a>Когда использовать собственный клиент SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — одна из технологий для доступа к данным в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Обсуждение других технологий доступа к данным см. в разделе [Схема технологий доступа к данным](../../connect/connect-history.md).  
  
 В принятии решения о необходимости использования в качестве технологии доступа к данным собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо принимать во внимание ряд факторов.  
  
 Если используется язык программирования с управляемым кодом, например Microsoft Visual C# или Visual Basic, и необходимо обращаться к новым функциям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то для новых приложений следует пользоваться поставщиком данных .NET Framework для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который является частью платформы .NET Framework.  
  
 Если разрабатывается приложение на основе COM и необходим доступ к новым функциям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], следует использовать собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если доступ к новым возможностям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не требуется, то можно продолжать использовать компоненты WDAC.  
  
 Для существующих приложений OLE DB и ODBC самый важный вопрос — необходим ли доступ к новым функциям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если имеется отлаженное приложение, не требующее новых возможностей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то можно продолжать использование компонентов WDAC. Но если вам нужно получить доступ к этим новым функциям, таким как [тип данных XML](../../t-sql/xml/xml-transact-sql.md), следует использовать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент.  
  
 Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и MDAC поддерживают уровень изоляции транзакций read committed при использовании управления версиями строк, однако изоляцию транзакций моментальных снимков поддерживает только собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. С точки зрения программирования уровень изоляции транзакции READ COMMITTED с управлением версиями строк — то же самое, что и транзакция READ COMMITTED.  
  
 Сведения о различиях между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственным клиентом и компонентами MDAC см. в разделе [Обновление приложения для SQL Server Native Client из MDAC](../../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client программирование](../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [Разделы руководства по ODBC](../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [Инструкции по OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
