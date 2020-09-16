---
description: Задание или изменение параметров сортировки сервера
title: Установка и изменение параметров сортировки для сервера| Документация Майкрософт
ms.custom: ''
ms.date: 05/10/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- server collations [SQL Server]
- collations [SQL Server], server
ms.assetid: 3242deef-6f5f-4051-a121-36b3b4da851d
author: stevestein
ms.author: sstein
ms.openlocfilehash: 3bd3a3de0bf42300075af11ddafb088dd746f954
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539792"
---
# <a name="set-or-change-the-server-collation"></a>Задание или изменение параметров сортировки сервера

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Параметры сортировки сервера применяются по умолчанию для всех установленных системных баз данных с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а также для новых пользовательских баз данных. Необходимо тщательно выбрать параметры сортировки уровня сервера, так как они влияют на следующее.
 - Правила сортировки и сравнения в `=`, `JOIN`, `ORDER BY` и другие операторы сравнения текстовых данных.
 - Параметры сортировки столбцов `CHAR`, `VARCHAR`, `NCHAR` и `NVARCHAR` системных представлений, системных функций и объектов в базе данных TempDB (например, темпоральных таблиц).
 - Имена переменных, курсоров и меток `GOTO`. Переменные @pi и @PI считаются различными, если параметры сортировки уровня сервера заданы с учетом регистра, и одной и той же переменной, если параметры сортировки уровня сервера заданы без учета регистра.
  
## <a name="setting-the-server-collation-in-sql-server"></a>Настройка параметров сортировки сервера в SQL Server

  Параметры сортировки сервера задаются во время установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Параметры сортировки на уровне сервера по умолчанию основываются на языковом стандарте операционной системы. Например, параметры сортировки по умолчанию для систем, использующих английский язык (EN-US): **SQL_Latin1_General_CP1_CI_AS**. Параметры сортировки только для Юникода не могут быть заданы как параметры сортировки уровня сервера. Дополнительные сведения, включая список языковых стандартов ОС в сопоставлениях параметров сортировки по умолчанию, см. в подразделе "Параметры сортировки уровня сервера" раздела [параметры сортировки и поддержка Юникода](collation-and-unicode-support.md#Server-level-collations).

> [!NOTE]  
> Параметры сортировки уровня сервера для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express LocalDB **SQL_Latin1_General_CP1_CI_AS** и не могут быть изменены во время установки или после нее.  

## <a name="changing-the-server-collation-in-sql-server"></a>Изменение параметров сортировки сервера в SQL Server

 Чтобы изменить параметры сортировки по умолчанию для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (эта операция может оказаться сложной), выполните следующие шаги:  
  
- Проверьте наличие данных и скриптов, необходимых для повторного создания пользовательской базы данных и всех ее объектов.  
  
- Экспортируйте все данные с помощью такого средства, как [bcp Utility](../../tools/bcp-utility.md). Дополнительные сведения см. в разделе [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
- Удалите все пользовательские базы данных.  
  
- Перестройте базу данных master, указав новые параметры сортировки в свойстве SQLCOLLATION команды **setup** . Пример:  
  
    ```  
    Setup /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=InstanceName
    /SQLSYSADMINACCOUNTS=accounts /[ SAPWD= StrongPassword ]
    /SQLCOLLATION=CollationName  
    ```  
  
     Дополнительные сведения см. в разделе [Перестроение системных баз данных](../../relational-databases/databases/rebuild-system-databases.md).  
  
- Создайте все базы данных и все их объекты.  
  
- Импортируйте все данные.  
  
> [!NOTE]  
> Вместо изменения параметров сортировки по умолчанию для всего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]можно указывать параметры сортировки по умолчанию для каждой новой базы данных, создаваемой с помощью условия `COLLATE` выражений `CREATE DATABASE` и `ALTER DATABASE`. Дополнительные сведения см. в разделе [Set or Change the Database Collation](set-or-change-the-database-collation.md).  
  
## <a name="setting-the-server-collation-in-managed-instance"></a>Задание параметров сортировки сервера в управляемом экземпляре
Параметры сортировки на уровне сервера в Управляемом экземпляре SQL Azure можно указать при создании экземпляра и нельзя изменить позднее. Параметры сортировки на уровне сервера можно настроить на [портале Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started#create-a-managed-instance) или с помощью [PowerShell и шаблона Resource Manager](https://docs.microsoft.com/azure/sql-database/scripts/sql-managed-instance-create-powershell-azure-resource-manager-template) во время создания экземпляра. Параметры сортировки по умолчанию — **SQL_Latin1_General_CP1_CI_AS**. Параметры сортировки только для Юникода и новые параметры сортировки UTF-8 не могут быть заданы как параметры сортировки уровня сервера.
При миграции баз данных SQL Server на управляемый экземпляр проверьте параметры сортировки сервера в исходном SQL Server с помощью функции `SERVERPROPERTY(N'Collation')` и создайте управляемый экземпляр, который соответствует параметрам сортировки SQL Server. Миграция базы данных из SQL Server в управляемый экземпляр с несоответствующими параметрами сортировки на уровне сервера может приводить к нескольким непредвиденным ошибкам в запросах. Изменить параметры сортировки уровня сервера у существующего управляемого экземпляра невозможно.

## <a name="see-also"></a>См. также:

 [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)   
 [Установка и изменение параметров сортировки базы данных](../../relational-databases/collations/set-or-change-the-database-collation.md)   
 [Задание или изменение параметров сортировки столбца](../../relational-databases/collations/set-or-change-the-column-collation.md)   
 [Перестроение системных баз данных](../../relational-databases/databases/rebuild-system-databases.md)  
 
