---
description: Добавление столбцов в таблицу (компонент Database Engine)
title: Добавление столбцов в таблицу (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- inserting columns
- columns [SQL Server], adding
- adding columns
ms.assetid: abeb8d52-d562-4e29-9e1e-2923ae874859
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f10eec1fac96247e2d909c6014fcc98c5568d91
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754964"
---
# <a name="add-columns-to-a-table-database-engine"></a>Добавление столбцов в таблицу (компонент Database Engine)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

В этой статье содержатся инструкции по добавлению новых столбцов в таблицу в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при помощи [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения

 Использование инструкции ALTER TABLE для добавления столбцов в таблицу приводит к автоматическому добавлению этих столбцов в конец таблицы. Если требуется, чтобы столбцы располагались в таблице в определенном порядке, воспользуйтесь [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Однако помните, что это не рекомендуемый метод конструирования баз данных. Рекомендуется указывать порядок, в котором возвращаются столбцы, на уровне приложения и запроса. Не следует предполагать, что SELECT * будет возвращать все столбцы в ожидаемом порядке, основанном на порядке их определения в таблице. Всегда указывайте столбцы в запросах и приложениях по именам в том порядке, в котором они должны следовать.

### <a name="security"></a><a name="Security"></a> безопасность

#### <a name="permissions"></a><a name="Permissions"></a> Permissions

Требуется разрешение ALTER на таблицу.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio

### <a name="to-insert-columns-into-a-table-with-table-designer"></a>Вставка в таблицу столбцов с помощью конструктора таблиц

1. В **обозревателе объектов** щелкните правой кнопкой мыши таблицу, в которую необходимо добавить столбцы, и выберите пункт **Конструктор**.
2. Щелкните первую пустую ячейку в столбце **Имя столбца** .
3. Введите имя столбца в ячейку. Имя столбца — значение, которое необходимо указать.
4. Нажмите клавишу TAB, чтобы перейти к ячейке **Тип данных** и выбрать тип данных из раскрывающегося списка.

   Это — обязательное значение, и если его не указать, будет использоваться значение по умолчанию.

   > [!NOTE]
   >  Значения по умолчанию можно установить или изменить в диалоговом окне **Параметры** в **Инструменты для баз данных**.

5. Продолжайте определение других свойств столбца во вкладке **Свойства столбца** .

    > [!NOTE]
    >  При создании нового столбца для свойств столбца устанавливаются значения по умолчанию, но их можно изменить во вкладке **Свойства столбца** .

6. По окончании добавления столбцов из меню **Файл** выберите пункт **Сохранить** _имя таблицы_.
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL
  
### <a name="to-insert-columns-into-a-table"></a>Вставка столбцов в таблицу  
  
В следующем примере добавляются два столбца в таблицу `dbo.doc_exa`.

```sql
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL, column_c INT NULL ;
```

#### <a name="for-more-information-see-alter-table-40transact-sql41"></a><a name="FollowUp"></a>Дополнительные сведения см. в разделе [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).
