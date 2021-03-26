---
title: Создание приложения драйвера OLE DB для SQL Server
description: Узнайте, как создать приложение OLE DB Driver for SQL Server и поиска других ресурсов.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fdf209a8bdb34dc5f22a60ee190a118d8a608995
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793081"
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>Создание приложения драйвера OLE DB для SQL Server

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Чтобы создать приложение OLE DB Driver for SQL Server, необходимо выполнить такие шаги:

1. установление соединения с источником данных;
2. выполнение команды;
3. обработку результатов.

> [!NOTE]
> По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранение учетных данных, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccng/cng-portal).

## <a name="in-this-section"></a>Содержание раздела

- [Установка подключения к источнику данных](establishing-a-connection-to-a-data-source.md)
- [Выполнение команды](executing-a-command.md)
- [Обработка результатов](processing-results.md)
- [О свойствах OLE DB](about-ole-db-properties.md)
- [Использование предложения OUTPUT с OLE DB в драйвере OLE DB для SQL Server](using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)

## <a name="see-also"></a>См. также раздел

[Программирование драйвера OLE DB для SQL Server](../ole-db/oledb-driver-for-sql-server-programming.md)
