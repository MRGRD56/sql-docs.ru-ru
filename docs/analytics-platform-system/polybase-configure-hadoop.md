---
title: 'Доступ к внешним данным: Hadoop — PolyBase'
description: Объясняется, как настроить Polybase в Parallel Data Warehouse для подключения к внешним Hadoop.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 18bec3d694985e15b1ae2d813fddf2f378c9d78a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049110"
---
# <a name="configure-polybase-in-parallel-data-warehouse-to-access-external-data-in-hadoop"></a>Настройка Polybase в параллельном хранилище данных для доступа к внешним данным в Hadoop

В этой статье объясняется, как использовать Polybase на устройстве APS для запроса внешних данных в Hadoop.

## <a name="prerequisites"></a>Предварительные требования

PolyBase поддерживает два поставщика Hadoop — Hortonworks Data Platform (HDP) и Cloudera Distributed Hadoop (CDH). В новых выпусках Hadoop соблюдается шаблон "Основной номер версии.дополнительный номер версии.версия". Также поддерживаются все версии в рамках поддерживаемых основного и дополнительного выпусков. Поддерживаются следующие поставщики Hadoop:
 - Hortonworks HDP 1.3 в ОС Linux или Windows Server;  
 - Hortonworks HDP 2,1-2,6 в Linux
 - Hortonworks HDP 3,0-3,1 в Linux
 - Hortonworks HDP 2.1–2.3 в ОС Windows Server;  
 - Cloudera CDH 4.3 в Linux;  
 - Cloudera CDH 5,1-5,5, 5,9-5,13, 5,15 & 5,16 в Linux

### <a name="configure-hadoop-connectivity"></a>Настройка подключения к Hadoop

Сначала настройте ТД для использования конкретного поставщика Hadoop.

1. Запустите [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) с hadoop connectivity и задайте соответствующее значение для поставщика. Значение для поставщика см. в статье [Конфигурация подключения к PolyBase (Transact-SQL)](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). 

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 and 3.0 - 3.1 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Перезапустите регион APS с помощью страницы состояния службы [Configuration Manager устройства](launch-the-configuration-manager.md).
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Включить вычисления включение  

Чтобы улучшить производительность при выполнении запроса, активируйте вычисление pushdown для кластера Hadoop.  
  
1. Откройте подключение к удаленному рабочему столу на узле управления PDW.

2. Найдите файл **yarn-site.xml** на управляющем узле. Как правило, путь выглядит следующим образом:  

   ```xml  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf\  
   ```  

3. Найдите аналогичный файл на компьютере с Hadoop в каталоге конфигурации. Открыв его, найдите и скопируйте значение ключа конфигурации yarn.application.classpath.  
  
4. На управляющем узле в **файлеyarn.site.xml** найдите свойство **Yarn. Application. подкаталог** . Вставьте значение, скопированное на компьютере с Hadoop, в качестве значения элемента.  
  
5. Для всех версий CDH 5.X необходимо добавить параметры конфигурации mapreduce.application.classpath либо в конец файла yarn.site.xml, либо в файл mapred-site.xml. HortonWorks содержит эти настройки в конфигурациях yarn.application.classpath. Примеры см. в статье [Конфигурация PolyBase](../relational-databases/polybase/polybase-configuration.md).

## <a name="example-xml-files-for-cdh-5x-cluster-default-values"></a>Пример XML-файлов для значений по умолчанию для кластера CDH 5. X

Yarn-site.xml yarn.application.classpath и mapreduce.application.classpath конфигурации.

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

Если вы решили разбить два параметра конфигурации на mapred-site.xml и yarn-site.xml, то файлы будут выглядеть следующим образом:

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

Обратите внимание, что было добавлено свойство mapreduce.application.classpath. В CDH 5. x значения конфигурации можно найти по тому же соглашению об именовании в Ambari.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="example-xml-files-for-hdp-3x-cluster-default-values"></a>Пример XML-файлов для значений по умолчанию для кластера HDP 3. X

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
  <property>
     <name>yarn.resourcemanager.connect.max-wait.ms</name>
     <value>40000</value>
  </property>
  <property>
     <name>yarn.resourcemanager.connect.retry-interval.ms</name>
     <value>30000</value>
  </property>
<!-- Applications' Configuration-->
  <property>
    <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
     <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
     <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
     <name>yarn.application.classpath</name>
     <value>$HADOOP_CONF_DIR,/usr/hdp/3.1.0.0-78/hadoop/*,/usr/hdp/3.1.0.0-78/hadoop/lib/*,/usr/hdp/current/hadoop-hdfs-client/*,/usr/hdp/current/hadoop-hdfs-client/lib/*,/usr/hdp/current/hadoop-yarn-client/*,/usr/hdp/current/hadoop-yarn-client/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/lib/*,/usr/hdp/share/hadoop/common/*,/usr/hdp/share/hadoop/common/lib/*,/usr/hdp/share/hadoop/tools/lib/*</value>
  </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
     <name>yarn.resourcemanager.principal</name>
     <value></value>
  </property>
-->
</configuration>
```

## <a name="configure-an-external-table"></a>Настройка внешней таблицы

Чтобы запросить данные из источника данных Hadoop, необходимо определить внешнюю таблицу для использования в запросах Transact-SQL. Далее указаны шаги по настройке внешней таблицы.

1. Создайте главный ключ в базе данных. Необходимо зашифровать секрет учетных данных.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

2. Создайте учетные данные на уровне базы данных для кластеров Hadoop, защищенных с помощью Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

3. Создайте внешний источник данных с помощью инструкции [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md).

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

4. Создайте формат внешнего файла с помощью инструкции [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE)  
   ```

5. Создайте внешнюю таблицу, указывающую на данные, хранящиеся в Hadoop, с помощью инструкции [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md). В этом примере внешние данные представляют собой данные датчика автомобиля.

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

6. Создайте статистику внешней таблицы.

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

Следующий нерегламентированный запрос объединяет реляционные данные с данными Hadoop. Он выбирает клиентов, которые работают быстрее, чем 35 миль/ч, объединяя структурированные данные клиентов, хранящиеся в ТД, с данными датчика автомобиля, хранящимися в Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>импорт данных  

Следующий запрос импортирует внешние данные в ТД. В этом примере данные для быстрых драйверов импортируются в ТД для более глубокого анализа. Для повышения производительности она использует технологию columnstore в ТД.  

```sql
CREATE TABLE Fast_Customers
WITH
(CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH (CustomerKey))
AS
SELECT DISTINCT
      Insured_Customers.CustomerKey, Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
```  

### <a name="exporting-data"></a>Экспорт данных  

Следующий запрос экспортирует данные из ТД в Hadoop. Он может использоваться для архивации реляционных данных в Hadoop, в то время как они по-прежнему могут выполнять запросы.

```sql
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] 
WITH (  
      LOCATION='/archive/customer/2009',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat
)  
AS
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssdt"></a>Просмотр объектов Polybase в SSDT  

В SQL Server Data Tools внешние таблицы отображаются в отдельных папках **внешние таблицы**. Внешние источники данных и форматы внешних файлов находятся в папках, вложенных в папку **Внешние ресурсы**.  
  
![Объекты Polybase в SSDT](media/polybase/external-tables-datasource.png)  

## <a name="next-steps"></a>Дальнейшие действия

Параметры безопасности Hadoop см. в статье [Настройка безопасности Hadoop](polybase-configure-hadoop-security.md).<br>
Дополнительные сведения о PolyBase см. в [этом руководстве](../relational-databases/polybase/polybase-guide.md). 
 
