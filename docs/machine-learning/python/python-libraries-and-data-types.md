---
title: Преобразование типов данных Python и SQL
description: Обзор явных и неявных преобразований типов данных между Python и SQL Server в решениях для обработки и анализа данных и машинного обучения.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 1d83caf3f80369da449175fca78a47677e10e861
ms.sourcegitcommit: 9774e2cb8c07d4f6027fa3a5bb2852e4396b3f68
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92098813"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Сопоставления типов данных между Python и SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

В этой статье перечислены поддерживаемые типы данных и преобразования типов данных при использовании функции интеграции Python в службы машинного обучения SQL Server.

По сравнению с SQL Server Python поддерживает ограниченное число типов данных. Поэтому при каждом использовании данных из SQL Server в сценариях Python данные SQL могут быть неявно преобразованы в совместимый тип данных Python. Однако зачастую точное преобразование невозможно выполнить автоматически, и в результате возвращается ошибка.

## <a name="python-and-sql-data-types"></a>Типы данных Python и SQL

В этой таблице приводятся неявные преобразования. Другие типы данных не поддерживаются.

| Тип SQL             | Тип Python | Описание |
|----------------------|-------------|-------------|
| **bigint**           | `float64`   |
| **binary**           | `bytes`     |
| **bit**              | `bool`      |
| **char**             | `str`       |
| **date**             | `datetime`  |
| **datetime**         |`datetime`   | Поддерживается с SQL Server 2017 CU6 и более поздних версий (с массивами **NumPy** типа `datetime.datetime` или **Pandas** `pandas.Timestamp`). `sp_execute_external_script` теперь поддерживает типы `datetime` с долей секунды.|
| **float**            | `float64`   |
| **nchar**            | `str`       |
| **nvarchar**         | `str`       |
| **nvarchar(max)**    | `str`       |
| **real**             | `float64`   |
| **smalldatetime**    | `datetime`  |
| **smallint**         | `int32`     |
| **tinyint**          | `int32`     |
| **uniqueidentifier** | `str`       |
| **varbinary**        | `bytes`     |
| **varbinary(max)**   | `bytes`     |
| **varchar(n)**       | `str`       |
| **varchar(max)**     | `str`       |

## <a name="see-also"></a>См. также раздел

+ [Сопоставления типов данных между R и SQL Server](../r/r-libraries-and-data-types.md)
