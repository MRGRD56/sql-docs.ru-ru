---
description: Отключение проверочных ограничений для репликации
title: Отключение проверочных ограничений для репликации | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- CHECK constraints, disabling
- constraints [SQL Server], disabling
- disabling constraints
- constraints [SQL Server], check
ms.assetid: af98fc70-24dd-4bd3-a0a3-f701dfa67b2c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1acf4970afde603aa58332c49cb9ea744999cb66
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748614"
---
# <a name="disable-check-constraints-for-replication"></a>Отключение проверочных ограничений для репликации
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Отключить проверочные ограничения в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] можно при помощи среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Также можно явно отключить проверочные ограничения при репликации, что может оказаться полезным при публикации данных от более ранней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Если таблица опубликована с использованием репликации, то проверочные ограничения для операций, выполняемых агентами репликации, автоматически отключаются. Когда агент репликации на подписчике выполняет вставку, обновление или удаление, ограничение не проверяется. Если эту же операцию выполняет пользователь, ограничение проверяется. Ограничение отключено для агента репликации по той причине, что оно уже проверено на издателе при выполнении исходной операции вставки, обновления или удаления данных. Дополнительные сведения см. в разделе [Указание параметров схемы](../../relational-databases/replication/publish/specify-schema-options.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER на таблицу.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>Отключение проверочных ограничений при репликации  
  
1.  Разверните в **обозревателе объектов** таблицу с проверочным ограничением, которое нужно изменить, а затем разверните папку **Ограничения** .  
  
2.  Щелкните правой кнопкой мыши проверочное ограничение, которое нужно изменить, и выберите пункт **Изменить**.  
  
3.  В диалоговом окне **Проверочные ограничения** в разделе **Конструктор таблиц** выберите для параметра **Включить использование для репликации** значение **Нет**.  
  
4.  Щелкните **Закрыть**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>Отключение проверочных ограничений при репликации  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В примере создается таблица со столбцом IDENTITY и ограничением CHECK. Затем в таблице удаляется это ограничение и снова создается с указанием предложения NOT FOR REPLICATION.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.doc_exd (column_a int IDENTITY (1,1)   
    CONSTRAINT exd_check CHECK (column_a > 1))   
  
    ALTER TABLE dbo.doc_exd   
    DROP CONSTRAINT exd_check;   
    GO  
    ALTER TABLE dbo.doc_exd    
    ADD CONSTRAINT exd_check CHECK NOT FOR REPLICATION (column_a > 1);  
    ```  
  
 Дополнительные сведения см. в разделе [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="TsqlExample"></a>   
## <a name="see-also"></a>См. также:  
 [Specify Schema Options (Указание параметров схемы)](../../relational-databases/replication/publish/specify-schema-options.md)  
  
  
