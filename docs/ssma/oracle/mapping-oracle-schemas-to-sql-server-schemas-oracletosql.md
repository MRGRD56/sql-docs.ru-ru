---
title: Сопоставление схем Oracle с SQL Server схемами (OracleToSQL) | Документация Майкрософт
description: Узнайте, как настроить SSMA для сопоставлений Oracle между схемами Oracle и SQL Server или принять значение по умолчанию.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 0edeaa08-9c5d-4e3a-bc15-b9a1f0c8a9dc
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 16c4de0bdf08be3bfdbc8644ae61d300284a0c25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080779"
---
# <a name="mapping-oracle-schemas-to-sql-server-schemas-oracletosql"></a>Сопоставление схем Oracle со схемами SQL Server (OracleToSQL)
В Oracle каждая база данных имеет одну или несколько схем. По умолчанию SSMA переносит все объекты в схеме Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базу данных с именем для схемы. Однако можно настроить сопоставление между схемами и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базами данных Oracle.  
  
## <a name="oracle-and-sql-server-schemas"></a>Схемы Oracle и SQL Server  
База данных Oracle содержит схемы. Экземпляр служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит несколько баз данных, каждый из которых может иметь несколько схем.  
  
Концепция Oracle схемы сопоставляется с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] концепцией базы данных и одной из ее схем. Например, Oracle может иметь схему с именем **HR**. Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может иметь базу данных с именем **HR**, а в этой базе данных — схемы. Одной из схем является схема **dbo** (или владелец базы данных). По умолчанию схема Oracle для схемы **HR** будет сопоставлена с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базой данных и схемой **HR. dbo**. SSMA ссылается на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сочетание базы данных и схемы в качестве схемы.  
  
Можно изменить сопоставление между Oracle и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемами.  
  
## <a name="modifying-the-target-database-and-schema"></a>Изменение целевой базы данных и схемы  
В SSMA можно сопоставлять схему Oracle с любой доступной [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемой.  
  
**Изменение базы данных и схемы**  
  
1.  В обозревателе метаданных Oracle выберите **схемы**.  
  
    Вкладка **сопоставление схем** доступна также при выборе отдельной базы данных, папки **Schemas** или отдельных схем. Список на вкладке **сопоставление схемы** настроен для выбранного объекта.  
  
2.  На панели справа перейдите на вкладку **сопоставление схем** .  
  
    Вы увидите список всех схем Oracle, за которыми следует целевое значение. Этот целевой объект обозначается в нотации с двумя частями (*Database. Schema*), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] где объекты и данные будут перенесены.  
  
3.  Выберите строку, содержащую сопоставление, которое необходимо изменить, и нажмите кнопку **изменить**.  
  
    В диалоговом окне **Выбор целевой схемы** можно просмотреть доступную целевую базу данных и схему или ввести имя базы данных и схемы в текстовое поле в нотации с двумя частями (Database. Schema), а затем нажать кнопку **ОК**.  
  
4.  Целевое изменение на вкладке **сопоставление схемы** .  
  
**Режимы сопоставления**  
  
-   Сопоставление с SQL Server  
  
Базу данных источника можно сопоставлять с любой целевой базой данных. По умолчанию база данных-источник сопоставляется с целевой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базой данных, к которой вы подключены с помощью SSMA. Если сопоставляемая Целевая база данных не является существующей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , будет выведено сообщение **"база данных и (или) схема не существует в целевых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] метаданных. Он будет создан во время синхронизации. Вы хотите продолжить?»** Нажмите кнопку "Да". Аналогичным образом можно сопоставлять схему с несуществующей схемой в целевой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базе данных, которая будет создана во время синхронизации.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Возврат к базе данных и схеме по умолчанию  
Если вы настраиваете сопоставление между схемой Oracle и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемой, можно вернуть сопоставление к значениям по умолчанию.  
  
**Возврат к базе данных и схеме по умолчанию**  
  
1.  На вкладке Сопоставление схемы выберите любую строку и нажмите кнопку **восстановить значения по умолчанию** , чтобы вернуться к базе данных и схеме по умолчанию.  
  
## <a name="next-steps"></a>Next Steps  
Если необходимо проанализировать преобразование объектов Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] объекты, можно [создать отчет о преобразовании](assessing-oracle-schemas-for-conversion-oracletosql.md). В противном случае можно [преобразовать определения объектов базы данных Oracle](converting-oracle-schemas-oracletosql.md) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определения объектов.  
  
## <a name="see-also"></a>См. также:  
[Подключение к SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
[Перенос баз данных Oracle в SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
