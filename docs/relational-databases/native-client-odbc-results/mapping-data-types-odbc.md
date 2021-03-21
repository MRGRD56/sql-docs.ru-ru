---
description: Сопоставление типов данных (ODBC)
title: Сопоставление типов данных (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [ODBC]
- result sets [ODBC], data types
- ODBC data types, mapping
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], mapping
- sql_variant data type
- SQL Server Native Client ODBC driver, data types
ms.assetid: 4ba0924d-9fca-4c48-aced-0a8d817b3dde
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16da42413b38606b2e4b4e7bdaf1e13b1964c3d6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752744"
---
# <a name="mapping-data-types-odbc"></a>Сопоставление типов данных (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента сопоставляет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типы данных SQL с ТИПАМИ данных ODBC SQL. В последующих разделах обсуждаются типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL с типами данных ODBC SQL, с которыми они сопоставляются. В разделах также обсуждаются типы данных ODBC SQL и соответствующие им типы данных ODBC C, а также поддерживаемые преобразования и преобразования по умолчанию.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Тип данных **timestamp** соответствует SQL_BINARY или SQL_VARBINARY типу данных ODBC, так как значения в столбцах **отметок времени** не являются значениями **DateTime** , а значения **binary (8)** или **varbinary (8)** , которые указывают последовательность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] действий в строке. Если драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] встречает значение типа SQL_C_WCHAR (Юникод), что означает нечетное число байт, то последний нечетный байт усекается.  
  
## <a name="dealing-with-sql_variant-data-type-in-odbc"></a>Работа с типом данных sql_variant в ODBC  
 Столбец типа данных **sql_variant** может содержать любые типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , за исключением больших объектов (LOB), таких как **Text**, **ntext** и **Image**. Например, столбец может содержать значения **smallint** для некоторых строк, значения **float** для других строк и значения **типа char/nchar** в оставшейся части.  
  
 Тип данных **sql_variant** похож на тип данных **variant** в Microsoft Visual Basic®.  
  
### <a name="retrieving-data-from-the-server"></a>Получение данных с сервера  
 ODBC не имеет концепции типов Variant, ограничивая использование типа данных **sql_variant** с драйвером ODBC в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если указана привязка, **sql_variant** тип данных должен быть привязан к одному из документированных типов данных ODBC. **SQL_CA_SS_VARIANT_TYPE**, новый атрибут, относящийся к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйверу ODBC для собственного клиента, возвращает пользователю тип данных экземпляра в столбце **sql_variant** .  
  
 Если привязка не указана, можно использовать функцию [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) для определения типа данных экземпляра в столбце **sql_variant** .  
  
 Чтобы получить **sql_variant** данные, выполните следующие действия.  
  
1.  Вызовите **SQLFetch** , чтобы поместить в полученную строку.  
  
2.  Вызовите **SQLGetData**, указав SQL_C_BINARY для типа и значение 0 для длины данных. Это приведет к тому, что драйвер прочитает заголовок **sql_variant** . Заголовок предоставляет тип данных этого экземпляра в столбце **sql_variant** . **SQLGetData** возвращает размер значения (в байтах).  
  
3.  Вызовите [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md) , указав **SQL_CA_SS_VARIANT_TYPE** в качестве значения атрибута. Эта функция возвращает клиенту тип данных C экземпляра в столбце **sql_variant** .  
  
 Фрагмент кода, демонстрирующий вышеуказанные шаги:  
  
```  
while ((retcode = SQLFetch (hstmt))==SQL_SUCCESS)  
{  
    if (retcode != SQL_SUCCESS && retcode != SQL_SUCCESS_WITH_INFO)  
    {  
        SQLError (NULL, NULL, hstmt, NULL,   
                    &lNativeError,szError,MAX_DATA,&sReturned);  
        printf_s ("%s\n",szError);  
        goto Exit;  
    }  
    retcode = SQLGetData (hstmt, 1, SQL_C_BINARY,   
                                pBuff,0,&Indicator);//Figure out the length  
    if (retcode != SQL_SUCCESS_WITH_INFO && retcode != SQL_SUCCESS)  
    {  
        SQLError (NULL, NULL, hstmt, NULL, &lNativeError,   
                    szError,MAX_DATA,&sReturned);  
        printf_s ("%s\n",szError);  
        goto Exit;  
    }  
    printf_s ("Byte length : %d ",Indicator); //Print out the byte length  
  
    int iValue = 0;  
    retcode = SQLColAttribute (hstmt, 1, SQL_CA_SS_VARIANT_TYPE, NULL,   
                                        NULL,NULL,&iValue);  //Figure out the type  
    printf_s ("Sub type = %d ",iValue);//Print the type, the return is C_type of the column]  
  
// Set up a new binding or do the SQLGetData on that column with   
// the appropriate type  
}  
```  
  
 Если пользователь создает привязку с помощью [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), драйвер считывает метаданные и данные. Затем драйвер преобразует данные к соответствующему типу ODBC, указанному в привязке.  
  
### <a name="sending-data-to-the-server"></a>Отправка данных на сервер  
 **SQL_SS_VARIANT**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для данных, отправляемых в столбец **sql_variant** , используется новый тип данных, ОТНОСЯЩийся к драйверу ODBC для собственного клиента. При отправке данных на сервер с помощью параметров (например, вставка в значения TableName (?,?)) используется [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) для указания сведений о параметрах, включая тип C и соответствующий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента преобразует тип данных C в один из соответствующих подтипов **sql_variant** .  
  
## <a name="see-also"></a>См. также:  
 [Обработка результатов &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
