---
description: Изменение столбцов (компонент Database Engine)
title: Изменение столбцов (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying data types
- column data types [SQL Server]
- data types [SQL Server], columns
ms.assetid: b67b95c5-61ef-4bd8-9a3e-1640eb7583ac
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d64d24cddb9e35e773e8613987732bc863f09e28
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751244"
---
# <a name="modify-columns-database-engine"></a>Изменение столбцов (компонент Database Engine)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Изменить тип данных столбца в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] можно в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или в [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!WARNING]  
>  Изменение типа данных столбца, в котором уже есть данные, может привести к полной потере данных при преобразовании существующих данных в новый тип. Кроме того, код и приложения, которые используют измененный столбец, могут завершиться сбоем. Это касается запросов, представлений, хранимых процедур, определяемых пользователем функций и клиентских приложений. Следует иметь в виду, что возникновение ошибок происходит каскадом. Например, может произойти сбой хранимой процедуры, которая вызывает определяемую пользователем функцию, зависящую от изменяемого столбца. Внимательно рассмотрите любые изменения, которые необходимо сделать со столбцом таблицы.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Изменение типа данных столбца с помощью следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER на таблицу.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>Изменение типа данных столбца  
  
1.  В **Обозревателе объектов** щелкните правой кнопкой мыши таблицу со столбцами, масштаб которых необходимо изменить, и выберите пункт **Конструктор**.  
  
2.  Выберите столбец, тип данных которого планируется изменить.  
  
3.  На вкладке **Свойства столбца** выберите ячейку сетки для свойства **Тип данных** и выберите новый тип данных из раскрывающегося списка.  
  
4.  В меню **Файл** выберите команду **Сохранить**_имя_таблицы_.  
  
> [!NOTE]  
>  При изменении типа данных столбца конструктор таблиц применяет длину типа данных, определенную по умолчанию для выбранного типа данных, даже если была указана другая длина. Всегда устанавливайте необходимое значение длины типа данных после того, как был указан тип данных.  
  
> [!WARNING]  
>  При попытке изменения типа данных столбца, связанного с другими таблицами, конструктор таблиц запрашивает подтверждение на внесение изменений и в столбцы других таблиц.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>Изменение типа данных столбца  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql  
    CREATE TABLE dbo.doc_exy (column_a INT ) ;  
    GO  
    INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;  
    GO  
    ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;  
    GO  
  
    ```  
  
 Дополнительные сведения см. в разделе [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).  
  
  
