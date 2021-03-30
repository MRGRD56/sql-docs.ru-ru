---
title: Общие сведения о виртуализации данных с помощью PolyBase
description: PolyBase позволяет экземпляру SQL Server обрабатывать запросы Transact-SQL, которые считывают данные из внешних источников, таких как Hadoop и хранилище BLOB-объектов Azure.
ms.date: 03/23/2021
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
ms.custom: contperf-fy21q2
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: debeb0916d8ecb14a5b0f52726bed90f04429fca
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103301"
---
# <a name="introducing-data-virtualization-with-polybase"></a>Общие сведения о виртуализации данных с помощью PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

PolyBase — это компонент виртуализации данных для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

## <a name="what-is-polybase"></a>Что такое PolyBase?

PolyBase позволяет вашему экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запрашивать данные с помощью T-SQL непосредственно из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Oracle, Teradata, MongoDB, кластеров Hadoop, Cosmos DB без необходимости устанавливать клиентское программное обеспечение для подключения. Можно также использовать универсальный соединитель ODBC для подключения к дополнительным поставщикам с помощью сторонних драйверов ODBC. PolyBase позволяет с помощью запросов T-SQL объединить данные из внешних источников с данными из реляционных таблиц в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

Чаще всего виртуализация данных с помощью PolyBase используется для того, чтобы оставить данные в исходном расположении и формате. Вы можете виртуализировать внешние данные в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и запрашивать их на месте так же, как любую другую таблицу в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это минимизирует необходимость использовать процессы ETL для перемещения данных. Такой сценарий виртуализации данных можно реализовать с помощью соединителей PolyBase.

> [!NOTE]
> Некоторые функции компонента PolyBase предоставляются в закрытой предварительной версии для **Управляемых экземпляров SQL Azure**, включая возможность запрашивать внешние данные (файлы Parquet) в Azure Data Lake Storage (ADLS) 2-го поколения. Закрытая предварительная версия включает возможность доступа к клиентским библиотекам и документации для тестирования, которые пока что не предоставляются для общего доступа. Если вам интересно и вы готовы выделить некоторое время, чтобы опробовать новые функции, оставить свой отзыв и задать вопросы, ознакомьтесь с [руководством по закрытой предварительной версии PolyBase для Управляемого экземпляра SQL Azure](https://sqlmipg.blob.core.windows.net/azsqlpolybaseshare/Azure_SQL_Managed_Instance_Polybase_Private_Preview_Onboarding_Guide.pdf).

### <a name="supported-sql-products-and-services"></a>Поддерживаемые продукты и службы SQL

PolyBase предоставляет одинаковые функции для следующих продуктов SQL от корпорации Майкрософт:

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] и более поздние версии (только Windows);
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] и более поздние версии (Linux);
- Хранилище [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[pdw](../../includes/sspdw-md.md)] (PDW), размещенное в Analytics Platform System (APS). 
- [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]

### <a name="polybase-connectors"></a>Соединители PolyBase

 Компонент PolyBase обеспечивает подключение к следующим внешним источникам данных:

| Внешние источники данных     | [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с PolyBase | APS PDW    | [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] |
|---------------------------|--------------------------|------------|---------------|
| Oracle, MongoDB, Teradata | Чтение                     | **Нет**     | **Нет**        |  
| Универсальные данные ODBC              | Чтение (только Windows)      | **Нет**     | **Нет**        |  
| Хранилище Azure             | Чтение/запись               | Чтение/запись | Чтение/запись    |
| Hadoop                    | Чтение/запись               | Чтение/запись | **Нет**        |  
| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | Чтение                     | **Нет**     | **Нет**        |  
|                           |                          |            |               |


* В [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] представлен компонент PolyBase с поддержкой подключений к Hadoop и хранилищу BLOB-объектов Azure.
* В [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] представлены дополнительные соединители, в том числе для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Oracle, Teradata и MongoDB.

 В числе внешних соединителей PolyBase:

- [[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Hadoop](polybase-configure-hadoop.md)*

\* PolyBase поддерживает два поставщика Hadoop — Hortonworks Data Platform (HDP) и Cloudera Distributed Hadoop (CDH).

 Чтобы использовать PolyBase, в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сделайте следующее:

1. Установите PolyBase в [Windows](polybase-installation.md) или в [Linux](polybase-linux-setup.md).
1. Начиная с [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], может потребоваться [включить компонент PolyBase в sp_configure](polybase-installation.md#enable). 
1. Создайте [внешний источник данных](../../t-sql/statements/create-external-data-source-transact-sql.md).
1. Создайте [внешнюю таблицу](../../t-sql/statements/create-external-table-transact-sql.md).



### <a name="azure-integration"></a>Интеграция с Azure

Запросы T-SQL на основе PolyBase также можно использовать для импорта данных в хранилище BLOB-объектов Azure и экспорта данных из него. Кроме того, PolyBase позволяет [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] импортировать данные в Azure Data Lake Store и хранилище BLOB-объектов Azure, а также экспортировать данные из них.

## <a name="why-use-polybase"></a>Зачем нужна технология PolyBase

PolyBase позволяет объединять данные из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с внешними данными. Прежде чем PolyBase объединит данные во внешних источниках, можно выполнить одно из следующих действий:

- передать часть данных, чтобы все они находились в одном месте;
- запросить данные из двух источников, а затем написать пользовательскую логику запроса для объединения и интеграции данных на уровне клиента.

PolyBase позволяет легко объединять данные, используя Transact-SQL.

PolyBase не требует установки дополнительного программного обеспечения в среде Hadoop. При запросе внешних данных используется такой же синтаксис T-SQL, как и при запросе таблицы базы данных. Все вспомогательные действия, реализуемые PolyBase, выполняются прозрачно. Автору запроса не требуется знать, как работает внешний источник.

### <a name="polybase-uses"></a>Варианты использования PolyBase

PolyBase поддерживает следующие сценарии в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

- **Запрашивание данных, хранящихся в Hadoop, из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или PDW.** Пользователи хранят данные в более экономичных распределенных и масштабируемых системах, таких как Hadoop. PolyBase позволяет легко запрашивать данные с помощью T-SQL.

- **Запрашивание данных, хранящихся в хранилище BLOB-объектов Azure.** Данные для использования службами Azure удобно хранить в хранилище BLOB-объектов Azure. PolyBase позволяет легко обращаться к данным с помощью T-SQL.

- **Импорт данных из Hadoop, хранилища BLOB-объектов Azure или Azure Data Lake Storage.** Воспользуйтесь преимуществами технологии Microsoft SQL columnstore и возможностями анализа, импортируя данные из Hadoop, хранилища BLOB-объектов Azure или Azure Data Lake Storage в реляционные таблицы. Нет необходимости в отдельном средстве ETL или импорта.

- **Экспортировать данные в Hadoop, хранилище BLOB-объектов Azure или Azure Data Lake Storage.** Архивация данных в Hadoop, хранилище BLOB-объектов Azure или Azure Data Lake Storage позволяет создать экономичное хранилище и обеспечить его подключение к сети для удобного доступа к данным.

- **Интегрироваться со средствами бизнес-аналитики.** Можно использовать PolyBase со средствами бизнес-аналитики и стеком технологий анализа Майкрософт, а также применять любые сторонние средства, совместимые с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="performance"></a>Производительность

- **Принудительная отправка вычислений в Hadoop.** PolyBase отправляет некоторые вычисления во внешний источник, чтобы оптимизировать запрос в целом. Оптимизатор запросов принимает решение принудительно отправить вычисления в Hadoop, если это улучшит производительность запросов.  Для принятия такого решения оптимизатор запросов использует статистику из внешних таблиц. При включении вычислений создаются задания MapReduce и применяются распределенные вычислительные ресурсы Hadoop. Дополнительные сведения см. в статье [Вычисления pushdown в PolyBase](polybase-pushdown-computation.md). 

- **Масштабирование вычислительных ресурсов.** Для повышения производительности запросов можно использовать [группы горизонтального увеличения масштаба PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это обеспечивает параллельную передачу данных между экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и узлами Hadoop, а также добавляет вычислительные ресурсы для работы с внешними данными.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы начать использовать компонент PolyBase, его необходимо установить в [Windows](polybase-installation.md) или [Linux](polybase-linux-setup.md) и [включить в sp_configure](polybase-installation.md#enable) (если требуется). Затем ознакомьтесь со следующими руководствами по настройке в зависимости от источника данных:

- [Hadoop](polybase-configure-hadoop.md)
- [Хранилище BLOB-объектов Azure](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Универсальные типы ODBC](polybase-configure-odbc-generic.md)
