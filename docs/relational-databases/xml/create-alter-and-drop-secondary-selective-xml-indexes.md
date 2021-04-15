---
title: Создание, изменение и удаление вторичных селективных XML-индексов | Документация Майкрософт
description: Сведения о создании вторичного селективного XML-индекса, а также изменении или удалении существующего вторичного селективного XML-индекса.
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 45128105-833b-40a9-9cc9-1ae03ac0b52b
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 9978c49c22de4930e7b4de35a5c45ee1477062be
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488964"
---
# <a name="create-alter-and-drop-secondary-selective-xml-indexes"></a>Создание, изменение и удаление вторичных селективных XML-индексов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Описание создания нового вторичного селективного XML-индекса, а также изменения или удаления существующего вторичного селективного XML-индекса.  
  
##  <a name="creating-a-secondary-selective-xml-index"></a><a name="create"></a> Создание вторичного селективного XML-индекса  
  
### <a name="how-to-create-a-secondary-selective-xml-index"></a>Руководство. Создание вторичного селективного XML-индекса  
 **Создание вторичного селективного XML-индекса с использованием Transact-SQL**  
 Создание вторичного селективного XML-индекса путем вызова инструкции CREATE XML INDEX. Дополнительные сведения см. в статье [CREATE XML INDEX (селективные XML-индексы)](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
 **Пример**  
  
 В следующем примере создается вторичный селективный XML-индекс с путем `'pathabc'`. Путь к индексу определяется по имени, заданному для него при создании с помощью инструкции CREATE SELECTIVE XML INDEX. Дополнительные сведения см. в разделе [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
```sql  
CREATE XML INDEX filt_sxi_index_c  
ON Tbl(xmlcol)  
USING XML INDEX sxi_index  
FOR  
(  
    pathabc  
)  
```  
  
  
##  <a name="altering-a-secondary-selective-xml-index"></a><a name="alter"></a> Изменение вторичного селективного XML-индекса  
 Инструкция ALTER не поддерживается для вторичных селективных XML-индексов. Чтобы изменить вторичный селективный XML-индекс, удалите существующий индекс и создайте его повторно.  
  
### <a name="how-to-alter-a-secondary-selective-xml-index"></a>Руководство. Изменение вторичного селективного XML-индекса  
 **Изменение вторичного селективного XML-индекса с использованием Transact-SQL**  
 1.  Удаление существующего вторичного селективного XML-индекса путем вызова инструкции DROP INDEX. Дополнительные сведения см. в разделе [DROP INDEX (селективные XML-индексы)](../../t-sql/statements/drop-index-selective-xml-indexes.md).  
  
2.  Повторное создание индекса с требуемыми параметрами путем вызова инструкции CREATE XML INDEX. Дополнительные сведения см. в статье [CREATE XML INDEX (селективные XML-индексы)](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
 **Пример**  
  
 В следующем примере показан способ изменения вторичного селективного XML-индекса путем его удаления и повторного создания.  
  
```sql  
DROP INDEX filt_sxi_index_c  
  
CREATE XML INDEX filt_sxi_index_c  
ON Tbl(xmlcol)  
USING XML INDEX sxi_index  
FOR  
(  
    pathabc  
)  
```  
  
  
##  <a name="dropping-a-secondary-selective-xml-index"></a><a name="drop"></a> Удаление вторичного селективного XML-индекса  
  
### <a name="how-to-drop-a-secondary-selective-xml-index"></a>Руководство. Удаление вторичного селективного XML-индекса  
 **Удаление вторичного селективного XML-индекса с использованием Transact-SQL**  
 Удаление вторичного селективного XML-индекса путем вызова инструкции DROP INDEX. Дополнительные сведения см. в разделе [DROP INDEX (селективные XML-индексы)](../../t-sql/statements/drop-index-selective-xml-indexes.md).  
  
 **Пример**  
  
 В следующем примере показана инструкция DROP INDEX.  
  
```sql  
DROP INDEX ssxi_index  
ON tbl  
```  
  
  
## <a name="see-also"></a>См. также:  
 [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
  
  
