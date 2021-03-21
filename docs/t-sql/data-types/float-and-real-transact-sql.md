---
description: Типы данных float и real (Transact-SQL)
title: Типы данных float и real (Transact-SQL)
ms.custom: ''
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- float
- real_TSQL
- real
- float_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- numeric data type, floating point
- float data type
- floating point data [SQL Server]
- real data type
ms.assetid: 08ea66b7-624e-4d8b-86bc-750ff76cdfc5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab78f62c182c2daaa9de67d1e19a5711dc932d94
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752734"
---
# <a name="float-and-real-transact-sql"></a>Типы данных float и real (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Типы приблизительных числовых данных, используемые для числовых данных с плавающей запятой. Данные с плавающей запятой являются приблизительными, поэтому не все значения из диапазона могут быть отображены точно. Синонимом по стандарту ISO для типа **real** является **float(24)** .
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
**float** [ **(** _n_ **)** ] Где *n* — это количество битов, используемых для хранения мантиссы числа в формате **float** при экспоненциальном представлении. Определяет точность данных и размер для хранения. Если указан параметр *n*, это должно быть значение в диапазоне от **1** до **53**. Значение *n* по умолчанию — **53**.
  
|Значение *n*|Точность|Объем памяти|  
|---|---|---|
|**1-24**|7 цифр|4 байта|  
|**25-53**|15 знаков|8 байт|  
  
> [!NOTE]  
>  В приложении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр *n* может принимать одно из двух возможных значений. Если **1**<=n<=**24**, *n* принимает значение **24**. Если **25**<=n<=**53**, *n* принимает значение **53**.  
  
Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **float**[ **(n)** ] соответствует стандарту ISO для всех значений *n* в диапазоне от **1** до **53**. Синонимом типа **double precision** является тип **float(53)** .

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks  
  
|Тип данных|Диапазон|Память|  
|---|---|---|
|**float**|- 1,79E+308 — -2,23E-308, 0 и 2,23E-308 — 1,79E+308|Зависит от значения *n*|  
|**real**|- 3,40E + 38 — -1,18E - 38, 0 и 1,18E - 38 — 3,40E + 38|4 байта|  
  
##  <a name="converting-float-and-real-data"></a>Преобразование данных типа float и real  
При преобразовании в любой целочисленный тип данных значения типа **float** усекаются.
  
Если тип данных **float** или **real** нужно преобразовать в символьный тип, то, как правило, строковую функцию STR использовать удобнее, чем CAST( ). Это объясняется большими возможностями функции STR в отношении форматирования. Дополнительные сведения см. в статьях [STR (Transact-SQL)](../../t-sql/functions/str-transact-sql.md) и [Функции (Transact-SQL)](../../t-sql/functions/functions.md).
  
До версии [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] точность преобразования значений **float** в **decimal** или **numeric** была ограничена 17 знаками. Любое значение типа **float** менее 5E-18 (в экспоненциальном представлении 5E-18 или десятичном представлении 0.0000000000000000050000000000000005) округлялось до 0. Начиная с версии [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] это ограничение отсутствует.
  
## <a name="see-also"></a>См. также раздел
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
