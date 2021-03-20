---
description: Использование типов данных
title: Использование типов данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC data types
- ODBC data types, about ODBC data types
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC]
- SQL Server Native Client ODBC driver, data types
- data types [ODBC], about data types
ms.assetid: 4f19b0d6-94ac-4a98-a121-57d38787864c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b2ceef68921c64d9e51e8f4ba68b315a88ec24fc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752754"
---
# <a name="data-type-usage"></a>Использование типов данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] накладывает следующее использование типов данных.  
  
|Тип данных|Ограничение|  
|---------------|----------------|  
|Литералы даты|Литералы даты, хранящиеся в столбце SQL_TYPE_TIMESTAMP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типы данных **DateTime** или **smalldatetime**), имеют значение времени 12:00:00.000 A.M..|  
|**money** и **smallmoney**|Значимыми являются только целые части типов данных **money** и **smallmoney** . Если десятичная часть данных SQL **money** усекается во время преобразования типов данных, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента возвращает предупреждение, а не ошибку.|  
|SQL_BINARY (допускающий значения NULL)|Если столбец SQL_BINARY допускает значения NULL, при подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 6.0 или более ранней данные, помещенные в источник данных, не заполняются нулями. При извлечении данных из такого столбца [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента дополняет его нулями справа. Однако данные, которые создаются в операциях, выполняемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например объединении), не имеют такого заполнения.<br /><br /> Также при помещении данных в такие столбцы в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 6.0 или более ранней [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] усекает данные справа, если их длина слишком большая, чтобы поместиться в столбец.<br /><br /> Примечание. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6,5 и более ранним версиям.|  
|SQL_CHAR (усечение)|Если при подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 или более ранней версии данные помещаются в столбец SQL_CHAR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] усекает их справа, не выдавая предупреждения, если их длина слишком большая, чтобы поместиться в столбец.<br /><br /> Примечание. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6,5 и более ранним версиям.|  
|SQL_CHAR (допускающий значения NULL)|Если столбец SQL_CHAR допускает значения NULL, при подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 6.0 или более ранней данные, данные, сохраненные в источник данных, не дополняются пробелами. При извлечении данных из такого столбца [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента дополняет его пустыми расположениями справа. Однако данные, которые создаются в операциях, выполняемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например объединении), не имеют такого заполнения.<br /><br /> Примечание. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6,5 и более ранним версиям.|  
|SQL_LONGVARBINARY, SQL_LONGVARCHAR, SQL_WLONGVARCHAR|Обновления столбцов с типами данных SQL_LONGVARBINARY, SQL_LONGVARCHAR или SQL_WLONGVARCHAR (с использованием предложения WHERE), которые влияют на несколько строк, полностью поддерживаются при подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.*x* и более поздних версий. При подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4,2 *x* возникает ошибка S1000, «частичная Вставка/обновление». Вставка или обновление столбца text или image не завершено», если обновление затрагивает более одной строки.<br /><br /> Примечание. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6,5 и более ранним версиям.|  
|Параметры строковых функций|*string_exp* параметры строковых функций должны иметь тип данных SQL_CHAR или SQL_VARCHAR. Типы данных SQL_LONG_VARCHAR не поддерживаются в строковых функциях. Параметр *Count* должен быть меньше или равен 8 000, поскольку типы данных SQL_CHAR и SQL_VARCHAR ограничены максимальной длиной 8 000 символов.|  
|Литералы времени|Литералы времени, хранящиеся в столбце SQL_TIMESTAMP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типы данных **DateTime** или **smalldatetime**), имеют значение даты, равное 1 января 1900.|  
|**timestamp**|В столбец **timestamp** можно вручную вставить только значение null. Однако, поскольку столбцы **отметок времени** автоматически обновляются с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , значение NULL перезаписывается.|  
|**tinyint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Тип данных **tinyint** не подписан. Столбец **tinyint** привязан к переменной типа данных SQL_C_UTINYINT по умолчанию.|  
|Псевдонимы типов данных|При подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4,2 *x* драйвер ODBC добавляет значение NULL в определение столбца, которое явно не объявляет допустимость значения NULL для столбца. Поэтому допустимость значений NULL, помещенная в определение псевдонима типа данных, не учитывается.<br /><br /> При подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4,2 *x* столбцы с псевдонимом типа данных, имеющим базовый тип данных **char** или **binary** и для которого не объявлено допустимость значения NULL, создаются как типы данных **varchar** или **varbinary**. [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md), [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)и [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) возвращают SQL_VARCHAR или SQL_VARBINARY в качестве типа данных для этих столбцов. Данные, полученные из этих столбцов, не заполнены.<br /><br /> Примечание. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6,5 и более ранним версиям.|  
|Типы данных большой длины|параметры для *данных на выполнение* ограничены как для SQL_LONGVARBINARY, так и для SQL_LONGVARCHARных типов данных.|  
|Типы больших значений|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента будет предоставлять типы **varchar (max)**, **varbinary (max)** и **nvarchar (max)** как SQL_VARCHAR, SQL_VARBINARY и SQL_WVARCHAR (соответственно) в API-интерфейсах, которые принимают или возвращают типы данных ODBC SQL.|  
|Определяемый пользователем тип|Столбцы определяемого пользователем типа преобразуются в тип данных SQL_SS_UDT. Если столбец определяемого пользователем типа явно сопоставлен с другим типом в инструкции SQL с помощью методов ToString() или ToXMLString() определяемого пользователем типа или с помощью функций CAST/CONVERT, то тип столбца в результирующем наборе будет отражать реальный тип, к которому столбец был преобразован.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента может быть привязан только к столбцу определяемого пользователем типа как к двоичному типу. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает преобразование только между типами данных SQL_SS_UDT и SQL_C_BINARY.|  
|XML|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически преобразует XML в текст Юникода. Тип XML преобразуется в SQL_SS_XML.|  
  
## <a name="see-also"></a>См. также:  
 [Обработка результатов &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
