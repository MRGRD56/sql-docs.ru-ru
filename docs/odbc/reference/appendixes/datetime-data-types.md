---
description: Типы данных даты и времени
title: Типы данных DateTime | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2bee41df18a0a05c7b7812c0a23321feec13ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194913"
---
# <a name="datetime-data-types"></a>Типы данных даты и времени
В ODBC *3. x* идентификаторы типов данных SQL даты, времени и timestamp изменились с SQL_DATE, SQL_TIME и SQL_TIMESTAMP (с экземплярами **#define** в файле заголовка 9, 10 и 11) на SQL_TYPE_DATE, SQL_TYPE_TIME и SQL_TYPE_TIMESTAMP (с экземплярами **#define** в файле заголовка 91, 92 и 93) соответственно. Соответствующие идентификаторы типа C изменились с SQL_C_DATE, SQL_C_TIME и SQL_C_TIMESTAMP на SQL_C_TYPE_DATE, SQL_C_TYPE_TIME и SQL_C_TYPE_TIMESTAMP соответственно, а экземпляры **#define** соответственно изменились.  
  
 Размер столбца и десятичные цифры, возвращаемые для типов данных SQL datetime в ODBC *3. x* , совпадают с точностью и масштабом, возвращаемым для них в ODBC *2. x*. Эти значения отличаются от значений в полях дескриптора SQL_DESC_PRECISION и SQL_DESC_SCALE. (Дополнительные сведения см. в разделе [размер столбца, десятичные цифры, длина октета и размер дисплея](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) в приложении г: типы данных.)  
  
 Эти изменения влияют на **SQLDescribeCol**, **SQLDescribeParam** и **SQLColAttributes**; **SQLBindCol**, **SQLBindParameter** и **SQLGetData**; и **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLStatistics** и **SQLSpecialColumns**.  
  
 Драйвер ODBC *3. x* обрабатывает вызовы функций, перечисленные в предыдущем абзаце, в соответствии с настройкой атрибута среды SQL_ATTR_ODBC_VERSION. Для **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLSpecialColumns** и **SQLStatistics**, если SQL_ATTR_ODBC_VERSION имеет значение SQL_OV_ODBC3, функции возвращают SQL_TYPE_DATE, SQL_TYPE_TIME и SQL_TYPE_TIMESTAMP в поле data_type. Столбец COLUMN_SIZE (в результирующем наборе, возвращаемом **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns** и **SQLSpecialColumns**) содержит двоичную точность для приблизительного числового типа. Столбец NUM_PREC_RADIX (в результирующем наборе, возвращаемом **SQLColumns**, **SQLGetTypeInfo** и **SQLProcedureColumns**) содержит значение 2. Если SQL_ATTR_ODBC_VERSION имеет значение SQL_OV_ODBC2, функции возвращают SQL_DATE, SQL_TIME и SQL_TIMESTAMP в поле DATA_TYPE, столбец COLUMN_SIZE содержит десятичную точность для приближенного числового типа, а столбец NUM_PREC_RADIX содержит значение 10.  
  
 Когда все типы данных запрашиваются при вызове **SQLGetTypeInfo**, результирующий набор, возвращаемый функцией, будет содержать как SQL_TYPE_DATE, SQL_TYPE_TIME, так и SQL_TYPE_TIMESTAMP, как определено в ODBC *3. x*, SQL_DATE, SQL_TIME и SQL_TIMESTAMP, как определено в ODBC *2. x*.  
  
 Из-за того, как диспетчер драйверов ODBC *3. x* выполняет сопоставление типов данных даты, времени и меток времени, ODBC *3.* драйверам x необходимо распознать только **#defines** 91, 92 и 93 для типов данных даты, времени и отметки времени C, вводимых в аргументы *TargetType* **SQLBindCol** и **SQLGetData** или аргумента *ValueType* **SQLBindParameter**, и необходимо распознать только **#defines** из 91, 92 и 93 для типов данных даты, времени и timestamp в аргументе *ParameterType* **SQLBindParameter** или аргумента *DataType* **SQLGetTypeInfo**. Дополнительные сведения см. в разделе [изменения типа данных DateTime](../../../odbc/reference/develop-app/datetime-data-type-changes.md).
