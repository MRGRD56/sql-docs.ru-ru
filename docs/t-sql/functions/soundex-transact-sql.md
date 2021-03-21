---
description: SOUNDEX (Transact-SQL)
title: SOUNDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOUNDEX
- SOUNDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SOUNDEX function
- comparing string data
- strings [SQL Server], similarity
- strings [SQL Server], comparing
- SOUNDEX values
ms.assetid: 8f1ed34e-8467-4512-a211-e0f43dee6584
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5165b060e54306842e68442b394463ccb944920
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748884"
---
# <a name="soundex-transact-sql"></a>SOUNDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает четырехсимвольный код (SOUNDEX) для оценки степени сходства двух строк.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SOUNDEX ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *character_expression*  
 Буквенно-цифровое [выражение](../../t-sql/language-elements/expressions-transact-sql.md) символьных данных. *character_expression* может быть константой, переменной или столбцом.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **varchar**  
  
## <a name="remarks"></a>Remarks  
 Функция SOUNDEX преобразует алфавитно-цифровую строку в 4-символьный код, чье значение зависит от способа звучания строки при произношении на английском языке. Первый символ кода является первым символом *character_expression*, преобразованным в верхний регистр. Второй, третий и четвертый символы кода являются числами, которые обозначают буквы в выражении. Буквы A, E, I, O, U, H, W и Y игнорируются, если только не являются первой буквой строки. Нули добавляются в конце при необходимости производить четырехсимвольный код. Дополнительные сведения о коде SOUNDEX см. в статье [Система индексирования Soundex](https://www.archives.gov/research/census/soundex.html).  
  
 Коды SOUNDEX из разных строк можно сравнивать, чтобы узнать, насколько похоже звучат строки при произношении. Функция DIFFERENCE выполняет SOUNDEX по двум строкам и возвращает целое число, представляющее сходство кодов SOUNDEX для этих строк.  
  
 Функция SOUNDEX учитывает параметры сортировки. Строковые функции могут быть вложенными.  
  
## <a name="soundex-compatibility"></a>Совместимость SOUNDEX  
 В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в функции SOUNDEX использовалось подмножество правил SOUNDEX. При уровне совместимости базы данных, равном 110 или выше, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяет более полный набор правил.  
  
 После обновления до уровня совместимости 110 или более высокого, возможно, придется перестроить индексы, кучи или ограничения CHECK, в которых используется функция SOUNDEX.  
  
-   Кучу, которая содержит материализованный вычисляемый столбец, определенный функцией SOUNDEX, нельзя запрашивать, пока она не будет перестроена с помощью инструкции `ALTER TABLE <table> REBUILD`.  
  
-   При повышении уровня совместимости ограничения CHECK, определенные функцией SOUNDEX, отключаются. Чтобы включить ограничение, выполните инструкцию `ALTER TABLE <table> WITH CHECK CHECK CONSTRAINT ALL`.  
  
-   Запросы к индексам (в том числе индексированным представлениям), которые содержат материализованный вычисляемый столбец, определенный функцией SOUNDEX, можно выполнять только после их перестроения с помощью инструкции `ALTER INDEX ALL ON <object> REBUILD`.  
  
## <a name="examples"></a>Примеры  
 Следующий пример демонстрирует использование функции SOUNDEX и связанной с ней функции DIFFERENCE. В первом примере стандартные значения `SOUNDEX` возвращаются для всех согласных. Возвращение функции `SOUNDEX` для `Smith` и `Smythe` выдает одинаковый результат SOUNDEX, поскольку все гласные, буквы `y` и `h`, а также удвоенные буквы не включаются.  
  
```sql
-- Using SOUNDEX  
SELECT SOUNDEX ('Smith'), SOUNDEX ('Smythe');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Допустимо для параметров сортировки Latin1_General.  
  
```  
S530  S530    
```  
  
 Функция `DIFFERENCE` производит сравнение результатов шаблонов `SOUNDEX`. В следующем примере показаны две строки, различающиеся только по гласной. Возвращенное значение `4` означает максимальное сходство.  
  
```sql
-- Using DIFFERENCE  
SELECT DIFFERENCE('Smithers', 'Smythers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Допустимо для параметров сортировки Latin1_General.  
  
```  
4             
```  
  
 В следующем примере строки различаются в согласных, следовательно, возвращенное значение `2` указывает на большую степень различия.  
  
```sql
SELECT DIFFERENCE('Anothers', 'Brothers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Допустимо для параметров сортировки Latin1_General.  
  
```  
2             
```  
  
## <a name="see-also"></a>См. также:  
 [DIFFERENCE (Transact-SQL)](../../t-sql/functions/difference-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [Уровень совместимости инструкции ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
  

