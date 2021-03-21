---
title: Проверка версии драйвера ODBC для SQL Server (Windows) | Документы Майкрософт
description: Сведения о том, как с помощью администратора источников данных ODBC операционной системы Windows проверить версию установленных на компьютере драйверов ODBC.
ms.custom: ''
ms.date: 11/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- driver version number [ODBC]
- ODBC drivers, version number
ms.assetid: 43451080-a562-4231-b1d4-1ba35ca0ea79
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 885984563072d8673aaf687452c35f8f923414a4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748179"
---
# <a name="check-the-odbc-sql-server-driver-version-windows"></a>Проверка версии драйвера ODBC для SQL Server (Windows)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  На компьютере могут быть установлены разнообразные драйверы ODBC, разработанные как [!INCLUDE[msCoName](../../includes/msconame-md.md)] , так и другими организациями. В этом разделе описано, как с помощью **администратора источников данных ODBC** операционной системы Windows проверить версию установленных драйверов ODBC.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-check-the-odbc-sql-server-driver-version-32-bit-odbc"></a>Проверка версии драйвера ODBC SQL Server (32-разрядная версия ODBC)  
  
-   В **администраторе источников данных ODBC** перейдите на вкладку **Драйверы** .  
  
     В столбце [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Версия **отображается запись для Microsoft** .  


> [!NOTE]  
>  Настраивая подключения для проверки подлинности Azure Active Directory для базы данных SQL, установите последнюю версию драйвера, например [версию 17 драйвера ODBC для SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).   

  
## <a name="see-also"></a>См. также:  
 [Открытие администратора источника данных ODBC](../../database-engine/configure-windows/open-the-odbc-data-source-administrator.md)  
  
