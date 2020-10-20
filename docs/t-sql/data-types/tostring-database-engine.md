---
description: ToString (компонент Database Engine)
title: ToString (компонент Database Engine)
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ToString
- ToString_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ToString [Database Engine]
ms.assetid: 5fc11ca5-c26d-4518-9512-67aa0270f110
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1d1460c83488f7ba708f737e7c9bb7bd46819152
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037467"
---
# <a name="tostring-database-engine"></a>ToString (компонент Database Engine)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает строку с логическим представлением *this*. Метод ToString вызывается неявно при преобразовании из типа **hierarchyid** в строковый тип. Он действует противоположно методу [Parse (ядро СУБД)](../../t-sql/data-types/parse-database-engine.md).
  
## <a name="syntax"></a>Синтаксис  

```syntaxsql
-- Transact-SQL syntax
node.ToString  ( )
-- This is functionally equivalent to the following syntax  
-- which implicitly calls ToString():  
CAST(node AS nvarchar(4000))  
```  
  
```syntaxsql
-- CLR syntax
string ToString  ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных

**Возвращаемый тип SQL Server:nvarchar(4000)**
  
**Возвращаемый тип CLR:String**
  
## <a name="remarks"></a>Remarks  
Возвращает логическое место в иерархии. Например, `/2/1/` представляет четвертую строку ([!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) в следующей иерархической структуре файловой системы:
  
```sql
/        C:\  
/1/      C:\Database Files  
/2/      C:\Program Files  
/2/1/    C:\Program Files\Microsoft SQL Server  
/2/2/    C:\Program Files\Microsoft Visual Studio  
/3/      C:\Windows  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-transact-sql-example-in-a-table"></a>A. Пример использования Transact-SQL в таблице  
В следующем примере столбец `OrgNode` возвращается и как тип данных **hierarchyid**, и в более удобном для чтения строковом формате:
  
```sql
SELECT OrgNode,  
OrgNode.ToString() AS Node  
FROM HumanResources.EmployeeDemo  
ORDER BY OrgNode ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
OrgNode   Node  
0x        /  
0x58      /1/  
0x5AC0    /1/1/  
0x5B40    /1/2/  
0x5BC0    /1/3/  
0x5C20    /1/4/  
...  
```  
  
### <a name="b-converting-transact-sql-values-without-a-table"></a>Б. Преобразование значений Transact-SQL без таблицы  
В приведенном ниже примере кода метод `ToString` преобразует значение **hierarchyid** в строку, а метод `Parse` преобразует строковое значение в **hierarchyid**.
  
```sql
DECLARE @StringValue AS nvarchar(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="c-clr-example"></a>В. Пример CLR  
В следующем фрагменте кода вызывается метод ToString():
  
```sql
this.ToString()  
```  
  
## <a name="see-also"></a>См. также раздел
[Справочник по методам типа данных hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Иерархические данные (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
