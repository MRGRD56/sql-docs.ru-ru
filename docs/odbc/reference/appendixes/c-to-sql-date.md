---
description: 'Преобразование из C в SQL: даты'
title: 'C в SQL: Date | Документация Майкрософт'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date data type [ODBC]
- converting data from c to SQL types [ODBC], date
- data conversions from C to SQL types [ODBC], date
ms.assetid: bea087d3-911f-418b-b483-d2b5b334da19
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0724309642b8a6dc640b6159715927544d74733
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207852"
---
# <a name="c-to-sql-date"></a>Преобразование из C в SQL: даты
Идентификатор для типа данных Date ODBC C:  
  
 SQL_C_TYPE_DATE  
  
 В следующей таблице показаны типы данных ODBC SQL, к которым могут быть преобразованы данные даты C. Описание столбцов и терминов в таблице см. в разделе [Преобразование данных из C в типы данных SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Идентификатор типа SQL|Тест|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Длина байта столбца >= 10<br /><br /> Длина байта столбца < 10<br /><br /> Значение данных не является допустимой датой|н/д<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Длина символа столбца >= 10<br /><br /> Длина символа столбца < 10<br /><br /> Значение данных не является допустимой датой|н/д<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|Значение данных является допустимой датой<br /><br /> Значение данных не является допустимой датой|н/д<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Значение данных является допустимой датой [a]<br /><br /> Значение данных не является допустимой датой|н/д<br /><br /> 22007|  
  
 [a] временная часть метки времени имеет значение 0.  
  
 Сведения о том, какие значения допустимы в структуре SQL_C_TYPE_DATE, см. в разделе [типы данных C](../../../odbc/reference/appendixes/c-data-types.md)ранее в этом приложении.  
  
 Когда данные даты C преобразуются в символьные данные SQL, полученные символьные данные имеют формат "*гггг* - *мм* - *дд*".  
  
 Драйвер не учитывает значение длины и индикатора при преобразовании данных из типа данных даты C и предполагает, что размер буфера данных равен размеру данных типа "Дата C". Значение length/индикатора передается в аргументе *StrLen_Or_Ind* в **SQLPutData** и в буфере, указанном аргументом *StrLen_or_IndPtr* в **SQLBindParameter**. Буфер данных указывается с помощью аргумента *датаптр* в **SQLPutData** и аргумента *параметервалуептр* в **SQLBindParameter**.
