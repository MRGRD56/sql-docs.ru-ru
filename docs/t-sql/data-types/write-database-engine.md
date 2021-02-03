---
description: Write (компонент Database Engine)
title: Write (ядро СУБД) | Документы Майкрософт
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Write_TSQL
- Write
dev_langs:
- TSQL
helpviewer_keywords:
- Write [Database Engine]
ms.assetid: 7c554334-d2d9-4eae-a4ae-097aa4020e1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: aba1271dd68224d237595a81d61b23d8db38b7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211490"
---
# <a name="write-database-engine"></a>Write (компонент Database Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Write записывает двоичное представление идентификатора **SqlHierarchyId** в передаваемый на входе объект **BinaryWriter**. Write невозможно вызвать с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)]. Пользуйтесь вместо этого инструкцией CAST или CONVERT.
  
## <a name="syntax"></a>Синтаксис  
  
```csharp
void Write( BinaryWriter w )
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*w*  
Объект **BinaryWriter**, в который будет записано двоичное представление этого узла **hierarchyid**.
  
## <a name="return-types"></a>Типы возвращаемых значений  
**Возвращаемый тип CLR:void**
  
## <a name="remarks"></a>Remarks  
При необходимости Write используется в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для внутренних целей, например при загрузке данных из столбца **hierarchyid**. Write также используется для внутренних целей, когда выполняется преобразование между **hierarchyid** и **varbinary**.
  
## <a name="examples"></a>Примеры  
  
```csharp
MemoryStream stream = new MemoryStream();  
BinaryWriter bw = new BinaryWriter(stream);  
hid.Write(bw);  
byte[] encoding = stream.ToArray();  
```  
  
## <a name="see-also"></a>См. также
[Read (ядро СУБД)](../../t-sql/data-types/read-database-engine.md)  
[ToString (ядро СУБД)](../../t-sql/data-types/tostring-database-engine.md)  
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Справочник по методам типа данных hierarchyid](./hierarchyid-data-type-method-reference.md)
  
