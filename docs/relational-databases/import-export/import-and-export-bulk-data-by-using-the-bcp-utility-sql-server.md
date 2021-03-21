---
title: Массовый импорт и экспорт данных с помощью программы bcp
description: Использование программы bcp для экспорта данных из любого места в базу данных SQL Server, где работает SELECT. Массовый экспорт данных из таблицы или из запроса и массовый импорт из файла.
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], bcp utility
- bulk importing [SQL Server], bcp utility
- bcp utility [SQL Server], about bcp utility
ms.assetid: 73e949de-67a3-4c84-9735-7da1ad4ba34a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 09/28/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: f2186cea32347c0e5418c2360371a2985f2ac9d6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749664"
---
# <a name="import-and-export-bulk-data-using-bcp-sql-server"></a>Массовый импорт и экспорт данных с помощью программы bcp (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этом разделе представлен обзор использования [программы bcp](../../tools/bcp-utility.md) для экспорта данных из любого местоположения в базу данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , в которой может применяться инструкция SELECT, включая секционированные представления.  
  
 Программа bcp (bcp.exe) представляет собой инструмент командной строки, использующий API-интерфейс программы массового копирования (BCP). Программа bcp выполняет следующие задачи:  
  
-   массовый экспорт данных из таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в файл данных;  
  
-   массовый экспорт данных из запроса;  
  
-   массовый импорт данных из файла данных в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   создание файлов форматирования.  
  
 Служебная программа bcp вызывается командой **bcp** . Применение команды **bcp** для массового импорта требует понимания схемы таблицы и типов данных ее столбцов (если не используется заранее созданный файл форматирования).  
  
 Программа bcp может экспортировать данные из таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в файл данных для использования другими программами. Программа также может импортировать данные в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из другой программы, обычно другой системы управления базой данных (СУБД). Вначале выполняется экспорт данных из исходной программы в файл данных, а затем отдельной операцией данные копируются из файла данных в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Команда **bcp** предоставляет параметры для указания типа данных файла данных и других сведений. Если такие ключи не заданы, программа выводит приглашение для ввода этих сведений, например для типа полей данных в файле данных. Затем команда запрашивает, нужно ли создать файл форматирования, содержащий данные ответы. Чтобы обеспечить гибкость для будущих операций массового импорта и экспорта, часто используется файл форматирования. В последующих командах **bcp** вы можете указать файл форматирования для эквивалентных файлов данных. Дополнительные сведения см. в разделе [Указание форматов данных для совместимости с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
>**Примечание.** Служебная программа bcp написана при использовании массового копирования ODBC.
  
 Описание синтаксиса команды **bcp** см. в разделе [Служебная программа bcp](../../tools/bcp-utility.md).  
  
## <a name="examples"></a>Примеры  

|Следующие разделы содержат примеры использования программы bcp: |
|---|
|[Программа bcp](../../tools/bcp-utility.md)<br /><br />Форматы данных для массового экспорта или импорта (SQL Server)<br />&emsp;&#9679;&emsp;[Использование собственного формата для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование символьного формата для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование собственного формата Юникода для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование символьного формата Юникода для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />Файлы форматирования для импорта или экспорта данных (SQL Server)<br />&emsp;&#9679;&emsp;[Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование файла форматирования для пропуска столбца таблицы (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Примеры массового импорта и экспорта XML-документов (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="more-examples-and-information"></a>Примеры и дополнительные сведения

- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)

- [Предложение SELECT (Transact-SQL)](../../t-sql/queries/select-clause-transact-sql.md)

- [Программа bcp](../../tools/bcp-utility.md)

- [Подготовка массового импорта данных (SQL Server)](../../relational-databases/import-export/prepare-to-bulk-import-data-sql-server.md)

- [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)

- [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)

- [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)

- [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)