---
title: Components
description: Сведения о компонентах SQL Server Native Client, таких как sqlncli11.dll, sqlnclir11. RLL, sqlncli. h и sqlncli11. lib.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- data access [SQL Server Native Client], components
- components [SQL Server Native Client]
- SQLNCLI, about SQL Server Native Client
ms.assetid: 65f932d5-daa1-4eff-b6df-ee633fcf2a7c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6908497c7d296c5138b95c0f41d560123b75d943
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462085"
---
# <a name="components-of-sql-server-native-client"></a>Компоненты собственного клиента SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client содержит следующие компоненты.  
  
|Компонент|Описание|  
|---------------|-----------------|  
|sqlncli11.dll|Файл динамически подключаемой библиотеки (DLL), включающий все функциональные возможности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. В его состав входят поставщик OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client и драйвер ODBC [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.|  
|sqlnclir11.rll|Соответствующий файл ресурса для библиотеки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.|   
|sqlncli.h|Файл заголовка собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], который содержит все определения, необходимые для использования собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Этот файл заголовка заменяет оба файла заголовков — odbcss.h и the sqloledb.h.<br /><br /> Примечание. Вы не можете ссылаться на SQLNCLI. h и ODBC. h в одной программе, но вы можете ссылаться на SQLNCLI. h и SQLOLEDB. h в той же программе, если только SQLOLEDB. h определен первым.|  
|sqlncli11.lib|Файл библиотеки, необходимый для непосредственного вызова функций программы **bcp** , которые являются частью [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвера ODBC для собственного клиента.<br /><br /> Примечание. Если вы ссылаетесь на файл sqlncli11. lib в программном коде, необходимо убедиться, что файл sqlncli11.dll находится в системном пути, а также в системном пути пользователей, которые используют ваше приложение.|  
  
## <a name="see-also"></a>См. также:  
 [Построение приложений с использованием SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
