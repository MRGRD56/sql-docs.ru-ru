---
description: NEWSEQUENTIALID (Transact-SQL)
title: NEWSEQUENTIALID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NEWSEQUENTIALID
- NEWSEQUENTIALID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEWSEQUENTIALID function
- GUIDs [SQL Server]
ms.assetid: e06d2cab-f1ff-42f1-8550-6aaec57be36f
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4fe754848be676d40387d4887a5a7e519da697f7
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2020
ms.locfileid: "91111050"
---
# <a name="newsequentialid-transact-sql"></a>NEWSEQUENTIALID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создает идентификатор GUID, имеющий значение, большее любого идентификатора GUID, который был прежде создан на указанном компьютере при помощи этой функции с момента запуска Windows. После перезагрузки Windows идентификатор GUID может вновь начинаться с более низкого диапазона, однако останется глобально уникальным. Если столбец GUID используется в качестве идентификатора строки, использование NEWSEQUENTIALID может ускорить работу по сравнению с использованием функции NEWID, поскольку функция NEWID вызывает непредсказуемый уровень нагрузки и использует меньшее число кэшированных страниц данных. Использование NEWSEQUENTIALID также помогает полностью заполнять страницы данных и страницы индекса.  
  
> [!IMPORTANT]  
>  Если важна конфиденциальность, то не следует применять эту функцию. Значение следующего формируемого идентификатора GUID можно предугадать и, следовательно, получить доступ к данным, связанным с этим идентификатором GUID.  
  
 NEWSEQUENTIALID является оболочкой для функции Windows [UuidCreateSequential](https://go.microsoft.com/fwlink/?LinkId=164027) с применением [случайной перестановки байт](https://blogs.msdn.microsoft.com/dbrowne/2012/07/03/how-to-generate-sequential-guids-for-sql-server-in-net/).
  
> [!WARNING]  
>  У функции UuidCreateSequential есть аппаратные зависимости. На [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно разработать кластеры последовательных значений при перемещении баз данных (например, автономных баз данных) на другие компьютеры. При использовании AlwaysOn и на [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] кластеры последовательных значений можно разработать при отработке отказа базы данных на другой компьютер.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
NEWSEQUENTIALID ( )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]


## <a name="return-type"></a>Тип возвращаемых данных  
 **uniqueidentifier**  
  
## <a name="remarks"></a>Комментарии  
 Функция NEWSEQUENTIALID() может быть использована только для ограничений DEFAULT для столбцов таблицы типа **uniqueidentifier**. Пример:  
  
```sql  
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT NEWSEQUENTIALID());   
```  
  
 Когда функция NEWSEQUENTIALID() используется в выражениях DEFAULT, она не может быть объединена с другими скалярными операторами. Например, нельзя выполнить следующее:  
  
```sql 
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT dbo.myfunction(NEWSEQUENTIALID()));  
```  
  
 В предыдущем примере `myfunction()` является пользовательской скалярной функцией, которая принимает и возвращает значение `uniqueidentifier`.  
  
 На функцию NEWSEQUENTIALID нельзя ссылаться в запросах.  
  
 Функцию NEWSEQUENTIALID можно использовать для формирования идентификаторов GUID, чтобы уменьшить число разбиений страниц и произвольных операций ввода-вывода на конечном уровне индексов.  
  
 Каждый идентификатор GUID, сформированный с помощью функции NEWSEQUENTIALID, является уникальным на данном компьютере. Идентификаторы GUID, сформированные с помощью функции NEWSEQUENTIALID, становятся уникальными для нескольких компьютеров, только если в компьютере-источнике имеется сетевая плата.  
  
## <a name="see-also"></a>См. также:  
 [NEWID (Transact-SQL)](../../t-sql/functions/newid-transact-sql.md)   
 [Операторы сравнения (Transact-SQL)](../../t-sql/language-elements/comparison-operators-transact-sql.md)  
  
  
