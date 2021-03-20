---
description: SET DATEFORMAT (Transact-SQL)
title: SET DATEFORMAT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEFORMAT
- SET DATEFORMAT
- SET_DATEFORMAT_TSQL
- DATEFORMAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], formats
- dates [SQL Server], ordering date parts
- SET DATEFORMAT option [SQL Server]
- DATEFORMAT option [SQL Server]
- date and time [SQL Server], SET DATEFORMAT
- options [SQL Server], date
- date and time [SQL Server], DATEFORMAT
- dateparts [SQL Server], dateformat
ms.assetid: da217878-7ec4-477e-aa13-604073c948f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3ec299dd8d8daee6ab9711c86bc2d4f54b7f11e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751484"
---
# <a name="set-dateformat-transact-sql"></a>SET DATEFORMAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Задает порядок элементов даты (месяц, день, год) при интерпретации символьных строк дат. Эти строки имеют тип **date**, **smalldatetime**, **datetime**, **datetime2** или **datetimeoffset**.  
  
 Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SET DATEFORMAT { format | @format_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *format* |  **@** _format_var_  
 Порядок следования составляющих частей даты. Допустимые параметры: **mdy**, **dmy**, **ymd**, **ydm**, **myd** и **dym**. Может быть задано в формате Юникод или в виде двухбайтовой кодировки (DBCS), преобразованной в Юникод. Для языкового стандарта "Английский (США)" значение по умолчанию равно **mdy**. Значения параметра DATEFORMAT по умолчанию для всех поддерживаемых языков см. в разделе [sp_helplanguage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
 Значение **ydm** параметра DATEFORMAT не поддерживается для типов данных **date**, **datetime2** и **datetimeoffset**.  
  
 Параметр DATEFORMAT может интерпретировать символьные строки по-разному для разных типов данных даты в зависимости от их формата строк. Например, интерпретации **datetime** и **smalldatetime** могут не соответствовать **date**, **datetime2** или **datetimeoffset**. Этот параметр влияет на интерпретацию символьных строк при их преобразовании в значения даты для базы данных. Он не влияет на отображение значений типов данных даты, хранящихся в базе данных, а также на формат их хранения.  
  
 Некоторые форматы символьных строк, например ISO 8601, интерпретируются независимо от параметра DATEFORMAT.  
  
 Установка SET DATEFORMAT происходит во время выполнения, а не во время синтаксического анализа.  
  
 SET DATEFORMAT имеет преимущество над неявной установкой формата даты [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере в качестве входных параметров сеансов используются различные строковые значения дат с одинаковым значением параметра `DATEFORMAT`.  
  
```sql
-- Set date format to day/month/year.  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '31/12/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: 2008-12-31 09:01:01.123  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '12/31/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: Msg 241: Conversion failed when converting date and/or time -- from character string.  
  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  

