---
description: nchar и nvarchar (Transact-SQL)
title: Типы nchar и nvarchar (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nvarchar data type
- nchar data type
ms.assetid: 81ee5637-ee31-4c4d-96d0-56c26a742354
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 11205430c39bf127d7875e820158ea9bfe7cfbe2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747764"
---
# <a name="nchar-and-nvarchar-transact-sql"></a>nchar и nvarchar (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Символьные типы данных имеют фиксированный (**nchar**) или переменный (**nvarchar**) размер. Начиная с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] при использовании параметров сортировки с поддержкой [дополнительных символов](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) эти типы данных хранят весь диапазон символьных данных [Юникод](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) и используют кодировку [UTF-16](https://www.wikipedia.org/wiki/UTF-16). Если указаны параметры сортировки без поддержки дополнительных символов, эти типы данных хранят только подмножество символьных данных, поддерживаемых кодировкой [UCS-2](https://www.wikipedia.org/wiki/Universal_Coded_Character_Set#Encoding_forms).

## <a name="arguments"></a>Аргументы
**nchar** [ ( n ) ]  
Строковые данные фиксированного размера. *n* определяет размер строки в парах байтов и должно иметь значение от 1 до 4000. Размер хранилища — дважды *n* байт. В случае с кодировкой [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF) размер при хранении определяется как дважды *n* байт, а количество хранимых символов равно *n*. Для кодировки UTF-16 размер при хранении также равен дважды *n* байт, но количество хранимых символов может быть меньше *n*, так как дополнительные символы используют две пары байтов (также называются [суррогатными парами](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Синонимами типа **nchar** по стандарту ISO являются типы **national char** и **national character**.
  
**nvarchar** [ ( n | **max** ) ]  
Строковые данные переменного размера. *n* определяет размер строки в парах байтов и может иметь значение от 1 до 4000. Значение **max** указывает, что максимальный размер при хранении составляет 2^30-1 символов (2 ГБ). Размер при хранении определяется как дважды *n* байт + 2 байта. В случае с кодировкой [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF) размер при хранении определяется как дважды *n* байт + 2 байта, а количество хранимых символов равно *n*. Для кодировки UTF-16 размер при хранении также равен дважды *n* байт + 2 байта, но количество хранимых символов может быть меньше *n*, так как дополнительные символы используют две пары байтов (также называются [суррогатными парами](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Синонимами типа **nvarchar** по стандарту ISO являются типы **national char varying** и **national character varying**.
  
## <a name="remarks"></a>Remarks  
Часто ошибочно считают, что в типах данных [NCHAR(*n*) и NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) число *n* указывает на количество символов. Однако на самом деле число *n* в [NCHAR(*n*) и NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) — это длина строки в **парах байтов** (0–4000). *n* никогда не определяет количество хранимых символов. То же самое верно и в отношении типов [CHAR(*n*) и VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md).   
Заблуждение возникает из-за того, что при использовании символов, определенных в диапазоне Юникода 0–65 535, на каждую пару байтов приходится один хранимый символ. Однако в старших диапазонах Юникода (65 536–1 114 111) один символ может занимать две пары байтов. Например, в столбце, определенном как NCHAR(10), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] может хранить 10 символов, занимающих одну пару байтов (диапазон Юникода 0–65 535), но меньше 10 символов, занимающих две пары байтов (диапазон Юникода 65 536–1 114 111). Дополнительные сведения о хранении символов Юникода и их диапазонах см. в разделе [Различия в хранении UTF-8 и UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).     

Если значение *n* в определении данных или в инструкции объявления переменной не указано, то длина по умолчанию равна 1. Когда *n* не задано функцией CAST, длина по умолчанию равняется 30.

Если вы используете **nchar** или **nvarchar**, мы рекомендуем:
- использовать **nchar**, если размеры записей данных в столбцах одинаковые;  
- использовать **nvarchar**, если размеры записей данных в столбцах существенно отличаются;  
- использовать **nvarchar(max)**, если размеры записей данных в столбцах существенно отличаются и длина строки может превышать 4000 пар байтов.  
  
Тип **sysname** — это предоставляемый системой определяемый пользователем тип данных, который функционально эквивалентен типу **nvarchar(128)** за исключением того, что он не допускает значения NULL. Тип **sysname** используется для ссылки на имена объектов баз данных.
  
Объектам, в которых используются типы данных **nchar** и **nvarchar**, назначаются параметры сортировки базы данных по умолчанию, если только иные параметры сортировки не назначены с помощью предложения COLLATE.
  
Для типов данных **nchar** и **nvarchar** параметр SET ANSI_PADDING всегда принимает значение ON. Параметр SET ANSI_PADDING OFF не применяется к типам данных **nchar** или **nvarchar**.
  
Префикс N в строковых константах с символами Юникода указывает на входные данные в кодировке UCS-2 или UTF-16 (в зависимости от того, используются ли параметры сортировки с поддержкой дополнительных символов). Без префикса N строка преобразуется в стандартную кодовую страницу базы данных, и определенные символы могут не распознаваться. Начиная с [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] при использовании параметров сортировки с поддержкой UTF-8 стандартная кодовая страница может хранить символы Юникода в кодировке UTF-8. 
 
> [!NOTE]  
> Когда строковая константа имеет префикс N и ее длина не превышает максимальную длину строкового типа данных nvarchar (4000), результатом неявного преобразования будет строка в кодировке UCS-2 или UTF-16. В противном случае результатом неявного преобразования будет большое значение nvarchar(max).
  
> [!WARNING]  
> Каждому ненулевому столбцу **varchar(max)** и **nvarchar(max)** необходимо дополнительно выделить 24 байта памяти, которые учитываются в максимальном размере строки в 8060 байт во время операции сортировки. Эти дополнительные байты могут неявно ограничивать число ненулевых столбцов **varchar(max)** или **nvarchar(max)** в таблице. При создании таблицы или во время вставки данных не возникает особых ошибок (кроме обычного предупреждения о том, что максимальный размер строки превышает максимально допустимое значение в 8060 байт). Такой большой размер строки может приводить к ошибкам (например, ошибке 512), которые пользователи не ожидают во время обычных операций.  Примерами операций могут служить обновление ключа кластеризованного индекса или сортировка полного набора столбцов.
  
## <a name="converting-character-data"></a>Преобразование в символьные данные  
Сведения о преобразовании символьных данных см. в статье [char и varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md).
  
## <a name="see-also"></a>См. также раздел
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[COLLATE (Transact-SQL)](../statements/collations.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)  
[SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)    
[Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)     
[Однобайтовые и многобайтовые кодировки](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
