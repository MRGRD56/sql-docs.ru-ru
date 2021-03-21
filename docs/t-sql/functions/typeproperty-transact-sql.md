---
description: TYPEPROPERTY (Transact-SQL)
title: TYPEPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TYPEPROPERTY
- TYPEPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], data types
- data types [SQL Server], status information
- TYPEPROPERTY function
ms.assetid: bc311c80-bac5-46ab-a5c8-68b1c6bbf24a
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7caea50cb3235c8995b60ebb73206ed6db7d5361
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750894"
---
# <a name="typeproperty-transact-sql"></a>TYPEPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о типе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
TYPEPROPERTY (type , property)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *type*  
 Имя типа данных.  
  
 *property*  
 Тип возвращаемых сведений по этому типу данных. Аргумент *property* может иметь одно из перечисленных ниже значений.  
  
|Свойство|Описание|Возвращенное значение|  
|--------------|-----------------|--------------------|  
|**AllowsNull**|Тип данных допускает значения NULL.|1 = истина<br /><br /> 0 = ложь<br /><br /> NULL = не удалось найти тип данных.|  
|**OwnerId**|Владелец типа.<br /><br /> Примечание. Владелец схемы может не быть владельцем типа.|Не равен NULL = идентификатор пользователя базы данных владельца типа.<br /><br /> NULL = неподдерживаемый тип или идентификатор типа недопустим.|  
|**Точность**|Точность типа данных.|Число цифр или символов.<br /><br /> –1 = **xml** или тип данных с большим значением<br /><br /> NULL = не удалось найти тип данных.|  
|**Масштаб**|Масштаб типа данных.|Число символов после запятой для типа данных.<br /><br /> NULL = тип данных не **числовой**, или не удалось найти тип данных.|  
|**UsesAnsiTrim**|При создании типа данных параметр дополнения символами ANSI был установлен в состояние ON.|1 = истина<br /><br /> 0 = ложь<br /><br /> NULL = тип данных не обнаружен или не принадлежит к двоичному или строковому типу данных.|  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="exceptions"></a>Исключения  
 Возвращает значение NULL в случае ошибки или если участник не имеет разрешений для просмотра объекта.  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые ему были предоставлены разрешения. Это означает, что встроенные функции, создающие метаданные, такие как TYPEPROPERTY, могут вернуть значение NULL в случае, если пользователь не имеет разрешений на объект. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-identifying-the-owner-of-a-data-type"></a>A. Определение владельца типа данных  
 Следующий пример возвращает владельца типа данных.  
  
```sql
SELECT TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId') AS owner_id, name, system_type_id, user_type_id, schema_id  
FROM sys.types;  
```  
  
### <a name="b-returning-the-precision-of-the-tinyint-data-type"></a>Б. Получение точности типа данных tinyint  
 В следующем примере возвращается точность или число цифр для типа данных `tinyint`.  
  
```sql
SELECT TYPEPROPERTY( 'tinyint', 'PRECISION');  
```  
  
## <a name="see-also"></a>См. также  
 [TYPE_ID (Transact-SQL)](../../t-sql/functions/type-id-transact-sql.md)   
 [TYPE_NAME (Transact-SQL)](../../t-sql/functions/type-name-transact-sql.md)   
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
  

