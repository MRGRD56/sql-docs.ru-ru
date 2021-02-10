---
description: Уникальная таблица, уникальная схема, уникальный Properties-Dynamic каталога (ADO)
title: Управление изменениями в базовой таблице набора записей (ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Unique Table property [ADO]
- Unique Schema property [ADO]
- Unique Catalog property [ADO]
ms.assetid: d0e775d8-e353-46a1-ad10-ed4cc240dfaa
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cd94edb4b1e2c2fba12221a3371592c34eb8c50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056365"
---
# <a name="unique-table-unique-schema-unique-catalog-properties-dynamic-ado"></a>Уникальная таблица, уникальная схема, уникальный Properties-Dynamic каталога (ADO)
Позволяет точно управлять изменениями в определенной базовой таблице в [наборе записей](./recordset-object-ado.md) , сформированном операцией Join над несколькими базовыми таблицами.  
  
-   **Уникальная таблица** указывает имя базовой таблицы, в которой разрешены обновления, вставки и удаления.  
  
-   **Уникальная схема** указывает *схему* или имя владельца таблицы.  
  
-   **Уникальный каталог** указывает *Каталог* или имя базы данных, содержащей таблицу.  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает **строковое** значение, которое является именем таблицы, схемы или каталога.  
  
## <a name="remarks"></a>Remarks  
 Требуемая базовая таблица уникально идентифицируется по именам каталогов, схем и таблиц. Если задано свойство **уникальной таблицы** , то для поиска базовой таблицы используются значения **уникальной схемы** или свойства **уникального каталога** . Он предназначен, но не является обязательным, так как либо **уникальная схема** , либо свойства **уникального каталога** задаются до того, как будет задано свойство **уникальной таблицы** .  
  
 Первичный ключ **уникальной таблицы** рассматривается как первичный ключ всего **набора записей**. Это ключ, используемый для любого метода, для которого необходим первичный ключ.  
  
 Если задана **уникальная таблица** , метод [Delete](./delete-method-ado-recordset.md) влияет только на именованную таблицу. Методы [AddNew](./addnew-method-ado.md), [Resync](./resync-method.md), [Update](./update-method.md)и [UpdateBatch](./updatebatch-method.md) влияют на все соответствующие базовые таблицы **набора записей**.  
  
 Перед выполнением пользовательских повторных синхронизаций необходимо указать **уникальную таблицу** . Если не указана **уникальная таблица** , то свойство [Command](./resync-command-property-dynamic-ado.md) повторной синхронизации не будет действовать.  
  
 Если не удается найти уникальную базовую таблицу, возникает ошибка во время выполнения.  
  
 Эти динамические свойства добавляются к коллекции [свойств](./properties-collection-ado.md) объекта **Recordset** , если свойство [CursorLocation](./cursorlocation-property-ado.md) имеет значение **адусеклиент**.  
  
## <a name="applies-to"></a>Применение  
 [Объект Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>См. также:  
 [Объект Recordset (ADO)](./recordset-object-ado.md)