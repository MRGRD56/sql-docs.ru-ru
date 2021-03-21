---
description: Тип ODBC SQL для параметров, возвращающих табличное значение
title: Тип ODBC SQL для параметров Table-Valued | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL_SS_TABLE
ms.assetid: 6725bfb9-5f10-4115-be09-fd9c9f5779ea
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 723177b3e61af5b5734f3c93dce7d6de72b8bb12
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748098"
---
# <a name="odbc-sql-type-for-table-valued-parameters"></a>Тип ODBC SQL для параметров, возвращающих табличное значение
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Поддержка возвращающих табличное значение параметров обеспечивается новым типом ODBC SQL — SQL_SS_TABLE.  
  
## <a name="remarks"></a>Remarks  
 Тип SQL_SS_TABLE нельзя преобразовать в любой другой тип данных ODBC или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Если SQL_SS_TABLE используется в качестве типа данных C в параметре *ValueType* SQLBindParameter или предпринимается попытка установить SQL_DESC_TYPE в записи дескриптора параметра приложения (APD) в SQL_SS_TABLE, возвращается SQL_ERROR и создается диагностическая запись с параметром SQLSTATE = HY003, "Недопустимый тип буфера приложения".  
  
 Если SQL_DESC_TYPE устанавливается в IPD-записи в значение SQL_SS_TABLE, а соответствующая запись дескриптора параметра приложения не SQL_C_DEFAULT, то возвращается значение SQL_ERROR и создается диагностическая запись с кодом SQLSTATE=HY003 и сообщением «Недопустимый тип буфера приложения». Это может произойти с *ParameterType* SQLSetDescField, SQLSetDescRec или SQLBindParameter.  
  
 Если параметр *TargetType* SQL_SS_TABLE при вызове SQLGetData, возвращается SQL_ERROR и создается диагностическая запись с параметром SQLSTATE = HY003, "Недопустимый тип буфера приложения".  
  
 Столбец возвращающего табличное значение параметра не может привязываться в виде типа SQL_SS_TABLE. Если функция **SQLBindParameter** вызывается с параметром *ParameterType* типа SQL_SS_TABLE, то возвращается значение SQL_ERROR и создается диагностическая запись с кодом SQLSTATE=HY004 и сообщением «Недопустимый тип данных SQL». Это также может произойти с SQLSetDescField и SQLSetDescRec.  
  
 Значения столбца возвращающего табличные значения параметра имеют те же возможности преобразования данных, как параметры и результирующие столбцы.  
  
 В [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях возвращающий табличное значение параметр может быть только входным параметром. Если предпринимается попытка задать для SQL_DESC_PARAMETER_TYPE значение, отличное от SQL_PARAM_INPUT через SQLBindParameter или SQLSetDescField, возвращается SQL_ERROR, а в инструкцию с SQLSTATE = HY105 и сообщением «Недопустимый тип параметра» добавляется запись диагностики.  
  
 Столбцы возвращающего табличное значение параметра не могут использовать в качестве параметра *StrLen_or_IndPtr* значение SQL_DEFAULT_PARAM, так как значения по умолчанию не поддерживаются возвращающими табличное значение параметрами. Вместо этого приложение может установить атрибут столбца SQL_CA_SS_COL_HAS_DEFAULT_VALUE в значение 1. Это значит, что во всех строках столбца будут значения по умолчанию. Если *StrLen_or_IndPtr* имеет значение SQL_DEFAULT_PARAM, то SQLExecute или SQLExecDirect возвращает SQL_ERROR, а запись диагностики будет добавлена в инструкцию с параметром SQLSTATE = HY090 и сообщением "Недопустимая строка или длина буфера".  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
