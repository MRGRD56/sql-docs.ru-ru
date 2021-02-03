---
description: GetReparentedValue (компонент Database Engine)
title: GetReparentedValue (ядро СУБД) | Документы Майкрософт
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reparent_TSQL
- Reparent
dev_langs:
- TSQL
helpviewer_keywords:
- Reparent [Database Engine]
ms.assetid: f47f8e25-08ef-498b-84f4-a317aca1f358
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c71c30acd6832ed2f53941bd3e841eb235c7b1cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165159"
---
# <a name="getreparentedvalue-database-engine"></a>GetReparentedValue (компонент Database Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает узел, путь к которому от корневого элемента состоит из пути к _newRoot_ и пути от _oldRoot_.
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Transact-SQL syntax  
node. GetReparentedValue ( oldRoot, newRoot )  
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetReparentedValue ( SqlHierarchyId oldRoot , SqlHierarchyId newRoot )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
_oldRoot_  
**hierarchyid** узла, представляющего уровень иерархии, который подлежит изменению.
  
_newRoot_  
Тип **hierarchyid**, представляющий узел. Замените раздел _oldRoot_ текущего узла, чтобы переместить этот узел.
  
## <a name="return-types"></a>Типы возвращаемых данных  
**Возвращаемый тип SQL Server:hierarchyid**
  
**Возвращаемый тип CLR:SqlHierarchyId**
  
## <a name="remarks"></a>Примечания  
Используется для изменения дерева путем перемещения узлов из _oldRoot_ в _newRoot_. Метод GetReparentedValue используется для перемещения узла иерархии в новое расположение в иерархии. Тип данных **hierarchyid** представляет иерархическую структуру, но не обеспечивает ее соблюдение. Пользователям необходимо убедиться в том, что структура идентификатора иерархии пригодна для нового местоположения. Уникальный индекс с типом данных **hierarchyid** позволяет избежать повторения записей. Пример перемещения поддерева целиком см. в статье [Иерархические данные (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md).
  
## <a name="examples"></a>Примеры  
  
### <a name="a-comparing-two-node-locations"></a>A. Сравнение двух местоположений узла  
В следующем примере показан текущий идентификатор иерархии для узла. В нем также демонстрируется, как будет выглядеть **hierarchyid** узла, если при перемещении этот узел станет потомком узла **\@NewParent**. В примере используется метод `ToString()` для демонстрации иерархических связей.
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ;  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- who is /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- who is /2/3/  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
(@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) ).ToString() AS Proposed_OrgNode_AS_Text,  
OrgNode AS Current_OrgNode,  
@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) AS Proposed_OrgNode,  
*  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = @SubjectEmployee ;  
GO  
```  
  
### <a name="b-updating-a-node-to-a-new-location"></a>Б. Перемещение узла в новое местоположение путем обновления  
В следующем примере используется метод `GetReparentedValue()` в инструкции UPDATE для перемещения узла из прежнего в новое местоположение в иерархии.
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ; -- Node /1/1/2/  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- Node /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- Node /2/3/  
  
UPDATE HumanResources.EmployeeDemo  
SET OrgNode = @SubjectEmployee. GetReparentedValue(@OldParent, @NewParent)   
WHERE OrgNode = @SubjectEmployee ;  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
*  
FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\gail0' ; -- Now node /2/3/2/  
```  
  
### <a name="c-clr-example"></a>В. Пример CLR  
В следующем фрагменте кода вызывается метод GetReparentedValue ():
  
```sql
this. GetReparentedValue(oldParent, newParent)  
```  
  
## <a name="see-also"></a>См. также раздел
[Справочник по методам типа данных hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Иерархические данные (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
