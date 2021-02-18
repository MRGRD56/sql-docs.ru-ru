---
title: Сохранение и загрузка объектов R с помощью ODBC
description: Пакет RevoScaleR содержит функции сериализации и десериализации, призванные значительно повысить производительность и обеспечить более компактное хранение объектов.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/15/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 379c6cc1d865b2820026948b352abb07b27eda52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273025"
---
# <a name="save-and-load-r-objects-from-sql-server-using-odbc"></a>Сохранение и загрузка объектов R из SQL Server с помощью ODBC
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Службы R Services SQL Server могут хранить сериализованные объекты R в таблице и затем загружать их оттуда по мере необходимости, при этом им не требуется перезапускать код R или переобучать модель. Такая возможность сохранения объектов R в базе данных крайне важна для таких сценариев, как обучение и сохранение модели, а также последующее ее использование для оценки или анализа.

Для повышения производительности этого критического этапа пакет **RevoScaleR** теперь содержит новые функции сериализации и десериализации, призванные значительно повысить производительность и обеспечить более компактное хранение объектов. Эта статья описывает эти функции и способы их использования.

## <a name="overview"></a>Обзор

Теперь пакет **RevoScaleR** включает новые функции, которые упрощают сохранение объектов R в SQL Server и последующее их считывание из таблицы SQL Server. В общем случае каждый вызов функции использует простое хранилище значений ключей, в котором ключ является именем объекта, а значение, сопоставленное с этим ключом, является объектом R типа varbinary, перемещаемым в таблицу или из нее.

Чтобы сохранить объекты R напрямую в SQL Server непосредственно из среды R, нужно:

+ установить подключение к SQL Server с помощью источника данных *RxOdbcData*;
+ вызвать новые функции через подключение ODBC;
+ при необходимости указать, что объект не сериализуется, а затем выбрать новый алгоритм сжатия для использования вместо алгоритма по умолчанию.

По умолчанию любой объект, вызываемый из R для перемещения в SQL Server, сериализуется и сжимается. И наоборот, при загрузке объекта из таблицы SQL Server для использования в коде R объект десериализуется и распаковывается.

## <a name="list-of-new-functions"></a>Список новых функций

- `rxWriteObject` записывает объект R в SQL Server с помощью источника данных ODBC.

- `rxReadObject` считывает объект R из базы данных SQL Server с помощью источника данных ODBC.

- `rxDeleteObject` удаляет объект R из базы данных SQL Server, указанной в источнике данных ODBC. Если существует несколько объектов, на которые указывает сочетание ключа и версии, все они удаляются.

- `rxListKeys` указывает все доступные объекты в виде пар "ключ — значение". Это помогает определить имена и версии объектов R.

Для получения подробных сведений о синтаксисе каждой функции воспользуйтесь справкой R. Позже подробные сведения также приведены в статье [ScaleR reference](/r-server/r-reference/revoscaler/revoscaler) (Справочник по ScaleR).

## <a name="how-to-store-r-objects-in-sql-server-using-odbc"></a>Сохранение объектов R в SQL Server с помощью ODBC

Эта процедура демонстрирует, как использовать новые функции для создания модели и ее сохранения в SQL Server.

1. Настройте строку подключения для SQL Server.
   ```R
   conStr <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Создайте объект источника данных *rxOdbcData* в R с помощью этой строки подключения.
   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr)
   ```

3. Удалите таблицу, если она уже существует и не требуется отслеживать старые версии объектов.

   ```R
   if(rxSqlServerTableExists(ds@table, ds@connectionString)) {
       rxSqlServerDropTable(ds@table, ds@connectionString)
       }
   ```
   
4. Определите таблицу, которую можно использовать для хранения двоичных объектов.

   ```R
   ddl <- paste(" CREATE TABLE [", ds@table, "] 
      (","  [id] varchar(200) NOT NULL,
       "," [value] varbinary(max),
       "," CONSTRAINT unique_id UNIQUE (id))", 
       sep = "") 
   ```
5. Откройте подключение ODBC, чтобы создать таблицу, а после завершения инструкции DDL закройте его.

   ```R
    rxOpen(ds, "w") 
    rxExecuteSQLDDL(ds, ddl) 
    rxClose(ds)
    ```
6. Создайте объекты R, которые вы хотите сохранить.

   ```R
   infertLogit <- rxLogit(case ~ age + parity + education + spontaneous + induced, 
     data = infert)
   ```
6. Используйте созданный ранее объект *RxOdbcData* , чтобы сохранить модель в базе данных.

   ```R
   rxWriteObject(ds, "logit.model", infertLogit)
   ```

## <a name="how-to-read-r-objects-from-sql-server-using-odbc"></a>Считывание объектов R из SQL Server с помощью ODBC

Эта процедура демонстрирует, как использовать новые функции для загрузки модели из SQL Server.

1. Настройте строку подключения для SQL Server.

   ```R
   conStr2 <- 'Driver={SQL Server};Server=localhost;Database=storedb;Trusted_Connection=true'
   ```
2. Создайте объект источника данных *rxOdbcData* в R с помощью этой строки подключения.

   ```R
   ds <- RxOdbcData(table="robjects", connectionString=conStr2)
   ```
3. Считайте модель из таблицы, указав имя ее объекта R.

   ```R
    infertLogit2 <- rxReadObject(ds, "logit.model")
   ```