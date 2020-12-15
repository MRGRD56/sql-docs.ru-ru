---
title: Сценарии запросов PolyBase | Документация Майкрософт
description: См. примеры запросов с использованием компонента PolyBase SQL Server, включая SELECT, JOIN для объединения внешних таблиц с локальными таблицами, импорт и экспорт данных, а также новые представления каталога.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 2eec9c64a65ae0fb548944dc88a056c15e5c1e80
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97416954"
---
# <a name="polybase-query-scenarios"></a>Сценарии запросов PolyBase

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

В этой статье приведены примеры запросов, в которых применяется компонент [PolyBase](../../relational-databases/polybase/polybase-guide.md) SQL Server (начиная с версии 2016). Прежде чем использовать эти примеры, необходимо сначала установить и настроить PolyBase. Дополнительные сведения см. в [руководстве по PolyBase](polybase-guide.md).
  
Отправить запрос к внешним таблицам можно с помощью инструкций Transact-SQL или средств бизнес-аналитики.
  
## <a name="select-from-external-table"></a>Выбор данных из внешней таблицы (SELECT)  

Простой запрос, возвращающий данные из определенной внешней таблицы.  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

Простой запрос, включающий предикат.

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>Соединение внешних и локальных таблиц (JOIN)

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>Импорт данных

Вы можете импортировать данные из Hadoop или службы хранилища Azure в SQL Server для постоянного хранения. Чтобы импортировать данные, на которые ссылается внешняя таблица, следует использовать инструкцию SELECT INTO. Оперативно создайте реляционную таблицу, а затем индекс хранилища столбцов на основе таблицы, описанной на втором шаге.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```

## <a name="export-data"></a>Экспорт данных

Вы можете экспортировать данные из SQL Server в службу хранилища Azure или Hadoop. 

В первую очередь включите функцию экспорта, задав для аргумента `sp_configure` параметра allow polybase export значение 1. Затем создайте внешнюю таблицу, которая указывает на целевой каталог. Если целевой каталог не существует, инструкция CREATE EXTERNAL TABLE создает его. Затем используйте инструкцию INSERT INTO, чтобы экспортировать данные из локальной таблицы SQL Server во внешний источник данных. 

Результаты инструкции SELECT экспортируются в указанное расположение в заданном формате. Внешние файлы получают имена вида *ИДзапроса_дата_время_ИД.формат*, где *ИД* — это нарастающий идентификатор, а *формат* — это формат экспортированных данных. Например, имя одного из файлов может выглядеть так: QID776_20160130_182739_0.orc.

> [!NOTE]
> При экспорте данных в Hadoop или хранилище BLOB-объектов Azure с помощью PolyBase передаются только данные без имен столбцов (метаданных), как определено в команде CREATE EXTERNAL TABLE.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Новые представления каталога

В новых представлениях каталога, указанных ниже, отображаются внешние ресурсы.

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 Определить, является ли таблица внешней, можно с помощью `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>Дальнейшие действия  

Дополнительные сведения об устранении неполадок см. в статье [Устранение неполадок с PolyBase](../../relational-databases/polybase/polybase-troubleshooting.md).
