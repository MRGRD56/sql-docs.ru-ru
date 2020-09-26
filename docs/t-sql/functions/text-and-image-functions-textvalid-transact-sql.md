---
description: Функции для работы с изображениями и текстом — TEXTVALID (Transact-SQL)
title: TEXTVALID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TEXTVALID_TSQL
- TEXTVALID
dev_langs:
- TSQL
helpviewer_keywords:
- invalid text pointers [SQL Server]
- valid text pointers [SQL Server]
- TEXTVALID function
- checking text pointers
- text-pointer values
- verifying text pointers
ms.assetid: 9411c349-b59b-4740-a270-92f91d81ad23
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 244865ca12b85076ef00d9187dc1ea8f01e9aa06
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380499"
---
# <a name="text-and-image-functions---textvalid-transact-sql"></a>Функции для работы с изображениями и текстом — TEXTVALID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Функция типа **text**, **ntext** или **image**, которая проверяет, является ли указанный текстовый указатель действительным.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Дополнительные возможности недоступны.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
TEXTVALID ( 'table.column' ,text_ ptr )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *table*  
 Имя таблицы, которая будет использоваться.  
  
 *column*  
 Имя столбца, который будет использоваться.  
  
 *text_ptr*  
 Текстовый указатель, который подлежит проверке.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="remarks"></a>Remarks  
 Возвращает 1, если указатель является действительным, и 0, если указатель недействителен. Обратите внимание на то, что идентификатор для столбца **text** должен включать имя таблицы. Нельзя использовать инструкции UPDATETEXT, WRITETEXT или READTEXT без действительных текстовых указателей.  
  
 Приведенные ниже функции и инструкции также будут полезны при работе с данными типов **text**, **ntext** и **image**.  
  
|Функция или инструкция|Описание|  
|---------------------------|-----------------|  
|PATINDEX **(** ' _%pattern%_ ' **,** _expression_ **)**|Возвращает позицию символа указанной символьной строки в столбцах **text** и **ntext**.|  
|DATALENGTH **(** _expression_ **)**|Возвращает длину данных в столбцах **text**, **ntext** и **image**.|  
|SET TEXTSIZE|Возвращает предельный размер (в байтах) для данных типа **text**, **ntext** или **image**, возвращаемых инструкцией SELECT.|  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается информация о том, существует ли действительный текстовый указатель для каждого значения в столбце `logo` таблицы `pub_info`.  
  
> [!NOTE]  
>  Чтобы выполнить этот пример, необходимо установить базу данных **pubs**.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, 'Valid (if 1) Text data'   
   = TEXTVALID ('pub_info.logo', TEXTPTR(logo))   
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id Valid (if 1) Text data   
------ ----------------------   
0736   1                        
0877   1                        
1389   1                        
1622   1                        
1756   1                        
9901   1                        
9952   1                        
9999   1                        
  
(8 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также  
 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
 [SET TEXTSIZE (Transact-SQL)](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Функции для работы с изображениями и текстом (Transact-SQL)](https://msdn.microsoft.com/library/b9c70488-1bf5-4068-a003-e548ccbc5199)   
 [TEXTPTR (Transact-SQL)](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)  
  
  
