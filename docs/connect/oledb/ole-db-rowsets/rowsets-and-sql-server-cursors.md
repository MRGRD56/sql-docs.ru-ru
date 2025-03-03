---
title: Наборы строк и курсоры SQL Server (драйвер OLE DB)
description: Узнайте, как SQL Server возвращает результирующие наборы объектам-получателям и чем результирующие наборы по умолчанию отличаются от серверных курсоров в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB rowsets, cursors
- OLE DB Driver for SQL Server, rowsets
- rowsets [OLE DB], cursors
- properties [OLE DB]
- cursors [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f7b5fa8d0dabcdb5d6355bc5b34764f4ce84edd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755324"
---
# <a name="rowsets-and-sql-server-cursors"></a>Наборы строк и курсоры SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] возвращает результирующие наборы пользователям двумя способами.  
  
-   Результирующие наборы по умолчанию, которые характеризуются следующим:  
  
    -   Сводят издержки к минимуму.  
  
    -   Обеспечивают максимальную производительность при выборке данных.  
  
    -   Поддерживают лишь функции однопроходных курсоров только для чтения.  
  
    -   Возвращают пользователю по одной строке одновременно.  
  
    -   Поддерживают одновременно только одну активную инструкцию в каждом соединении.  
  
         После выполнения инструкции другие инструкции можно будет выполнить в этом соединении только после извлечения пользователем всех результатов или отмены этой инструкции.  
  
    -   Поддерживают все инструкции [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Серверные курсоры, которые характеризуются следующим:  
  
    -   Поддерживают все функции курсоров.  
  
    -   Могут возвращать пользователю блоки строк.  
  
    -   Поддерживают несколько активных инструкций в одном соединении.  
  
    -   Позволяют выбирать наиболее применимые функциональные средства курсора с точки зрения производительности.  
  
         Поддержка функциональных средств курсора может привести к снижению производительности по отношению к результирующему набору по умолчанию. Это снижение производительности можно скомпенсировать, если пользователь имеет возможность применять функциональные средства курсоров для извлечения набора строк меньшего размера.  
  
    -   Не поддерживают инструкции [!INCLUDE[tsql](../../../includes/tsql-md.md)], которые возвращают несколько результирующих наборов.  
  
 Пользователь может запросить другой режим работы курсоров, установив определенные свойства набора строк. Если пользователь не задает ни одного из этих свойств набора строк или задает для них всех значения по умолчанию, то драйвер OLE DB для SQL Server реализует набор строк с помощью результирующего набора по умолчанию. Если любому из этих свойств присвоено значение, отличное от применяемого по умолчанию, то драйвер OLE DB для SQL Server реализует набор строк с помощью серверного курсора.  
  
 Следующие свойства набора строк указывают OLE DB Driver for SQL Server использовать курсоры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Некоторые свойства можно безопасно сочетать с другими. Например, набор строк, предоставляющий доступ к свойствам DBPROP_IRowsetScroll и DBPROP_IRowsetChange, становится набором строк с закладками, обеспечивающим возможность немедленного обновления. Другие свойства являются взаимоисключающими. Например, набор строк со свойством DBPROP_OTHERINSERT не может содержать закладок.  
  
|Идентификатор свойства|Значение|Поведение набора строк|  
|-----------------|-----------|---------------------|  
|DBPROP_SERVERCURSOR|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк является последовательным и поддерживает только прямую прокрутку и выборку. Относительное позиционирование строки поддерживается. Текст команды может содержать предложение ORDER BY.|  
|DBPROP_CANSCROLLBACKWARDS или DBPROP_CANFETCHBACKWARDS|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает прокрутку и выборку в любом направлении. Относительное позиционирование строки поддерживается. Текст команды может содержать предложение ORDER BY.|  
|DBPROP_BOOKMARKS или DBPROP_LITERALBOOKMARKS|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк является последовательным и поддерживает только прямую прокрутку и выборку. Относительное позиционирование строки поддерживается. Текст команды может содержать предложение ORDER BY.|  
|DBPROP_OWNUPDATEDELETE, DBPROP_OWNINSERT или DBPROP_OTHERUPDATEDELETE|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает прокрутку и выборку в любом направлении. Относительное позиционирование строки поддерживается. Текст команды может содержать предложение ORDER BY.|  
|DBPROP_OTHERINSERT|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает прокрутку и выборку в любом направлении. Относительное позиционирование строки поддерживается. Текст команды может включать предложение ORDER BY, если для указанных в ссылке столбцов существует индекс.<br /><br /> Если набор строк содержит закладки, свойство DBPROP_OTHERINSERT не может иметь значение VARIANT_TRUE. При попытке создать набор строк с этим свойством видимости и закладками возникает ошибка.|  
|DBPROP_IRowsetLocate или DBPROP_IRowsetScroll|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает прокрутку и выборку в любом направлении. В наборе строк поддерживаются закладки и абсолютное позиционирование с помощью интерфейса **IRowsetLocate**. Текст команды может содержать предложение ORDER BY.<br /><br /> Свойства DBPROP_IRowsetLocate и DBPROP_IRowsetScroll требуют наличия закладок в наборе строк. При попытке создать набор строк с закладками и свойством DBPROP_OTHERINSERT, которому присвоено значение VARIANT_TRUE, возникает ошибка.|  
|DBPROP_IRowsetChange или DBPROP_IRowsetUpdate|VARIANT_TRUE|Возможно обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк. Этот набор строк является последовательным и поддерживает только прямую прокрутку и выборку. Относительное позиционирование строки поддерживается. Все команды, поддерживающие обновляемые курсоры, могут поддерживать эти интерфейсы.|  
|DBPROP_IRowsetLocate или DBPROP_IRowsetScroll и DBPROP_IRowsetChange или DBPROP_IRowsetUpdate|VARIANT_TRUE|Возможно обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк. Этот набор строк поддерживает прокрутку и выборку в любом направлении. В наборе строк поддерживаются закладки и абсолютное позиционирование с помощью интерфейса **IRowsetLocate**. Текст команды может содержать предложение ORDER BY.|  
|DBPROP_IMMOBILEROWS|VARIANT_FALSE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает только прямую прокрутку. Относительное позиционирование строки поддерживается. Текст команды может включать предложение ORDER BY, если для указанных в ссылке столбцов существует индекс.<br /><br /> Свойство DBPROP_IMMOBILEROWS можно использовать только в наборах строк, которые способны показывать в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] строки, вставленные командами в рамках других сеансов или другими пользователями. При попытке открыть набор строк с этим свойством, заданным равным VARIANT_FALSE, для любого набора строк, для которого свойство DBPROP_OTHERINSERT не может иметь значение VARIANT_TRUE, возникает ошибка.|  
|DBPROP_REMOVEDELETED|VARIANT_TRUE|Обновление данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через набор строк невозможно. Этот набор строк поддерживает только прямую прокрутку. Относительное позиционирование строки поддерживается. Текст команды может содержать предложение ORDER BY, если это не запрещено другим свойством.|  
  
 Набор строк драйвера OLE DB для SQL Server, поддерживаемый серверным курсором, можно легко создать для базовой таблицы или представления [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью метода **IOpenRowset::OpenRowset**. Укажите таблицу или представление по имени, передав требуемые наборы свойств набора строк в параметре *rgPropertySets*.  
  
 Если пользователь требует, чтобы набор строк поддерживался серверным курсором, то возможности выбора текста команды, которая создает набор строк, становятся ограниченными. В частности, текст команды ограничивается применением либо одной инструкции SELECT, которая возвращает один результирующий набор строк, либо хранимой процедуры, реализующей одну инструкцию SELECT, которая возвращает результат в виде одного набора строк.  
  
 В двух этих таблицах показаны сопоставления различных свойств OLE DB и моделей курсоров. В них также показано, какие свойства набора строк следует задать, чтобы использовать модель курсора определенного типа.  
  
 В каждой ячейке таблицы содержится значение свойства набора строк для определенной модели курсора. Типы данных всех свойств наборов строк, приведенных выше в этом разделе, относятся к типу данных VT_BOOL, а их значением по умолчанию является VARIANT_FALSE. В таблице используются следующие символы:  
  
 F = значение по умолчанию (VARIANT_FALSE)  
  
 T = VARIANT_TRUE  
  
 \- = VARIANT_TRUE или VARIANT_FALSE  
  
 Чтобы ввести в действие модель курсора определенного типа, определите столбец, соответствующий этой модели курсора, и найдите все свойства набора строк со значением «Т» в этом столбце. Чтобы воспользоваться данной конкретной моделью курсора, присвойте этим свойствам набора строк значение VARIANT_TRUE. Свойствам набора строк, для которых в качестве значения указано «-», можно присваивать либо значение VARIANT_TRUE, либо значение VARIANT_FALSE.  
  
|Свойства набора строк и модели курсора|По умолчанию<br /><br /> набор по<br /><br /> set<br /><br /> (RO)|быстрый;<br /><br /> однопроходный<br /><br /> только<br /><br /> (RO)|Статические<br /><br /> (RO)|Keyset<br /><br /> управляемый<br /><br /> (RO)|  
|--------------------------------------|-------------------------------------------|--------------------------------------------|-----------------------|----------------------------------|  
|DBPROP_SERVERCURSOR|F|T|T|T|  
|DBPROP_DEFERRED|F|F|-|-|  
|DBPROP_IrowsetChange|F|F|F|F|  
|DBPROP_IrowsetLocate|F|F|-|-|  
|DBPROP_IrowsetScroll|F|F|-|-|  
|DBPROP_IrowsetUpdate|F|F|F|F|  
|DBPROP_BOOKMARKS|F|F|-|-|  
|DBPROP_CANFETCHBACKWARDS|F|F|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|F|F|-|-|  
|DBPROP_CANHOLDROWS|F|F|-|-|  
|DBPROP_LITERALBOOKMARKS|F|F|-|-|  
|DBPROP_OTHERINSERT|F|T|F|F|  
|DBPROP_OTHERUPDATEDELETE|F|T|F|T|  
|DBPROP_OWNINSERT|F|T|F|T|  
|DBPROP_OWNUPDATEDELETE|F|T|F|T|  
|DBPROP_QUICKSTART|F|F|-|-|  
|DBPROP_REMOVEDELETED|F|F|F|-|  
|DBPROP_IrowsetResynch|F|F|F|-|  
|DBPROP_CHANGEINSERTEDROWS|F|F|F|F|  
|DBPROP_SERVERDATAONINSERT|F|F|F|-|  
|DBPROP_UNIQUEROWS|-|F|F|F|  
|DBPROP_IMMOBILEROWS|-|-|-|T|  
  
|Свойства набора строк и модели курсора|Динамический (только для чтения)|С набором ключей (для чтения и записи)|Динамический (для чтения и записи)|  
|--------------------------------------|--------------------|---------------------|----------------------|  
|DBPROP_SERVERCURSOR|T|T|T|  
|DBPROP_DEFERRED|-|-|-|  
|DBPROP_IrowsetChange|F|-|-|  
|DBPROP_IrowsetLocate|F|-|F|  
|DBPROP_IrowsetScroll|F|-|F|  
|DBPROP_IrowsetUpdate|F|-|-|  
|DBPROP_BOOKMARKS|F|-|F|  
|DBPROP_CANFETCHBACKWARDS|-|-|-|  
|DBPROP_CANSRCOLLBACKWARDS|-|-|-|  
|DBPROP_CANHOLDROWS|F|-|F|  
|DBPROP_LITERALBOOKMARKS|F|-|F|  
|DBPROP_OTHERINSERT|T|F|T|  
|DBPROP_OTHERUPDATEDELETE|T|T|T|  
|DBPROP_OWNINSERT|T|T|T|  
|DBPROP_OWNUPDATEDELETE|T|T|T|  
|DBPROP_QUICKSTART|-|-|-|  
|DBPROP_REMOVEDELETED|T|-|T|  
|DBPROP_IrowsetResynch|-|-|-|  
|DBPROP_CHANGEINSERTEDROWS|F|-|F|  
|DBPROP_SERVERDATAONINSERT|F|-|F|  
|DBPROP_UNIQUEROWS|F|F|F|  
|DBPROP_IMMOBILEROWS|F|T|F|  
  
 Выбор модели курсора для конкретного ряда свойств набора строк определяется следующим образом.  
  
 Получите подмножество свойств, приведенных в предыдущих таблицах, из указанной коллекции свойств набора строк. В зависимости от значения флага для каждого свойства набора строк разделите эти свойства на две подгруппы — обязательные (T, F) и необязательные (-). Для каждой модели курсора начните с первой таблицы и перемещайтесь слева направо. Сравните значения свойств в этих двух подгруппах со значениями соответствующих свойств в этом столбце. Выбирается та модель курсора, для которой все обязательные свойства совпадают, а число несовпадений в необязательных свойствах минимально. При наличии нескольких моделей курсора выбирается самая левая.  
  
## <a name="sql-server-cursor-block-size"></a>Размер блока курсора SQL Server  
 Если курсор [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает набор строк драйвера OLE DB для SQL Server, то размер блока курсора определяется числом элементов в параметре массива дескрипторов строк метода **IRowset::GetNextRows** или **IRowsetLocate::GetRowsAt**. Строки, указанные дескрипторами в массиве, являются элементами блока курсора.  
  
 Что касается наборов строк, поддерживающих закладки, то элементы блока курсора определяются дескрипторами строк, которые извлекаются с помощью метода **IRowsetLocate::GetRowsByBookmark**.  
  
 Независимо от того, какой метод использовался для заполнения набора строк и формирования блока курсора [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], этот блок курсора остается активным до выполнения следующего метода извлечения строк применительно к этому набору строк.  
  
## <a name="see-also"></a>См. также:  
 [Наборы строк](../../oledb/ole-db-rowsets/rowsets.md)  
  
  
