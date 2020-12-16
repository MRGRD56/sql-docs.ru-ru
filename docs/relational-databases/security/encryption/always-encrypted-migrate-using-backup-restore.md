---
description: Резервное копирование и восстановление баз данных с помощью Always Encrypted
title: Резервное копирование и восстановление баз данных с помощью Always Encrypted | Документация Майкрософт
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d767d205b13585b337f6886ba7f5b003ceba56dc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477605"
---
# <a name="backup-and-restore-databases-using-always-encrypted"></a>Резервное копирование и восстановление баз данных с помощью Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

В этой статье описывается резервное копирование и восстановление базы данных, содержащей столбцы, защищенные с помощью [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

При создании резервной копии базы данных результирующий файл содержит зашифрованные данные, хранящиеся в зашифрованных столбцах, и все метаданные для ключей Always Encrypted.

При восстановлении базы данных восстанавливаются все зашифрованные данные и все метаданные для ключей Always Encrypted. 

Если база данных восстановлена на другом сервере или с другим именем, не нужно делать ничего особенного, чтобы приложение могло запрашивать зашифрованные данные в целевой базе данных, так как ключи в обеих базах данных одинаковы.

Дополнительные сведения о резервном копировании и восстановлении баз данных см. в следующих статьях:
- [Backup Overview (SQL Server)](../../backup-restore/backup-overview-sql-server.md)
- [Восстановление базы данных в Управляемый экземпляр](/azure/sql-database/sql-database-managed-instance-get-started-restore)

## <a name="next-steps"></a>Next Steps
- [Выполнение запросов к столбцам с помощью Always Encrypted с использованием SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Разработка приложений с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>См. также:
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Экспорт и импорт баз данных с помощью Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Перенос данных в столбцы или из них с помощью Always Encrypted с использованием мастера импорта и экспорта SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Массовая загрузка зашифрованных данных в столбцы с помощью Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)