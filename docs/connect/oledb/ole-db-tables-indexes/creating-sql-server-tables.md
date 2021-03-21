---
title: Создание таблиц SQL Server (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как функция ITableDefinition::CreateTable в OLE DB Driver for SQL Server позволяет объектам-получателям создавать таблицы SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- OLE DB Driver for SQL Server, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cab38e1c68bc12cab536090cc074cab14d97a48e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754274"
---
# <a name="creating-sql-server-tables"></a>Создание таблиц SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В драйвере OLE DB для SQL Server доступна функция **ITableDefinition::CreateTable** , которая позволяет потребителям создавать таблицы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Потребители используют функцию **CreateTable** для создания именованных потребителем постоянных таблиц, а также постоянных и временных таблиц с уникальными именами, созданными драйвером OLE DB для SQL Server.  
  
 При вызове потребителем метода **ITableDefinition::CreateTable**, если свойство DBPROP_TBL_TEMPTABLE имеет значение VARIANT_TRUE, драйвер OLE DB для SQL Server вместо потребителя создает временное имя таблицы. Потребитель задает для параметра *pTableID* метода **CreateTable** значение NULL. Временные таблицы с именами, сформированными драйвером OLE DB для SQL Server, не содержатся в наборе строк **TABLES**, но к ним можно получить доступ через интерфейс **IOpenRowset**.  
  
 Когда потребители задают имя таблицы в члене *pwszName* объединения *uName* в параметре *pTableID*, драйвер OLE DB для SQL Server создает таблицу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с таким именем. Имя таблицы подчиняется ограничениям для имен таблиц, принятым в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], и может указывать на постоянную таблицу, а также локальную или глобальную временную таблицу. Дополнительные сведения см. в разделе [CREATE TABLE](../../../t-sql/statements/create-table-transact-sql.md). Параметр *ppTableID* может иметь значение NULL.  
  
 OLE DB Driver for SQL Server может создавать имена постоянных или временных таблиц. Когда потребитель задает для параметра *pTableID* значение NULL, а для параметра *ppTableID* — значение, указывающее на действительный DBID\*, драйвер OLE DB для SQL Server возвращает сформированное имя таблицы в члене *pwszName* объединения *uName* в DBID, на который указывает значение *ppTableID*. Для создания временной таблицы, имя для которой сформирует драйвер OLE DB для SQL Server, потребитель включает свойство таблицы OLE DB DBPROP_TBL_TEMPTABLE в набор свойств таблицы, на который указывает параметр *rgPropertySets*. Названные OLE DB Driver for SQL Server временные таблицы являются локальными.  
  
 Функция **CreateTable** возвращает значение DB_E_BADTABLEID, если член *eKind* параметра *pTableID* не указывает на DBKIND_NAME.  
  
## <a name="dbcolumndesc-usage"></a>Использование структуры DBCOLUMNDESC  
 Потребитель может задать тип данных столбца либо с помощью переменной-члена *pwszTypeName*, либо с помощью переменной-члена *wType*. Если потребитель указывает тип данных в *pwszTypeName*, OLE DB Driver for SQL Server игнорирует значение *wType*.  
  
 Если используется переменная-член *pwszTypeName*, потребитель задает тип данных, используя имена типов данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Допустимыми являются имена типов данных, возвращаемые в столбце TYPE_NAME набора строк схемы PROVIDER_TYPES.  
  
 OLE DB Driver for SQL Server распознает подмножество значений DBTYPE перечисления OLE DB в элементе *wType*. Дополнительные сведения см. в разделе [Сопоставление типов данных в интерфейсе ITableDefinition](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md).  
  
> [!NOTE]  
>  Функция **CreateTable** возвращает значение DB_E_BADTYPE, если потребитель указал тип данных столбца с помощью члена *pTypeInfo* или *pclsid*.  
  
 Пользователь задает имя столбца в элементе *pwszName* объединения *uName* в переменной-члене *dbcid* структуры DBCOLUMNDESC. Имя столбца задается в виде символьной строки в Юникоде. Элемент *eKind* структуры *dbcid* должен быть равен DBKIND_NAME. Функция **CreateTable** возвращает значение DB_E_BADCOLUMNID, если элемент *eKind* недопустим или значение *pwszName* равно NULL либо не является допустимым идентификатором [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Все свойства столбцов доступны для всех столбцов, определенных в данной таблице. Функция **CreateTable** возвращает значение DB_S_ERRORSOCCURRED или DB_E_ERRORSOCCURRED, если установленные значения свойств противоречат друг другу. Функция **CreateTable** возвращает ошибку, если недопустимые значения свойств столбцов вызывают ошибку при создании таблицы в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Свойства столбцов в структуре DBCOLUMNDESC интерпретируются следующим образом.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: Описание VARIANT_FALSE: задает свойство идентификатора для создаваемого столбца. В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] свойство идентификатора может иметь только один столбец таблицы. Если установить значение VARIANT_TRUE этого свойства для нескольких столбцов, возникнет ошибка при попытке драйвера OLE DB для SQL Server создать таблицу на сервере.<br /><br /> Свойство идентификаторов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] действительно только для типов **integer**, **numeric** и **decimal**, когда масштаб равен 0. Установка свойства столбца в VARIANT_TRUE или в любой другой тип данных вызовет ошибку, когда драйвер OLE DB для SQL Server попытается создать таблицу на сервере.<br /><br /> Драйвер OLE DB для SQL Server возвращает значение DB_S_ERRORSOCCURRED, если оба свойства DBPROP_COL_AUTOINCREMENT и DBPROP_COL_NULLABLE имеют значение VARIANT_TRUE, а элемент *dwOption* свойства DBPROP_COL_NULLABLE не равен DBPROPOPTIONS_REQUIRED. Если оба свойства DBPROP_COL_AUTOINCREMENT и DBPROP_COL_NULLABLE имеют значение VARIANT_TRUE и элемент *dwOption* свойства DBPROP_COL_NULLABLE равен DBPROPOPTIONS_REQUIRED, будет возвращено значение DB_E_ERRORSOCCURRED. Столбец определяется с помощью свойства идентификатора [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а элементу DBPROP_COL_NULLABLE *dwStatus* присваивается значение DBPROPSTATUS_CONFLICTING.|  
|DBPROP_COL_DEFAULT|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: None<br /><br /> Описание. Создает для столбца ограничение [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] DEFAULT.<br /><br /> Элемент *vValue* структуры DBPROP может относиться к любому типу из определенного набора типов. Элемент *vValue.vt* должен задавать тип, совместимый с типом данных столбца. Например, если столбец имеет тип DBTYPE_WSTR и для этого столбца задано значение по умолчанию BSTR N/A, эти два типа совместимы. Если установить такое же значение по умолчанию для столбца с типом DBTYPE_R8, возникнет ошибка при попытке драйвера OLE DB для SQL Server создать таблицу на сервере.|  
|DBPROP_COL_DESCRIPTION|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: None<br /><br /> Описание. Свойство столбца DBPROP_COL_DESCRIPTION не реализовано OLE DB Driver for SQL Server.<br /><br /> Элемент *dwStatus* структуры DBPROP возвращает значение DBPROPSTATUS_NOTSUPPORTED при попытке потребителя записать значение свойства.<br /><br /> Установка свойства не является неустранимой ошибкой для OLE DB Driver for SQL Server. Если все остальные значения параметров допустимы, таблица [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] будет создана.|  
|DBPROP_COL_FIXEDLENGTH|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. OLE DB Driver for SQL Server использует значение DBPROP_COL_FIXEDLENGTH для определения сопоставления типов данных, когда потребитель определяет тип данных столбца, используя элемент *wType* структуры DBCOLUMNDESC. Дополнительные сведения см. в разделе [Сопоставление типов данных в интерфейсе ITableDefinition](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md).|  
|DBPROP_COL_NULLABLE|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: None<br /><br /> Описание. При создании таблицы OLE DB Driver for SQL Server указывает, разрешаются ли для данного столбца значения NULL, если задано это свойство. Когда свойство не задано, способность столбца принимать значения NULL определяется установленным по умолчанию параметром базы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ANSI_NULLS.<br /><br /> OLE DB Driver for SQL Server является ISO-совместимым поставщиком. Подключенные сеансы ведут себя в соответствии со стандартом ISO. Если потребитель не задал свойство DBPROP_COL_NULLABLE, столбец принимает значения NULL.|  
|DBPROP_COL_PRIMARYKEY|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: Описание VARIANT_FALSE: при VARIANT_TRUE OLE DB Driver for SQL Server создает столбец с ограничением PRIMARY KEY.<br /><br /> При задании в качестве свойства столбца только один столбец может определять это ограничение. Если установить значение этого свойства в VARIANT_TRUE для нескольких столбцов, возникнет ошибка при попытке драйвера OLE DB для SQL Server создать таблицу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Примечание. Потребитель может использовать метод **IIndexDefinition::CreateIndex** для создания ограничения PRIMARY KEY для нескольких столбцов.<br /><br /> Драйвер OLE DB для SQL Server возвращает значение DB_S_ERRORSOCCURRED, если оба свойства DBPROP_COL_PRIMARYKEY и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE, а элемент *dwOption* свойства DBPROP_COL_UNIQUE не равен DBPROPOPTIONS_REQUIRED.<br /><br /> Если оба свойства DBPROP_COL_PRIMARYKEY и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE и элемент *dwOption* свойства DBPROP_COL_UNIQUE равен DBPROPOPTIONS_REQUIRED, будет возвращено значение DB_E_ERRORSOCCURRED. Столбец определяется с помощью свойства идентификаторов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а элементу DBPROP_COL_PRIMARYKEY *dwStatus* присваивается значение DBPROPSTATUS_CONFLICTING.<br /><br /> Драйвер OLE DB для SQL Server возвращает ошибку, когда оба свойства DBPROP_COL_PRIMARYKEY и DBPROP_COL_NULLABLE равны VARIANT_TRUE.<br /><br /> Драйвер OLE DB для SQL Server возвращает ошибку из [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], если потребитель пытается создать ограничение PRIMARY KEY для столбца недопустимого типа данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ограничения PRIMARY KEY нельзя определять для столбцов, относящихся к типам данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**bit**, **text**, **ntext** и **image**.|  
|DBPROP_COL_UNIQUE|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: Описание VARIANT_FALSE: применяет к столбцу ограничение [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UNIQUE.<br /><br /> При задании в качестве свойства столбца только к одному столбцу может быть применено это ограничение. Потребитель может использовать метод **IIndexDefinition::CreateIndex** с целью создания ограничения UNIQUE для сочетания значений нескольких столбцов.<br /><br /> Драйвер OLE DB для SQL Server возвращает значение DB_S_ERRORSOCCURRED, если оба свойства DBPROP_COL_PRIMARYKEY и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE, а элемент *dwOption* не равен DBPROPOPTIONS_REQUIRED.<br /><br /> Если оба свойства DBPROP_COL_PRIMARYKEY и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE и элемент *dwOption* равен DBPROPOPTIONS_REQUIRED, будет возвращено значение DB_E_ERRORSOCCURRED. Столбец определяется с помощью свойства идентификаторов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а элементу DBPROP_COL_PRIMARYKEY *dwStatus* присваивается значение DBPROPSTATUS_CONFLICTING.<br /><br /> Драйвер OLE DB для SQL Server возвращает значение DB_S_ERRORSOCCURRED, если оба свойства DBPROP_COL_NULLABLE и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE, а элемент *dwOption* не равен DBPROPOPTIONS_REQUIRED.<br /><br /> Если оба свойства DBPROP_COL_NULLABLE и DBPROP_COL_UNIQUE имеют значение VARIANT_TRUE и элемент *dwOption* равен DBPROPOPTIONS_REQUIRED, будет возвращено значение DB_E_ERRORSOCCURRED. Столбец определяется с помощью свойства идентификатора [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а элементу DBPROP_COL_NULLABLE *dwStatus* присваивается значение DBPROPSTATUS_CONFLICTING.<br /><br /> Драйвер OLE DB для SQL Server возвращает ошибку из [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], если потребитель пытается создать ограничение UNIQUE для столбца недопустимого типа данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ограничения UNIQUE нельзя определять для столбцов, относящихся к типу данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **bit**.|  
  
 Когда потребитель вызывает метод **ITableDefinition::CreateTable**, драйвер OLE DB для SQL Server интерпретирует свойства таблицы указанным ниже образом.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|Ч/З Чтение/запись<br /><br /> Значение по умолчанию: Описание VARIANT_FALSE: по умолчанию OLE DB Driver for SQL Server создает таблицы с именами, указанными объектом-получателем. При VARIANT_TRUE OLE DB Driver for SQL Server создает имя временной таблицы для объекта-получателя. Потребитель задает для параметра *pTableID* метода **CreateTable** значение NULL. Параметр *ppTableID* должен содержать допустимый указатель.|  
  
 Если потребитель запрашивает открытие набора строк для успешно созданной таблицы, драйвер OLE DB для SQL Server открывает набор строк с поддержкой курсора. Все свойства набора строк могут быть заданы в передаваемых наборах свойств.  
  
 В данном примере создается таблица [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```  
// This CREATE TABLE statement shows the details of the table created by   
// the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//    CONSTRAINT PK_OrderDetails  
//         PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
// The PRIMARY KEY constraint is created in an additional example.  
HRESULT CreateTable  
    (  
    ITableDefinition* pITableDefinition  
    )  
    {  
    DBID            dbidTable;  
    const ULONG     nCols = 5;  
    ULONG           nCol;  
    ULONG           nProp;  
    DBCOLUMNDESC    dbcoldesc[nCols];  
  
    HRESULT         hr;  
  
    // Set up column descriptions. First, set default property values for  
    //  the columns.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbcoldesc[nCol].pwszTypeName = NULL;  
        dbcoldesc[nCol].pTypeInfo = NULL;  
        dbcoldesc[nCol].rgPropertySets = new DBPROPSET;  
        dbcoldesc[nCol].pclsid = NULL;  
        dbcoldesc[nCol].cPropertySets = 1;  
        dbcoldesc[nCol].ulColumnSize = 0;  
        dbcoldesc[nCol].dbcid.eKind = DBKIND_NAME;  
        dbcoldesc[nCol].wType = DBTYPE_I4;  
        dbcoldesc[nCol].bPrecision = 0;  
        dbcoldesc[nCol].bScale = 0;  
  
        dbcoldesc[nCol].rgPropertySets[0].rgProperties =   
            new DBPROP[NCOLPROPS_MAX];  
        dbcoldesc[nCol].rgPropertySets[0].cProperties = NCOLPROPS_MAX;  
        dbcoldesc[nCol].rgPropertySets[0].guidPropertySet =  
            DBPROPSET_COLUMN;  
  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                dwOptions = DBPROPOPTIONS_REQUIRED;  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].colid  
                 = DB_NULLID;  
  
            VariantInit(  
                &(dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                    vValue));  
  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt = VT_BOOL;  
            }  
        }  
  
    // Set the column-specific information.  
    dbcoldesc[0].dbcid.uName.pwszName = L"OrderID";  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[0].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[1].dbcid.uName.pwszName = L"ProductID";  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[1].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[2].dbcid.uName.pwszName = L"UnitPrice";  
    dbcoldesc[2].wType = DBTYPE_CY;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[2].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[3].dbcid.uName.pwszName = L"Quantity";  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[3].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[4].dbcid.uName.pwszName = L"Discount";  
    dbcoldesc[4].wType = DBTYPE_NUMERIC;  
    dbcoldesc[4].bPrecision = 2;  
    dbcoldesc[4].bScale = 2;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].dwPropertyID =   
        DBPROP_COL_DEFAULT;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.vt = VT_BSTR;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.bstrVal =  
        SysAllocString(L"0");  
    dbcoldesc[4].rgPropertySets[0].cProperties = 2;  
  
    // Set up the dbid for OrderDetails.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    if (FAILED(hr = pITableDefinition->CreateTable(NULL, &dbidTable,  
        nCols, dbcoldesc, NULL, 0, NULL, NULL, NULL)))  
        {  
        DumpError(pITableDefinition, IID_ITableDefinition);  
        goto SAFE_EXIT;  
        }  
  
SAFE_EXIT:  
    // Clean up dynamic allocation in the property sets.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            if (dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt == VT_BSTR)  
                {  
                SysFreeString(dbcoldesc[nCol].rgPropertySets[0].  
                    rgProperties[nProp].vValue.bstrVal);  
                }  
            }  
  
        delete [] dbcoldesc[nCol].rgPropertySets[0].rgProperties;  
        delete [] dbcoldesc[nCol].rgPropertySets;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>См. также:  
 [Таблицы и индексы](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)  
  
  
