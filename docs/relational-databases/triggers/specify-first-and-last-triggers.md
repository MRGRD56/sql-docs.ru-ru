---
description: Указание первого и последнего триггеров
title: Указание первого и последнего триггеров | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- first triggers [SQL Server]
- last triggers
- DML triggers, first or last triggers
- INSTEAD OF triggers
- AFTER triggers
ms.assetid: 9e6c7684-3dd3-46bb-b7be-523b33fae4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 98269d555a7cd639589544dbf8c645eb08ed5cb7
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88427356"
---
# <a name="specify-first-and-last-triggers"></a>Указание первого и последнего триггеров
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Возможно указать, что один из триггеров AFTER, связанных с таблицей, является либо первым триггером AFTER, либо последним триггером AFTER, срабатывающим для каждого запускающего действия INSERT, DELETE и UPDATE. Порядок запуска триггеров AFTER, срабатывающих в промежутке между первым и последним триггерами, не определен.  
  
 Задать порядок срабатывания для триггера AFTER можно с помощью хранимой процедуры **sp_settriggerorder** . Хранимая процедура **sp_settriggerorder** имеет следующие параметры.  
  
|Параметр|Описание|  
|------------|-----------------|  
|**Первая**|Указывает, что триггер DML является первым триггером AFTER, срабатывающим для запускающего действия.|  
|**Последняя**|Указывает, что триггер DML является последним триггером AFTER, срабатывающим для запускающего действия.|  
|**None**|Указывает, что не существует определенного порядка, в соответствии с которым триггер DML должен срабатывать. Используется главным образом для смещения триггера с позиции первого или последнего.|  
  
 В нижеследующем примере показано использование **sp_settriggerorder**:  
  
```  
sp_settriggerorder @triggername = 'MyTrigger', @order = 'first', @stmttype = 'UPDATE'  
```  
  
> [!IMPORTANT]  
>  Первый и последний триггеры должны быть двумя различными триггерами DML.  
  
 Для таблицы могут быть в одно и то же время определены триггеры INSERT, UPDATE и DELETE. Каждый тип инструкции может иметь свои собственные первый и последний триггеры, но они не могут быть одними и теми же триггерами.  
  
 Если первый или последний триггер, определенный для таблицы, не охватывает запускающее действие, то есть если он не охватывает FOR UPDATE, FOR DELETE или FOR INSERT, не существует первого или последнего триггера для отсутствующих действий.  
  
 Триггеры INSTEAD OF не могут быть указаны в качестве первого или последнего триггеров. Триггеры INSTEAD OF срабатывают до выполнения обновлений в базовых таблицах. Если обновления в базовых таблицах выполняются триггером INSTEAD OF, то эти обновления происходят прежде, чем срабатывают какие-либо триггеры AFTER, определенные для таблицы. Например, если триггер INSTEAD OF INSERT по представлению вставляет данные в базовую таблицу, а сама базовая таблица содержит триггер INSTEAD OF INSERT и три триггера AFTER INSERT, то триггер INSTEAD OF INSERT по базовой таблице срабатывает вместо действия вставки, а триггеры AFTER по базовой таблице срабатывают после выполнения любого действия вставки в базовой таблице. Дополнительные сведения см. в разделе [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
 Если инструкция ALTER TRIGGER изменяет первый или последний триггер, атрибут **First** или **Last** удаляется и в качестве значения порядка указывается **None**. Порядок должен быть изменен с использованием свойства **sp_settriggerorder**.  
  
 Функция OBJECTPROPERTY сообщает, является ли триггер первым или последним, используя следующие свойства: **ExecIsFirstInsertTrigger**, **ExecIsFirstUpdateTrigger**, **ExecIsFirstDeleteTrigger**, **ExecIsLastInsertTrigger**, **ExecIsLastUpdateTrigger** и **ExecIsLastDeleteTrigger**.  
  
 Репликация автоматически создает первый триггер для любой таблицы, включенной в подписку немедленным обновлением или обновлением с постановкой в очередь. Репликация требует, чтобы ее триггер был первым. При попытке вставить таблицу с указанным первым триггером в немедленно обновляемую подписку или подписку, обновляемую посредством очередей, репликация инициирует ошибку. При попытке сделать триггер первым триггером после включения таблицы в подписку **sp_settriggerorder** возвращает ошибку. В случае использования ALTER для триггера репликации или использования **sp_settriggerorder** , чтобы сделать триггер репликации последним или триггером вне порядка, подписка не будет функционировать должным образом.  
  
## <a name="see-also"></a>См. также:  
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_settriggerorder (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-settriggerorder-transact-sql.md)  
  
  
