---
title: Создание, изменение и удаление селективных XML-индексов | Документация Майкрософт
description: Сведения о создании нового селективного XML-индекса, а также об изменении или удалении существующего селективного XML-индекса.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: c398f396-f630-4a2d-a264-f243c5346de1
author: rothja
ms.author: jroth
ms.openlocfilehash: a2e6b5b0ee1b5ea0d4ea0ea6dc0982c4a9da40fb
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491005"
---
# <a name="create-alter-and-drop-selective-xml-indexes"></a>Создание, изменение и удаление селективных XML-индексов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Описывает способы создания нового селективного XML-индекса, изменения или удаления существующего селективного XML-индекса.  
  
 Дополнительные сведения о селективных XML-индексах см. в разделе [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
##  <a name="creating-a-selective-xml-index"></a><a name="create"></a> Создание селективного XML-индекса  
  
### <a name="how-to-create-a-selective-xml-index"></a>Руководство. Создание селективного XML-индекса  
 **Создание нового селективного XML-индекса с помощью Transact-SQL**  
 Создание селективного XML-индекса путем вызова инструкции CREATE SELECTIVE XML INDEX. Дополнительные сведения см. в разделе [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
 **Пример**  
  
 В следующем примере показан синтаксис для создания селективного XML-индекса. Он также содержит несколько вариантов синтаксиса для описания индексируемых путей с указаниями по оптимизации.  
  
```sql  
CREATE SELECTIVE XML INDEX sxi_index  
ON Tbl(xmlcol)  
  
FOR(  
    pathab   = '/a/b' as XQUERY 'node()'  
    pathabc  = '/a/b/c' as XQUERY 'xs:double',   
    pathdtext = '/a/b/d/text()' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON  
    pathabe = '/a/b/e' as SQL NVARCHAR(100)  
)  
```  
  
  
##  <a name="altering-a-selective-xml-index"></a><a name="alter"></a> Изменение селективного XML-индекса  
  
### <a name="how-to-alter-a-selective-xml-index"></a>Руководство. Изменение селективного XML-индекса  
 **изменить селективный XML-индекс с помощью Transact-SQL**  
 Измените существующий селективный XML-индекс с помощью инструкции ALTER INDEX. Дополнительные сведения см. в разделе [ALTER INDEX (селективные XML-индексы)](../../t-sql/statements/alter-index-selective-xml-indexes.md).  
  
 **Пример**  
  
 В следующем примере показана инструкция ALTER INDEX. Эта инструкция добавляет путь `'/a/b/m'` в часть XQuery индекса и удаляет путь `'/a/b/e'` из части SQL индекса, созданного в примере в разделе [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md). Путь для удаления определяется по имени, указанному при его создании.  
  
```sql  
ALTER INDEX sxi_index  
ON Tbl  
FOR   
(  
    ADD pathm = '/a/b/m' as XQUERY 'node()' ,  
    REMOVE pathabe  
)  
```  
  
  
##  <a name="dropping-a-selective-xml-index"></a><a name="drop"></a> Удаление селективного XML-индекса  
  
### <a name="how-to-drop-a-selective-xml-index"></a>Руководство. Удаление селективного XML-индекса  
 **Удаление селективного XML-индекса с помощью Transact-SQL**  
 Удалите селективный XML-индекс с помощью инструкции DROP INDEX. Дополнительные сведения см. в разделе [DROP INDEX (селективные XML-индексы)](../../t-sql/statements/drop-index-selective-xml-indexes.md).  
  
 **Пример**  
  
 В следующем примере показана инструкция DROP INDEX.  
  
```sql  
DROP INDEX sxi_index ON tbl  
```  
  
  
  
