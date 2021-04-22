---
title: 'Доступ к внешним данным: Hadoop — PolyBase'
description: В статье используется PolyBase на экземпляре SQL Server с Hadoop. PolyBase подходит для специализированных запросов к внешним таблицам и импорта и экспорта данных.
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: 3b56ae68b582916debfdaf4696823422940887f3
ms.sourcegitcommit: 554497d604e0c63c055bf6d572d92fdadb027dbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107571379"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>Настройка PolyBase для доступа к внешним данным в Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

В этой статье описывается использование PolyBase в экземпляре SQL Server для запроса внешних данных в Hadoop.

## <a name="prerequisites"></a>Предварительные требования

- Если вы не установили PolyBase, см. раздел [Установка PolyBase](polybase-installation.md). Необходимые условия описываются в статье, посвященной установке.

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- Начиная с SQL Server 2019, необходимо также [включать компонент PolyBase](polybase-installation.md#enable).

::: moniker-end

- PolyBase поддерживает два поставщика Hadoop — Hortonworks Data Platform (HDP) и Cloudera Distributed Hadoop (CDH). В новых выпусках Hadoop соблюдается шаблон "Основной номер версии.дополнительный номер версии.версия". Также поддерживаются все версии в рамках поддерживаемых основного и дополнительного выпусков. Поддерживаются следующие поставщики Hadoop:

  - Hortonworks HDP 1.3, 2.1–2.6, 3.0 в Linux
  - Hortonworks HDP 1.3, 2.1–2.3, 3.1<sup>*</sup> в Window Server
  - Cloudera CDH 4.3, 5.1–5.5, 5.9–5.13 в Linux

   <sup>*</sup> Для Hortonworks HDB 3.1 требуется SQL Server 2019 с накопительным пакетом обновления 9 (CU9, версия 15.0.4102) или более поздняя версия.

> [!NOTE]
> PolyBase поддерживает зоны шифрования Hadoop начиная с SQL Server 2016 с пакетом обновления 1 (SP1) и накопительным пакетом обновления 7, а также с SQL Server 2017 с накопительным пакетом обновления 3. Если вы используете [масштабируемые группы PolyBase](polybase-scale-out-groups.md), все вычислительные узлы также должны находиться в сборке, которая включает в себя поддержку зон шифрования Hadoop.

### <a name="configure-hadoop-connectivity"></a>Настройка подключения к Hadoop

Сначала необходимо настроить SQL Server PolyBase для использования определенного поставщика Hadoop.

1. Запустите [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) с hadoop connectivity и задайте соответствующее значение для поставщика. Значение для поставщика см. в статье [Конфигурация подключения к PolyBase (Transact-SQL)](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). По умолчанию для подключения к Hadoop установлено значение 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Перезапустите SQL Server с помощью **services.msc**. При перезапуске SQL Server следующие службы также будут перезапущены:  

   - Служба перемещения данных SQL Server PolyBase  
   - Компонент SQL Server PolyBase Engine  
  
   ![остановка и запуск служб PolyBase в services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "остановка и запуск служб PolyBase в services.msc")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Активация вычислений pushdown  

Чтобы улучшить производительность при выполнении запроса, активируйте вычисление pushdown для кластера Hadoop.  
  
1. Найдите файл **yarn-site.xml** в каталоге установки SQL Server. Как правило, путь выглядит следующим образом:  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. Найдите аналогичный файл на компьютере с Hadoop в каталоге конфигурации. Открыв его, найдите и скопируйте значение ключа конфигурации yarn.application.classpath.  
  
1. На компьютере SQL Server в файле **yarn-site.xml** найдите свойство **yarn.application.classpath**. Вставьте значение, скопированное на компьютере с Hadoop, в качестве значения элемента.  
  
1. Для всех версий CDH 5.X необходимо добавить параметры конфигурации mapreduce.application.classpath либо в конец файла yarn-site.xml, либо в файл mapred-site.xml. HortonWorks содержит эти настройки в конфигурациях yarn.application.classpath. Примеры см. в статье [Конфигурация PolyBase](../../relational-databases/polybase/polybase-configuration.md).

## <a name="configure-an-external-table"></a>Настройка внешней таблицы

Чтобы запросить данные из источника данных Hadoop, необходимо определить внешнюю таблицу для использования в запросах Transact-SQL. Далее указаны шаги по настройке внешней таблицы.

1. Создайте в базе данных главный ключ, если его нет. Это необходимо для шифрования секрета учетных данных.

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>Аргументы
    PASSWORD ='password'

    Пароль, который использовался при шифровке главного ключа базы данных. Аргумент password должен соответствовать требованиям политики паролей Windows на компьютере, где размещается экземпляр SQL Server.
1. Создайте учетные данные на уровне базы данных для кластеров Hadoop, защищенных с помощью Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. Создайте внешний источник данных с помощью инструкции [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

3. Создайте формат внешнего файла с помощью инструкции [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. Создайте внешнюю таблицу, указывающую на данные, хранящиеся в Hadoop, с помощью инструкции [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). В этом примере внешние данные представляют собой данные датчика автомобиля.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

5. Создайте статистику внешней таблицы.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Запросы PolyBase

Есть три функции, которые выполняет PolyBase:  
  
- отправка нерегламентированных запросов к внешним таблицам;  
- импорт данных;  
- экспорт данных.  

Следующие запросы предоставляют пример с вымышленными данными датчика автомобиля.

### <a name="ad-hoc-queries"></a>Нерегламентированные запросы  

Следующий нерегламентированный запрос объединяет реляционные данные с данными Hadoop. Он выбирает клиентов, которые ездят быстрее 35 миль/ч, объединяя структурированные данные клиента, хранящиеся в SQL Server, с данными автомобильного датчика, хранящимися в Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>импорт данных  

Следующий запрос позволяет импортировать внешние данные в SQL Server. В этом примере импортируются данные быстрых водителей в SQL Server для выполнения углубленного анализа. Для повышения производительности здесь используется технология columnstore.  

```sql
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

### <a name="exporting-data"></a>Экспорт данных  

Следующий запрос позволяет экспортировать данные из SQL Server в Hadoop. Чтобы сделать это, необходимо сначала включить экспорт PolyBase. Создайте внешнюю целевую таблицу, прежде чем экспортировать в нее данные.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
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
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssms"></a>Просмотр объектов PolyBase в SSMS  

В SSMS внешние таблицы отображаются в отдельной папке **Внешние таблицы**. Внешние источники данных и форматы внешних файлов находятся в папках, вложенных в папку **Внешние ресурсы**.  
  
![Объекты PolyBase в SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Дальнейшие действия

В следующих статьях приведены дополнительные сведения о способах использования и мониторинга PolyBase.

[Масштабируемые группы PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)  
[Устранение неполадок c PolyBase](polybase-troubleshooting.md)  
