---
description: SQL Server Native Client преобразования, выполненные от сервера к клиенту
title: Преобразования, выполняемые при передаче от сервера к клиенту
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB], server to client
ms.assetid: 676fdf24-fb72-4ea0-a8d2-2b197da3c83f
author: markingmyname
ms.author: maghan
ms.custom: seo-dt-2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f7fbf944f45231a5dea9bb34de5e8095a9bc7a68
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748404"
---
# <a name="sql-server-native-client-conversions-performed-from-server-to-client"></a>SQL Server Native Client преобразования, выполненные от сервера к клиенту
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В данном разделе описываются преобразования даты и времени, выполняемые между [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (или более поздней версией) и клиентским приложением, написанным с использованием OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="conversions"></a>Преобразования  
 В следующей таблице описываются преобразования между типом, возвращенным клиенту, и типом в привязке. Если был вызван метод ICommandWithParameters::SetParameterInfo, но указанны в параметре *pwszDataSourceType* тип не совпадает с действительным типом на сервере, сервер выполнит неявное преобразование выходного параметра в тип, который указан в методе ICommandWithParameters::SetParameterInfo. Это может привести к непредвиденным результатам преобразования, если правила преобразования сервера отличаются от правил, описанных в этом разделе. Например, когда требуется предоставить дату по умолчанию, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует значение 1900-1-1, а не 1899-12-30.  
  
|Полученное значение -><br /><br /> От|DATE|DBDATE|DBTIME|DBTIME2|DBTIMESTAMP|DBTIMESTAMPOFFSET|FILETIME|BYTES|VARIANT|SSVARIANT|BSTR|STR|WSTR|  
|----------------------|----------|------------|------------|-------------|-----------------|-----------------------|--------------|-----------|-------------|---------------|----------|---------|----------|  
|Дата|1,7|OK|-|-|1|1,3|1,7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Time|5, 6, 7|-|9|OK|6|3, 6|5, 6|-|OK (VT_BSTR)|OK|OK|4|4|  
|Smalldatetime|7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Datetime|5, 7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Datetime2|5, 7|8|9,10|10|7|3|5, 7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Datetimeoffset|5, 7, 11|8, 11|9, 10, 11|10, 11|7, 11|ОК|5, 7, 11|-|OK (VT_BSTR)|OK|OK|4|4|  
|Char, Varchar,<br /><br /> Nchar, Nvarchar|7, 13|12|12, 9|12|12|12|7, 13|Н/Д|Н/Д|Н/Д|Н/Д|Н/Д|Недоступно|  
|Sql_variant<br /><br /> (datetime)|7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (smalldatetime)|7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (date)|1,7|OK|2|2|1|1,3|1,7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (time)|5, 6, 7|2|6|OK|6|3, 6|5, 6|-|OK (VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetime2)|5, 7|8|9,10|10|OK|3|5, 7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetimeoffset)|5, 7, 11|8, 11|9, 10, 11|10, 11|7, 11|ОК|5, 7, 11|-|OK (VT_BSTR)|OK|OK|4|4|  
  
## <a name="key-to-symbols"></a>Расшифровка символов  
  
|Символ|Значение|  
|------------|-------------|  
|OK|Никаких преобразований не требуется.|  
|-|Преобразование не поддерживается. Если при вызове IAccessor::CreateAccessor осуществляется проверка связывания, то в параметре *rgStatus* возвращается значение DBBINDSTATUS_UPSUPPORTEDCONVERSION. Если проверка метода доступа является отложенной, то устанавливается значение DBSTATUS_E_BADACCESSOR.|  
|1|Поля времени установлены в нуль.|  
|2|Установлено значение DBSTATUS_E_CANTCONVERTVALUE.|  
|3|Часовой пояс установлен в нуль.|  
|4|Если буфер клиента недостаточно большой, устанавливается значение DBSTATUS_S_TRUNCATED. Если тип сервера включает доли секунд, то число цифр в результирующей строке точно совпадает с масштабом типа сервера.|  
|5|Усечение секунд или долей секунд пропускается.|  
|6|Дата устанавливается в текущую дату, за исключением случая, когда источник является строковым литералом времени, а назначение — DBTYPE_DATE. В этом случае используется дата 1899-12-30.|  
|7|При превышении значения устанавливается значение DBSTATUS_E_DATAOVERFLOW.|  
|8|Поля времени не учитываются.|  
|9|Поля долей секунд пропускаются.|  
|10|Компонент даты не учитывается.|  
|11|Время приводится к часовому поясу клиента. Если во время преобразования происходит ошибка, то устанавливается значение DBSTATUS_E_DATAOVERFLOW.|  
|12|Строка анализируется как литерал ISO и преобразуется в целевой тип. Если попытка оказалась неудачной, то строка анализируется как литерал даты OLE (который также содержит компоненты времени) и преобразуется из даты OLE (DBTYPE_DATE) в целевой тип. Для успешного выполнения синтаксического анализа формата ISO, строка должна соответствовать синтаксису допускаемых литералов целевого типа. Условием успешного синтаксического анализа OLE является соответствие строки синтаксису, распознаваемому OLE. Если не удалось выполнить синтаксический анализ строки, то устанавливается значение DBSTATUS_E_CANTCONVERTVALUE. Если значения каких-либо компонентов выходят за пределы диапазона, то устанавливается значение DBSTATUS_E_DATAOVERFLOW.|  
|13|Строка анализируется как литерал ISO и преобразуется в целевой тип. Если попытка оказалась неудачной, то строка анализируется как литерал даты OLE (который также содержит компоненты времени) и преобразуется из даты OLE (DBTYPE_DATE) в целевой тип. Строка должна соответствовать синтаксису литералов типа datetime, за исключением случаев, когда назначением является DBTYPE_DATE или DBTYPE_DBTIMESTAMP. В этом случае для успешного завершения синтаксического анализа формата ISO допускаются литералы типа datetime или time. Условием успешного синтаксического анализа OLE является соответствие строки синтаксису, распознаваемому OLE. Если не удалось выполнить синтаксический анализ строки, то устанавливается значение DBSTATUS_E_CANTCONVERTVALUE. Если значения каких-либо компонентов выходят за пределы диапазона, то устанавливается значение DBSTATUS_E_DATAOVERFLOW.|  
  
## <a name="see-also"></a>См. также:  
 [Привязки и преобразования &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/conversions-ole-db.md)  
  
  
