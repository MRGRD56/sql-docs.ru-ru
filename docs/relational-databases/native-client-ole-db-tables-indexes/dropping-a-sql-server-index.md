---
description: Drop SQL Server индекс (поставщик собственного клиента OLE DB)
title: Drop SQL Server индекс (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- removing indexes
- deleting indexes
- DropIndex function
- dropping indexes
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
ms.assetid: add3ba14-10b1-4723-b7c0-3e83689e9fdd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26c14af7fd452b93df93ef5c59b777b4057e3279
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748644"
---
# <a name="dropping-a-sql-server-native-client-index"></a>Удаление SQL Server Native Client индекса

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DBа собственного клиента предоставляет функцию **IIndexDefinition::D ропиндекс** . Это позволяет потребителям удалять индексы из таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента предоставляет некоторые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ограничения первичного ключа и уникальности в качестве индексов. Владелец таблицы, владелец базы данных и члены некоторых административных ролей могут изменять таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], удаляя ограничения. По умолчанию только владелец таблицы может удалять существующие индексы. Таким образом, будет ли функция **DropIndex** выполнена успешно или с ошибкой, зависит не только от прав доступа пользователя приложения, но также и от указанного типа индекса.  
  
 Пользователь задает имя таблицы в виде символьной строки в Юникоде в элементе *pwszName* объединения *uName* в параметре *pTableID*. Элемент *eKind* параметра *pTableID* должен быть равен DBKIND_NAME.  
  
 Потребитель задает имя индекса в виде строки в Юникоде в элементе *pwszName* объединения *uName* в параметре *pIndexID*. Элемент *eKind* параметра *pIndexID* должен быть равен DBKIND_NAME. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента не поддерживает функцию OLE DB удаления всех индексов в таблице, если *pIndexID* имеет значение null. Если значение параметра *pIndexID* равно NULL, то возвращается E_INVALIDARG.  
  
## <a name="see-also"></a>См. также:  
 [Таблицы и индексы](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
