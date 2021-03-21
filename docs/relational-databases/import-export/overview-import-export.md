---
title: Импорт и экспорт данных в SQL Server и базе данных SQL Azure
description: Вы можете использовать Transact-SQL, программы командной строки и мастеры для импорта и экспорта данных в SQL Server и Базу данных SQL Azure, чтобы работать с различными форматами данных.
ms.date: 10/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 85b4c92a1158bf1b0a8870137ef254dc472dec54
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737654"
---
# <a name="import-and-export-data-from-sql-server-and-azure-sql-database"></a>Импорт и экспорт данных в SQL Server и базе данных SQL Azure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Для импорта и экспорта данных в SQL Server и базе данных SQL Azure доступны разнообразные методы. Сюда входят инструкции Transact-SQL, программы командной строки и мастеры.

Кроме того, вы можете импортировать и экспортировать данные в разных форматах. Эти форматы включают неструктурированные файлы, файлы Excel, основные типы реляционных баз данных и форматы различных облачных служб.

## <a name="methods-for-importing-and-exporting-data"></a>Методы импорта и экспорта данных

### <a name="use-transact-sql-statements"></a>Использование инструкций Transact-SQL
Вы можете импортировать данные с помощью команд `BULK INSERT` или `OPENROWSET(BULK...)`. Обычно эти команды выполняются в SQL Server Management Studio (SSMS). Дополнительные сведения см. в разделе [Массовый импорт данных с помощью инструкции BULK INSERT или OPENROWSET(BULK...)](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="use-bcp-from-the-command-prompt"></a>Используйте BCP в командной строке
Вы можете импортировать и экспортировать данные с помощью служебной программы командной строки BCP. Дополнительные сведения см. в разделе [Массовый импорт и экспорт данных с использованием программы BCP](import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md).

### <a name="use-the-import-flat-file-wizard"></a>Использование мастера импорта неструктурированных файлов
Если вам не нужны все параметры конфигурации, доступные в окне мастера экспорта и импорта (а также другие средства), вы можете импортировать текстовый файл в SQL Server с помощью **мастера импорта неструктурированных файлов** в SQL Server Management Studio (SSMS). Дополнительные сведения см. в следующих статьях:
- [Мастер импорта неструктурированных файлов в SQL](import-flat-file-wizard.md)
- [Новые возможности SQL Server Management Studio 17.3](https://blogs.technet.microsoft.com/dataplatforminsider/2017/10/10/whats-new-in-sql-server-management-studio-17-3/)
- [Общие сведения о новом мастере импорта неструктурированных файлов в SSMS 17.3](https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173)

### <a name="use-the-sql-server-import-and-export-wizard"></a>Использование мастера импорта и экспорта SQL Server
Мастер импорта и экспорта SQL Server позволяет вам экспортировать данные из самых разных источников и импортировать их во множество различных назначений. Чтобы использовать мастер, необходимо установить SQL Server Integration Services (SSIS) или SQL Server Data Tools (SSDT). Дополнительные сведения см. в разделе [Импорт и экспорт данных с помощью мастера импорта и экспорта SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

### <a name="design-your-own-import-or-export"></a>Разработка собственного импорта и экспорта
Если вы хотите настроить свой собственный вариант импорта данных, воспользуйтесь следующими функциями или службами.
-   SQL Server Integration Services Дополнительные сведения см. в разделе [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md).
-   Фабрика данных Azure Дополнительные сведения см. в разделе [Введение в фабрику данных Azure](/azure/data-factory/data-factory-introduction).

## <a name="data-formats-for-import-and-export"></a>Форматы данных для импорта и экспорта

### <a name="supported-formats"></a>Поддерживаемые форматы

Вы можете выполнять импорт и экспорт данных в виде неструктурированных файлов, а также во множестве других форматов, в виде реляционных баз данных и облачных служб. Дополнительные сведения об использовании этих вариантов в конкретных инструментах см. в следующих разделах.
-   Сведения о мастере импорта и экспорта SQL Server см. в разделе [Подключение к источникам данных с помощью мастера импорта и экспорта SQL Server](../../integration-services/import-export-data/connect-to-data-sources-with-the-sql-server-import-and-export-wizard.md).
-   Сведения о SQL Server Integration Services см. в разделе [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md).
-   Сведения о фабрике данных Azure см. в разделе [Соединители фабрики данных Azure](/azure/data-factory/data-factory-amazon-redshift-connector).

### <a name="commonly-used-data-formats"></a>Часто используемые форматы данных

Для часто используемых форматов данных есть особые возможности и примеры. Дополнительные сведения об этих форматах данных, см. в следующих разделах:
-   сведения об Excel см. в разделе [Импорт из Excel](import-data-from-excel-to-sql.md);
-   сведения о JSON см. в разделе [Импорт документов JSON](../json/import-json-documents-into-sql-server.md);
-   сведения об XML см. в разделе [Импорт и экспорт XML-документов](examples-of-bulk-import-and-export-of-xml-documents-sql-server.md);
-   сведения о хранилище BLOB-объектов Azure см. в разделе [Импорт и экспорт данных из хранилища BLOB-объектов Azure](examples-of-bulk-access-to-data-in-azure-blob-storage.md).

## <a name="next-steps"></a>Дальнейшие действия
Если вы не знаете, с чего начать импорт или экспорт, попробуйте запустить мастер импорта и экспорта SQL Server. Краткие сведения см. в разделе [Приступая к работе с простым примером мастера импорта и экспорта](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).