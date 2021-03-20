---
title: Учетные данные (ядро СУБД) | Документация Майкрософт
description: Сведения об учетных данных в SQL Server Ознакомьтесь с учетными данными, необходимыми для подключения к ресурсу извне SQL Server.
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- principals [SQL Server], credentials
- schemas [SQL Server], credentials
- permissions [SQL Server], credentials
- groups [SQL Server], credentials
- ALTER ANY CREDENTIAL permission
- security [SQL Server], credentials
- authentication [SQL Server], credentials
- users [SQL Server], credentials
- credentials [SQL Server], about credentials
- credentials [SQL Server]
ms.assetid: c8df6022-e0b4-46b8-9670-3f86938d3177
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 605b9373f21a7a1a0776f154851b03cad2caac1c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753274"
---
# <a name="credentials-database-engine"></a>Учетные данные (компонент Database Engine)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Учетные данные представляют собой запись, которая содержит сведения для проверки подлинности (учетные данные), которые необходимы для подключения к ресурсу вне [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Эти сведения используются [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Большинство учетных данных содержат имя пользователя Windows и пароль.  
  
 Хранимые в учетных данных сведения позволяют пользователю, подключившемуся к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с применением проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , обращаться к ресурсам, размещенным вне данного экземпляра сервера. Когда внешним ресурсом является ОС Windows, проверяется соответствие данных пользователя сведениям, указанным в учетной записи Windows. Одни учетные данные могут соответствовать только одному имени для входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а каждое имя для входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может соответствовать только одним учетным данным.  
  
 Сведения об учетных данных, которые хранятся в базе данных master и могут использоваться для всего экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], см. в разделе [CREATE CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-credential-transact-sql.md). Сведения об учетных данных, которые используются определенной базой данных и переносятся вместе с ней, см. в разделе [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
 Системные учетные данные создаются автоматически и связываются с определенными конечными точками. Имена для системных учетных данных начинаются с двух символов решетки (##).  
  
 Дополнительные сведения об учетных данных см. в представлениях каталога [sys.credentials](../../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md) и [sys.database_scoped_credentials](../../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="related-content"></a>См. также  
 [Создание учетных данных](../../../relational-databases/security/authentication-access/create-a-credential.md)   
 [CREATE CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-credential-transact-sql.md)   
 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md)  
 [Обеспечение безопасности SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
  
