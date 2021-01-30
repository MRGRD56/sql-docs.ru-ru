---
description: 'Преобразование данных из SQL в C: битовые данные'
title: 'С SQL на C: bit | Документация Майкрософт'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], bit
- bit data type [ODBC]
- data conversions from SQL to C types [ODBC], bit
ms.assetid: 0eeaab8b-ad82-4a36-b464-9a1211d5f72c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce4e38a450a16bc1eeb23f54ef68445a29ef4d97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203081"
---
# <a name="sql-to-c-bit"></a>Преобразование данных из SQL в C: битовые данные
Идентификатором для типа данных bit SQL ODBC является:  
  
 SQL_BIT  
  
 В следующей таблице показаны типы данных ODBC C, к которым могут быть преобразованы битовые данные SQL. Описание столбцов и терминов в таблице см. в разделе [Преобразование данных из SQL в типы данных C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Идентификатор типа C|Тест|**таржетвалуептр*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR<br /><br /> SQL_C_WCHAR|*BufferLength* > 1<br /><br /> *BufferLength* <= 1|Данные<br /><br /> Не определено.|1<br /><br /> Не определено.|н/д<br /><br /> 22003|  
|SQL_C_STINYINT<br /><br /> SQL_C_UTINYINT<br /><br /> SQL_C_TINYINT<br /><br /> SQL_C_SBIGINT<br /><br /> SQL_C_UBIGINT<br /><br /> SQL_C_SSHORT<br /><br /> SQL_C_USHORT<br /><br /> SQL_C_SHORT<br /><br /> SQL_C_SLONG<br /><br /> SQL_C_ULONG<br /><br /> SQL_C_LONG<br /><br /> SQL_C_FLOAT<br /><br /> SQL_C_DOUBLE<br /><br /> SQL_C_NUMERIC|Нет [a]|Данные|Размер типа данных C|н/д|  
|SQL_C_BIT|Нет [a]|Данные|1 [b]|н/д|  
|SQL_C_BINARY|*BufferLength* >= 1<br /><br /> *BufferLength* < 1|Данные<br /><br /> Не определено.|1<br /><br /> Не определено.|н/д<br /><br /> 22003|  
  
 [a] значение *BufferLength* игнорируется для этого преобразования. Драйвер предполагает, что размер **таржетвалуептр* — это размер типа данных C.  
  
 [b] это размер соответствующего типа данных C.  
  
 Когда двоичные данные SQL преобразуются в символьные данные C, возможны значения "0" и "1".
