---
description: Удаление столбцов из таблицы
title: Удаление столбцов из таблицы | Документация Майкрософт
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], deleting
- removing columns
- deleting columns
- dropping columns
ms.assetid: 0d8f6e4f-bc71-4fa3-8615-74249c8e072d
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31ed97bd88c560d884d51569af7d3278bc9c6f89
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743334"
---
# <a name="delete-columns-from-a-table"></a>Удаление столбцов из таблицы

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

В этом разделе приведены инструкции по удалению столбцов таблиц в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].

> [!CAUTION]
> При удалении столбца из таблицы удаляются сам столбец и все содержащиеся в нем данные.

 **В этом разделе**

- **Перед началом работы**

   [Ограничения](#Restrictions)

   [Безопасность](#Security)

- **Удаление столбца из таблицы с помощью:**

   [Среда SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения

Нельзя удалить столбец, имеющий ограничение CHECK. В первую очередь необходимо удалить ограничение.

Удалить столбец, имеющий ограничения PRIMARY KEY, FOREIGN KEY или другие зависимости можно только с использованием конструктора таблиц. При использовании обозревателя объектов или [!INCLUDE[tsql](../../includes/tsql-md.md)]необходимо в первую очередь удалить зависимости столбца.

### <a name="security"></a><a name="Security"></a> безопасность

#### <a name="permissions"></a><a name="Permissions"></a> Permissions

Требуется разрешение ALTER на таблицу.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio

### <a name="to-delete-columns-by-using-object-explorer"></a>Удаление столбцов с помощью обозревателя объектов

1. В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. В **обозревателе объектов** найдите таблицу, из которой нужно удалить столбцы, и разверните ее, чтобы отобразить имена столбцов.
3. Щелкните правой кнопкой мыши столбец, который необходимо удалить, и выберите команду **Удалить**.
4. В диалоговом окне **Удаление объекта** нажмите кнопку **ОК**.

Если столбец содержит ограничения или другие зависимости, то в диалоговом окне **Удаление объекта** будет отображено сообщение об ошибке. Чтобы устранить проблему, удалите упомянутые ограничения.

### <a name="to-delete-columns-by-using-table-designer"></a>Удаление столбцов с использованием конструктора таблиц

1. В **обозревателе объектов** щелкните правой кнопкой мыши таблицу, из которой необходимо удалить столбцы, и выберите пункт **Конструктор**.
2. Щелкните правой кнопкой мыши столбец, который надо удалить, и выберите из контекстного меню пункт **Удалить столбец** .
3. Если столбец участвует в связи (FOREIGN KEY или PRIMARY KEY), то будет выдано сообщение с запросом на подтверждение удаления выбранных столбцов и их связей. выберите **Yes** (Да).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL

### <a name="to-delete-columns"></a>Удаление столбцов

В следующем примере демонстрируется удаление столбца.

```sql
ALTER TABLE dbo.doc_exb DROP COLUMN column_b;
```

Если столбец содержит ограничения или другие зависимости, то будет возвращено сообщение об ошибке. Чтобы устранить проблему, удалите упомянутые ограничения.

Дополнительные примеры см. в статье [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).

## <a name="FollowUp"></a>
