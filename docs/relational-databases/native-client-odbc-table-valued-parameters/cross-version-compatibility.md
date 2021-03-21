---
description: Совместимость версий
title: Совместимость между версиями | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), cross-version compatibility
ms.assetid: 5f14850b-b85c-41e2-8116-6f5b3f5e0856
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46f9656038ca1fe2e6baf2b1f5b9fb7142a0c783
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752464"
---
# <a name="cross-version-compatibility"></a>Совместимость версий
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Конфликты между версиями могут происходить, если экземпляры клиента или сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], предшествующие [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], участвуют в обработке возвращающих табличное значение параметров.  
  
 В общем случае функциональные возможности возвращающих табличное значение параметров доступны только клиентам [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (с помощью собственного клиента SQL Server версии 10.0) или более поздних версий, соединенных с серверами [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (или более поздней версии). Новые столбцы в результирующих наборах функций каталога будут представлены только при подключении к [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] серверу (или более поздней версии).  
  
 Если клиентское приложение, скомпилированное с помощью собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предыдущей версии, выполняет инструкции, которые ожидают возвращающие табличное значение параметры, то сервер определяет это условие как ошибку преобразования данных, и ODBC возвращает код SQLSTATE 07006 и сообщение «Нарушение атрибута ограниченного типа данных».  
  
 Если клиентское приложение, скомпилированное с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственного клиента 10,0 или более поздней версии, пытается использовать возвращающие табличное значение параметры при соединении с экземпляром сервера, выпущенным ранее [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент обнаружит это, а вызовы SQLBindCol, SQLBindParameter, склсетдескфиелдс и SQLSetDescRec будут завершаться ошибкой с кодом SQLSTATE 07006, а сообщение "нарушение атрибута ограниченного типа данных (версия SQL Server для этого соединения не поддерживает  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
