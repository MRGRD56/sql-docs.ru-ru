---
description: Динамические свойства ADO
title: Динамические свойства ADO | Документация Майкрософт
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO]
- properties [ADO], dynamic
ms.assetid: d7b06d72-f792-4328-93a2-5006b9e2c581
author: rothja
ms.author: jroth
ms.openlocfilehash: f815c82b16ef31bc64e5524ec7114779b2ba3234
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161661"
---
# <a name="ado-dynamic-properties"></a>Динамические свойства ADO
Динамические свойства могут быть добавлены к коллекциям [свойств](./properties-collection-ado.md) [соединения](./connection-object-ado.md), [команды](./command-object-ado.md)или объектов [набора записей](./recordset-object-ado.md) . Источником этих свойств является либо поставщик данных, например [поставщик OLE DB для SQL Server](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), либо поставщик услуг, например [Служба курсора Майкрософт для OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md). Дополнительные сведения о конкретном динамическом свойстве см. в документации соответствующего поставщика данных или поставщика услуг.  
  
 [Индекс динамического свойства ADO](./ado-dynamic-property-index.md) обеспечивает перекрестную ссылку между именами ADO и OLE DB для каждого динамического свойства стандартного поставщика OLE DB.  
  
 Следующие динамические свойства особенно интересны, они также описаны в источниках, упомянутых ранее. Специальные функции ADO описаны в подразделах справки ADO в следующем списке.  
  
|Динамическое свойство|Описание|  
|-|-|  
|[Optimize](./optimize-property-dynamic-ado.md) (Оптимизация)|Указывает, следует ли создавать индекс для этого поля.|  
|[Командная строка](./prompt-property-dynamic-ado.md).|Указывает, должен ли поставщик OLE DB запрашивать сведения об инициализации у пользователя.|  
|[Изменить имя формы](./reshape-name-property-dynamic-ado.md)|Задает имя объекта **набора записей** .|  
|[Команда повторной синхронизации](./resync-command-property-dynamic-ado.md)|Указывает указанную пользователем строку команды, которая выдается методом повторной **синхронизации** для обновления данных в таблице, указанной в динамическом свойстве **уникальной таблицы** .|  
|[Уникальная таблица, уникальная схема, уникальный каталог](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|**Уникальная таблица** Указывает имя базовой таблицы, в которой разрешены обновления, вставки и удаления.<br /><br /> **Уникальная схема** Указывает схему или имя владельца таблицы.<br /><br /> **Уникальный каталог** Указывает каталог или имя базы данных, содержащей таблицу.|  
|[Повторная синхронизация обновлений](./update-resync-property-dynamic-ado.md)|Указывает, следует ли за методом **UpdateBatch** выполнить неявную операцию повторной **синхронизации** , и если да, то область действия этой операции.|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>См. также:  
 [Справочник по API ADO](./ado-api-reference.md)   
 [Коллекции ADO](./ado-collections.md)   
 [Перечислимые константы ADO](./ado-enumerated-constants.md)   
 [Приложение б. ошибки ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [События ADO](./ado-events.md)   
 [Методы ADO](./ado-methods.md)   
 [Объектная модель ADO](./ado-object-model.md)   
 [Объекты и интерфейсы ADO](./ado-objects-and-interfaces.md)   
 [Свойства ADO](./ado-properties.md)