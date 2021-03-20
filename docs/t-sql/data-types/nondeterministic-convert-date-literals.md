---
description: Недетерминированное преобразование строк дат литералов в значения DATE
title: Недетерминированное преобразование литералов даты | Документация Майкрософт
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 12d0a52315d5865fcda260af787a71cc274a851e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104736694"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>Недетерминированное преобразование строк дат литералов в значения DATE

Будьте осторожны, когда разрешаете преобразование строк CHARACTER в тип данных DATE. Дело в том, что такие преобразования часто бывают _недетерминированными_.

Вы управляете этими недетерминированными преобразованиями, учитывая параметры [SET LANGUAGE](../statements/set-language-transact-sql.md) и [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md).



## <a name="set-language-example-month-name-in-polish"></a>Пример SET LANGUAGE: название месяца на польском языке

- `SET LANGUAGE Polish;`

Строка символов может быть названием месяца. Но это название на английском, польском, хорватском или другом языке? В сеансе пользователя будет настроен соответствующий язык?

Например, рассмотрим слово _listopad_, являющееся названием месяца. Но какой это месяц, зависит от языка, который используется, по мнению системы SQL:
- Если польский, то _listopad_ — это 11-й месяц (_ноябрь_).
- Если хорватский, то _listopad_ — это 10-й месяц (_ноябрь_).

#### <a name="code-example-of-set-language"></a>Пример кода SET LANGUAGE

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
***/
```



## <a name="set-dateformat-example"></a>Пример SET DATEFORMAT

- `SET DATEFORMAT dmy;`

Предыдущий формат **ДМГ** указывает, что пример строки даты "01-03-2018" будет интерпретироваться как _первый день марта 2018 года_.

Если указан формат **МДГ**, то "01-03-2018" означает _третий день января 2018 года_.

А если указан формат **ГМД**, то неизвестно, какими будут выходные данные. Числовое значение "2018" слишком велико, чтобы быть днем.
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>Определенные страны

В Японии и Китае используется формат DATEFORMAT **ГМД**. Части этого формата расположены от большего к меньшему. Поэтому этот формат удобно сортировать. Этот формат считается _международным_ форматом. Дело в том, что четыре цифры года ни с чем не спутаешь, и ни в одной стране не используется устаревший формат **ГДМ**.

В других странах, например Германии и Франции, используется DATEFORMAT **ДМГ**, то есть **дд-мм-гггг**. Формат **ДМГ** сортировать неудобно, но это последовательность от меньшего к большему.

Только в США и Федеративных Штатах Микронезии используется не сортируемый формат **МДГ**. Этот формат совпадает с тем, как в английском произносятся даты.

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>Пример кода SET DATEFORMAT: *МДГ* и *ДМГ*

В следующем примере кода Transact-SQL используется та же строка символов даты с тремя разными параметрами DATEFORMAT. При выполнении кода получается результат, показанный в комментарии:

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
***/
```

В предыдущем примере кода в последнем примере есть несоответствие между форматом **ГМД** и входной строкой. Третий узел входной строки представляет числовое значение, слишком большое для дня. Корпорация Майкрософт не гарантирует выходное значение в таких случаях.

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT предлагает явные коды для _детерминированного_ управления форматами даты

В нашей документации по CAST и CONVERT приводятся явные коды, которые можно использовать с функцией CONVERT для _детерминированного_ управления преобразованиями даты. Каждый месяц эта статья имеет наибольшее количество просмотров.

- [CAST и CONVERT (Transact-SQL): стили даты и времени](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST и CONVERT (Transact-SQL): некоторые преобразования типа данных даты и времени являются недетерминированными](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>Уровень совместимости 90 и выше

В SQL Server 2000 уровень совместимости был 80. Для уровней 80 и ниже неявные преобразования даты были детерминированными.

Начиная с SQL Server 2005 и уровня совместимости 90, неявные преобразования даты стали недетерминированными. Преобразования даты стали зависеть от SET LANGUAGE и SET DATEFORMAT, начиная с уровня 90.

#### <a name="unicode"></a>Юникод

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
Преобразование символьных данных между различными параметрами сортировки не в Юникоде также считается недетерминированным.



## <a name="see-also"></a>См. также раздел

- [Задание языка сеанса](../../relational-databases/collations/set-a-session-language.md)
- [Типы данных и функции даты и времени (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->

