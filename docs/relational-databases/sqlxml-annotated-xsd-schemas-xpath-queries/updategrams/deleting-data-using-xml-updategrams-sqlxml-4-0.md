---
title: Удаление данных с помощью XML диаграмм обновления (SQLXML)
description: Сведения об удалении данных с помощью диаграмма обновления XML в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- <after> block
- updategrams [SQLXML], deleting data
- <before> block
- mapping-schema attribute
- record deletions [SQLXML]
ms.assetid: 4fb116d7-7652-474a-a567-cb475a20765c
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d6923cbe4ce1968992a7944e3a4aedce41bcd7ac
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491556"
---
# <a name="deleting-data-using-xml-updategrams-sqlxml-40"></a>Удаление данных с помощью диаграмм обновления XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Диаграмма обновления указывает операцию удаления, когда экземпляр записи появляется в **\<before>** блоке без соответствующих записей в **\<after>** блоке. В этом случае диаграмма обновления удаляет запись **\<before>** из базы данных в блоке.  
  
 Формат диаграммы обновления для операции удаления:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   <updg:before>  
       <ElementName />  
      [<ElementName .../>... ]  
   </updg:before>  
    [<updg:after>  
    </updg:after>]  
  </updg:sync>  
</ROOT>  
```  
  
 Тег можно опустить, **\<after>** Если диаграмма обновления выполняет только операцию удаления. Если необязательный атрибут **Mapping-Schema** не указан, то **\<ElementName>** указанный в диаграмма обновления сопоставляется с таблицей базы данных, а дочерние элементы или атрибуты сопоставляются со столбцами в таблице.  
  
 Если элемент, указанный в диаграмма обновления, либо соответствует нескольким строкам в таблице, либо не соответствует ни одной строке, диаграмма обновления возвращает ошибку и отменяет весь **\<sync>** блок. В каждый момент времени лишь одна запись может быть удалена элементом в диаграмме обновления.  
  
## <a name="examples"></a>Примеры  
 В примерах этого раздела используется сопоставление по умолчанию (то есть в диаграмме обновления схема сопоставления не указана). Дополнительные примеры диаграмм обновления, в которых используются схемы сопоставления, см. [в разделе Указание схемы сопоставления с заметками в диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
 Чтобы создать рабочие образцы с помощью следующих примеров, необходимо выполнить требования, указанные в [требованиях к запуску примеров SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-deleting-a-record-by-using-an-updategram"></a>A. Удаление записи с помощью диаграммы обновления  
 Следующие диаграммы обновления удаляют две записи из таблицы HumanResources.Shift.  
  
 В этих примерах диаграмма обновления не указывает схему сопоставления. Поэтому диаграмма обновления использует сопоставление по умолчанию, в котором имя элемента соответствует имени таблицы, а атрибуты и вложенные элементы соответствуют столбцам.  
  
 Первый диаграмма обновления является атрибутивной и определяет две смены (Day-вечером и вечером-ночь) в **\<before>** блоке. Так как в блоке нет соответствующей записи **\<after>** , это операция удаления.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
       <HumanResources.Shift ShiftID="4"  
                        Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift ShiftID="5"  
                        Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:before>  
  <updg:after>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Полный пример б ("Вставка нескольких записей с помощью диаграмма обновления") при [вставке данных с помощью XML диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
2.  Скопируйте диаграмма обновления выше в блокнот и сохраните его как Updategram-RemoveShifts.xml в той же папке, которая использовалась для завершения ("Вставка нескольких записей с помощью диаграмма обновления") при [вставке данных с помощью XML диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
3.  Создайте тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs) и воспользуйтесь им для выполнения диаграммы обновления.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="see-also"></a>См. также  
 [Вопросы безопасности диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
