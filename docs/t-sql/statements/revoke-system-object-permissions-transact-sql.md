---
description: REVOKE, отмена разрешения на системные объекты (Transact-SQL)
title: REVOKE, отмена разрешений на системные объекты (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, system objects
- permissions [SQL Server], system objects
ms.assetid: 84983238-dd7d-45bd-99bb-52c9d8e96a87
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c7485325af023c264193fcc8ce8a449553dcb0b9
ms.sourcegitcommit: b93beb4f03aee2c1971909cb1d15f79cd479a35c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91498048"
---
# <a name="revoke-system-object-permissions-transact-sql"></a>REVOKE, отмена разрешения на системные объекты (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отменяет для участника разрешения на системные объекты (например, хранимые процедуры, расширенные хранимые процедуры, функции и представления).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
REVOKE { SELECT | EXECUTE } ON [sys.]system_object FROM principal   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 [**sys.** ] .  
 Квалификатор **sys** требуется только тогда, когда имеется ссылка на представления каталога и динамические административные представления.  
  
 *system_object*  
 Задает объект, для которого отменяется разрешение.  
  
 *principal*  
 Задает участника, у которого отменяется разрешение.  
  
## <a name="remarks"></a>Комментарии  
 Эта инструкция предназначена для отмены разрешений на хранимые процедуры, расширенные хранимые процедуры, функции с табличным значением и скалярные функции, представления, представления каталога, представления совместимости, представления INFORMATION_SCHEMA, динамические административные представления и системные таблицы, которые устанавливаются в составе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Каждый из этих системных объектов существует в виде уникальной записи в базе данных ресурсов (**mssqlsystemresource**). Она доступна только для чтения. Ссылка на объект представлена в виде записи в схеме **sys** каждой базы данных.  
  
 Разрешение имен по умолчанию устраняет проблему неправомочных имен процедур в базе данных ресурсов. Таким образом, квалификатор **sys.** требуется только при указании представлений каталога или динамических административных представлений.  
  
> [!CAUTION]  
>  Вызов разрешений в системных объектах вызовет ошибки в зависящих от них приложениях. В [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] используются представления каталогов, и нормальная работа может быть нарушена, если изменить разрешения по умолчанию для представлений каталогов.  
  
 Отмена разрешений на триггеры и столбцы системных объектов не поддерживается.  
  
 При обновлении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешения на доступ к системным объектам сохраняются.  
  
 Системные объекты отображаются в представлении каталога [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CONTROL SERVER.  
  
## <a name="examples"></a>Примеры  
 В следующем примере производится отмена разрешения `EXECUTE` на процедуру `sp_addlinkedserver` у роли `public`.  
  
```sql  
REVOKE EXECUTE ON sys.sp_addlinkedserver FROM public;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [sys.system_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [GRANT, предоставление разрешения на системный объект (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)   
 [DENY, запрет разрешений на системный объект (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)  
  
  
