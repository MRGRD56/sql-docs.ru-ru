---
title: Перенос баз данных в SQL Server на Linux
description: В это статье описываются различные способы переноса баз данных и данных в SQL Server на Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.openlocfilehash: d267bc2861d94e46cdb10fa825d204c8110a9273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345579"
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>Перенос баз данных и структурированных данных в SQL Server на Linux 

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Вы можете перенести базы данных и данные на сервер SQL Server, работающий в Linux. Выбор способа переноса зависит от исходных данных и ваших условий. В следующих разделах приводятся рекомендации по различным сценариям переноса.

## <a name="migrate-from-sql-server-on-windows"></a>Перенос из SQL Server в Windows
Если нужно перенести базы данных с сервера SQL Server в Windows на сервер SQL Server на Linux, рекомендуется использовать резервное копирование и восстановление SQL Server.

1. Создайте резервную копию базы данных на компьютере Windows.
2. Перенесите файл резервной копии на конечный компьютер SQL Server на Linux.
3. Восстановите резервную копию на компьютере Linux. 

Руководство по переносу базы данных с помощью резервного копирования и восстановления см. в следующей статье:

- [Восстановление базы данных SQL Server из Windows в Linux](sql-server-linux-migrate-restore-database.md)

Кроме того, можно экспортировать базу данных в файл BACPAC (сжатый файл, содержащий схему базы данных и данные). Если у вас есть файл BACPAC, его можно передать на компьютер Linux, а затем импортировать в SQL Server. Дополнительные сведения см. в следующих разделах:

- [Экспорт и импорт базы данных с помощью SSMS или SqlPackage.exe](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>Перенос с других серверов баз данных
В SQL Server на Linux можно переносить базы данных из других систем баз данных. К ним относятся Microsoft Access, DB2, MySQL, Oracle и Sybase. В этом случае используйте Помощник по миграции SQL Server (SSMA) для автоматизации переноса. Дополнительные сведения см. в статье [Перенос баз данных в SQL Server на Linux с помощью SSMA](sql-server-linux-migrate-ssma.md).  

## <a name="migrate-structured-data"></a>Перенос структурированных данных
Существуют также способы импорта необработанных данных. У вас могут быть файлы со структурированными данными, экспортированные из других баз данных или источников данных. В этом случае вы можете выполнить массовую вставку данных с помощью программы bcp. Кроме того, вы можете запустить службы SQL Server Integration Services в Windows, чтобы импортировать данные в базу данных SQL Server на Linux. Службы SQL Server Integration Services позволяют выполнять более сложные преобразования данных во время импорта. 

Дополнительные сведения об этих способах см. в следующих статьях:

- [Массовое копирование данных с помощью bcp](sql-server-linux-migrate-bcp.md)
- [Извлечение, преобразование и загрузка данных в SQL Server на Linux с помощью служб SSIS](sql-server-linux-migrate-ssis.md) 
