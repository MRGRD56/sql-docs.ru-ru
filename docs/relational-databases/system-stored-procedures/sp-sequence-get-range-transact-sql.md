---
description: sp_sequence_get_range (Transact-SQL)
title: sp_sequence_get_range (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sequence_get_range
- sp_sequence_get_range_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sp_sequence_get_range procedure
- sp_sequence_get_range
ms.assetid: 8ca6b0c6-8d9c-4eee-b02f-51ddffab4492
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 23ca83a62a194bb30bbf7a9b1432e42525592f3c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744644"
---
# <a name="sp_sequence_get_range-transact-sql"></a>sp_sequence_get_range (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Возвращает диапазон значений последовательности из объекта последовательности. Объект последовательности создает и выдает запрошенное количество значений, а также предоставляет приложению метаданные, связанные с диапазоном.  
  
 Дополнительные сведения о порядковых номерах см. в разделе [порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_sequence_get_range [ @sequence_name = ] N'<sequence>'   
     , [ @range_size = ] range_size  
     , [ @range_first_value = ] range_first_value OUTPUT   
    [, [ @range_last_value = ] range_last_value OUTPUT ]  
    [, [ @range_cycle_count = ] range_cycle_count OUTPUT ]  
    [, [ @sequence_increment = ] sequence_increment OUTPUT ]  
    [, [ @sequence_min_value = ] sequence_min_value OUTPUT ]  
    [, [ @sequence_max_value = ] sequence_max_value OUTPUT ]  
    [ ; ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @sequence_name = ] N'sequence'` Имя объекта последовательности. Схема является необязательной. *sequence_name* имеет тип **nvarchar (776)**.  
  
`[ @range_size = ] range_size` Число значений для выборки из последовательности. **\@ range_size** имеет тип **bigint**.  
  
`[ @range_first_value = ] range_first_value` Параметр OUTPUT возвращает первое (минимальное или максимальное) значение объекта последовательности, используемого для вычисления запрошенного диапазона. **\@ range_first_value** **sql_variant** с тем же базовым типом, что и у объекта последовательности, используемого в запросе.  
  
`[ @range_last_value = ] range_last_value` Необязательный выходной параметр возвращает последнее значение запрошенного диапазона. **\@ range_last_value** **sql_variant** с тем же базовым типом, что и у объекта последовательности, используемого в запросе.  
  
`[ @range_cycle_count = ] range_cycle_count` Необязательный выходной параметр возвращает количество раз, когда объект последовательности был циклическим для возврата запрошенного диапазона. **\@ range_cycle_count** имеет **тип int**.  
  
`[ @sequence_increment = ] sequence_increment` Необязательный выходной параметр возвращает приращение объекта последовательности, используемого для вычисления запрошенного диапазона. **\@ sequence_increment** **sql_variant** с тем же базовым типом, что и у объекта последовательности, используемого в запросе.  
  
`[ @sequence_min_value = ] sequence_min_value` Необязательный выходной параметр возвращает минимальное значение объекта последовательности. **\@ sequence_min_value** **sql_variant** с тем же базовым типом, что и у объекта последовательности, используемого в запросе.  
  
`[ @sequence_max_value = ] sequence_max_value` Необязательный выходной параметр возвращает максимальное значение объекта последовательности. **\@ sequence_max_value** **sql_variant** с тем же базовым типом, что и у объекта последовательности, используемого в запросе.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Remarks  
 sp_sequence_get_rangeis в представлении sys. на схему и можно ссылаться как на sys.sp_sequence_get_range.  
  
### <a name="cycling-sequences"></a>Циклические последовательности  
 При необходимости объект последовательности может выполняться циклически требуемое число раз для обработки запрошенного диапазона. Количество циклов возвращается вызывающему методу через параметр `@range_cycle_count`.  
  
> [!NOTE]  
>  При циклическом выполнении объект последовательности перезапускается с минимального значения в возрастающей последовательности и с максимального в убывающей, а не с начального значения объекта последовательности.  
  
### <a name="non-cycling-sequences"></a>Нециклические последовательности  
 Если количество значений в запрашиваемом диапазоне больше остающегося количества доступных значений в объекте последовательности, то запрашиваемый диапазон не вычитается из объекта последовательности и возвращается следующее сообщение об ошибке 11732:  
  
 `The requested range for sequence object '%.*ls' exceeds the maximum or minimum limit. Retry with a smaller range.`  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение UPDATE для объекта последовательности или его схемы.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах используется объект последовательности с именем Test. Ранжесек. Используйте следующую инструкцию, чтобы создать последовательность Test. Ранжесек.  
  
```  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.RangeSeq  
    AS int   
    START WITH 1  
    INCREMENT BY 1  
    MINVALUE 1  
    MAXVALUE 25  
    CYCLE  
    CACHE 10  
;  
```  
  
### <a name="a-retrieving-a-range-of-sequence-values"></a>A. Получение диапазона значений последовательности  
 Следующая инструкция получает четыре порядковых номера из объекта последовательности Test. Ранжесек и возвращает первый из чисел пользователю.  
  
```  
DECLARE @range_first_value_output sql_variant ;  
  
EXEC sys.sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 4  
, @range_first_value = @range_first_value_output OUTPUT ;  
  
SELECT @range_first_value_output AS FirstNumber ;  
  
```  
  
### <a name="b-returning-all-output-parameters"></a>Б. Возврат всех выходных параметров  
 В следующем примере возвращаются все выходные значения процедуры sp_sequence_get_range.  
  
```  
DECLARE    
  @FirstSeqNum sql_variant  
, @LastSeqNum sql_variant  
, @CycleCount int  
, @SeqIncr sql_variant  
, @SeqMinVal sql_variant  
, @SeqMaxVal sql_variant ;  
  
EXEC sys.sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 5  
, @range_first_value = @FirstSeqNum OUTPUT   
, @range_last_value = @LastSeqNum OUTPUT   
, @range_cycle_count = @CycleCount OUTPUT  
, @sequence_increment = @SeqIncr OUTPUT  
, @sequence_min_value = @SeqMinVal OUTPUT  
, @sequence_max_value = @SeqMaxVal OUTPUT ;  
  
-- The following statement returns the output values  
SELECT  
  @FirstSeqNum AS FirstVal  
, @LastSeqNum AS LastVal  
, @CycleCount AS CycleCount  
, @SeqIncr AS SeqIncrement  
, @SeqMinVal AS MinSeq  
, @SeqMaxVal AS MaxSeq ;  
  
```  
  
 Если увеличить значение аргумента `@range_size`, например, до 75, то объект последовательности будет выполняться циклически. Проверьте по аргументу `@range_cycle_count`, выполнялся ли объект циклически и, если это так, сколько раз.  
  
### <a name="c-example-using-adonet"></a>В. Пример использования ADO.NET  
 В следующем примере извлекается диапазон из теста Test. Ранжесек с помощью ADO.NET.  
  
```  
SqlCommand cmd = new SqlCommand();  
cmd.Connection = conn;  
cmd.CommandType = CommandType.StoredProcedure;  
cmd.CommandText = "sys.sp_sequence_get_range";  
cmd.Parameters.AddWithValue("@sequence_name", "Test.RangeSeq");  
cmd.Parameters.AddWithValue("@range_size", 10);  
  
// Specify an output parameter to retrieve the first value of the generated range.  
SqlParameter firstValueInRange = new SqlParameter("@range_first_value", SqlDbType.Variant);  
firstValueInRange.Direction = ParameterDirection.Output;  
cmd.Parameters.Add(firstValueInRange);  
  
conn.Open();  
cmd.ExecuteNonQuery();  
  
// Output the first value of the generated range.  
Console.WriteLine(firstValueInRange.Value);  
  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
