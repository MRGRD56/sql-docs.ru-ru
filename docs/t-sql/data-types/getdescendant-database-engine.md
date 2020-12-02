---
description: GetDescendant (компонент Database Engine)
title: GetDescendant (ядро СУБД) | Документы Майкрософт
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GetDescendant_TSQL
- GetDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- GetDescendant [Database Engine]
ms.assetid: f5f39596-033e-4243-acbc-caa188b45b03
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c5e76db626c0129253f343075894825c114da4ec
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "92037159"
---
# <a name="getdescendant-database-engine"></a>GetDescendant (компонент Database Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает дочерний узел данного родительского узла.
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Transact-SQL syntax  
parent.GetDescendant ( child1 , child2 )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetDescendant ( SqlHierarchyId child1 , SqlHierarchyId child2 )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*child1*  
Значение NULL или **hierarchyid** дочернего узла текущего узла.
  
*child2*  
Значение NULL или **hierarchyid** дочернего узла текущего узла.
  
## <a name="return-types"></a>Типы возвращаемых данных  
**Возвращаемый тип SQL Server:hierarchyid**
  
**Возвращаемый тип CLR:SqlHierarchyId**
  
## <a name="remarks"></a>Примечания  
Возвращает один дочерний узел, который является потомком родителя.
-   Если родительская запись — NULL, метод возвращает значение NULL.  
-   Если родительская запись — не NULL, а потомки child1 и child2 — NULL, метод  возвращает одного потомка данного родителя.  
-   Если родительская запись и потомок child1 — не NULL, а потомок child2 равен NULL, метод  возвращает значение потомка родителя, который больше чем child1.  
-   Если родитель и потомок child2 — не NULL, а потомок child1 равен NULL, метод  возвращает значение потомка родителя, который меньше child2.  
-   Если родитель и дочерние узлы child1 и child2 не равны NULL, метод возвращает дочерний узел родителя, который больше child1 и меньше child2.  
-   Если child1 не равен NULL и не является дочерним узлом родителя, то возникает исключение.  
-   Если child2 не равен NULL и не является дочерним узлом родителя, то возникает исключение.  
-   Если child1 >= child2, то возникает исключение.  
  
Метод GetDescendant является детерминированным. Поэтому, если метод GetDescendant вызывается с одинаковыми входными данными, результат будет таким же. Однако точный идентификатор полученного дочернего элемента может быть различным в зависимости от связей с другими узлами, как показано в примере В.
  
## <a name="examples"></a>Примеры  
  
### <a name="a-inserting-a-row-as-the-least-descendant-node"></a>A. Вставка строки как наименьшего узла-потомка  
Новый сотрудник принят на работу, подотчетный существующему сотруднику в узле `/3/1/`. Выполните следующий код, чтобы вставить новую строку с использованием метода GetDescendant без аргументов, указав новый узел строк как `/3/1/1/`:
  
```sql
DECLARE @Manager hierarchyid;   
SET @Manager = CAST('/3/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(NULL, NULL),  
'adventure-works\FirstNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="b-inserting-a-row-as-a-greater-descendant-node"></a>Б. Вставка строки как наибольшего узла-потомка  
На работу принят другой новый сотрудник, подотчетный тому же руководителю, что и в примере А. Выполните следующий код, чтобы вставить новую строку с использованием метода GetDescendant, в котором аргумент child 1 указывает, что узел новой строки последует за узлом в примере А, принимая вид `/3/1/2/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, NULL),  
'adventure-works\SecondNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="c-inserting-a-row-between-two-existing-nodes"></a>В. Вставка строки между двумя существующими узлами  
На работу принят третий сотрудник, подотчетный тому же руководителю, что и в примере А. В этом примере вставляется новая строка в узел, больший, чем `FirstNewEmployee` в примере А, и меньший, чем `SecondNewEmployee` в примере Б. Выполните приведенный ниже код с использованием метода GetDescendant. Используйте как аргумент child1, так и аргумент child2, чтобы указать, что узел новой строки будет узлом `/3/1/1.1/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid, @Child2 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
SET @Child2 = CAST('/3/1/2/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, @Child2),  
'adventure-works\ThirdNewEmployee', 'Application Intern', '3/11/07') ;  
  
```  
  
После выполнения примеров А, Б и В узлы, добавленные в таблицу, будут одноранговыми со следующими значениями **hierarchyid**:
  
`/3/1/1/`
  
`/3/1/1.1/`
  
`/3/1/2/`
  
Узел `/3/1/1.1/` больше узла `/3/1/1/`, но находится на том же уровне иерархии.
  
### <a name="d-scalar-examples"></a>Г. Скалярные примеры  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает произвольные вставки и удаления любых узлов **hierarchyid**. С помощью метода GetDescendant() всегда можно создать узел между любыми двумя узлами **hierarchyid**. Выполните следующий код, чтобы сформировать образцы узлов с использованием `GetDescendant`:
  
```sql
DECLARE @h hierarchyid = hierarchyid::GetRoot();  
DECLARE @c hierarchyid = @h.GetDescendant(NULL, NULL);  
SELECT @c.ToString();  
DECLARE @c2 hierarchyid = @h.GetDescendant(@c, NULL);  
SELECT @c2.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
SET @c = @h.GetDescendant(@c, @c2);  
SELECT @c.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
```  
  
### <a name="e-clr-example"></a>Д. Пример CLR  
В следующем фрагменте кода вызывается метод `GetDescendant()`.
  
```sql
SqlHierarchyId parent, child1, child2;  
parent = SqlHierarchyId.GetRoot();  
child1 = parent.GetDescendant(SqlHierarchyId.Null, SqlHierarchyId.Null);  
child2 = parent.GetDescendant(child1, SqlHierarchyId.Null);  
Console.Write(parent.GetDescendant(child1, child2).ToString());  
```  
  
## <a name="see-also"></a>См. также раздел
[Справочник по методам типа данных hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Иерархические данные (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
