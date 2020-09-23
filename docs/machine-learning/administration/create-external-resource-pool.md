---
title: Создание пула ресурсов
description: Узнайте, как создать и использовать пул ресурсов для управления рабочими нагрузками Python и R в Службах машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 08f2c66fec80ce27e3e7a9ffca7a00194ff3b81b
ms.sourcegitcommit: 5da46e16b2c9710414fe36af9670461fb07555dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/01/2020
ms.locfileid: "89283768"
---
# <a name="create-a-resource-pool-for-sql-server-machine-learning-services"></a>Создание пула ресурсов для Служб машинного обучения SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Узнайте, как создать и использовать пул ресурсов для управления рабочими нагрузками Python и R в Службах машинного обучения SQL Server. 

Процесс состоит из нескольких шагов:

1. Просмотр состояния всех существующих пулов ресурсов. Важно понимать, какие службы используют существующие ресурсы.
2. Изменение пулов ресурсов сервера.
3. Создание пула ресурсов для внешних процессов.
4. Создание функции классификации для идентификации запросов внешних сценариев.
5. Проверка того, что новый внешний пул ресурсов получает задания R или Python от указанных клиентов или учетных записей.

<a name="bkmk_ReviewStatus"></a>

##  <a name="review-the-status-of-existing-resource-pools"></a>Просмотр состояния существующих пулов ресурсов
  
1.  Используйте следующую инструкцию, чтобы проверить ресурсы, назначенные в пул по умолчанию для сервера.
  
    ```sql
    SELECT * FROM sys.resource_governor_resource_pools WHERE name = 'default'
    ```

    **Пример результата**

    |pool_id|name|min_cpu_percent|max_cpu_percent|min_memory_percent|max_memory_percent|cap_cpu_percent|min_iops_per_volume|max_iops_per_volume|
    |-|-|-|-|-|-|-|-|-|
    |2|значение по умолчанию|0|100|0|100|100|0|0|

2.  Проверьте ресурсы, назначенные **внешнему** пулу ресурсов по умолчанию.
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pools WHERE name = 'default'
    ```

    **Пример результата**

    |external_pool_id|name|max_cpu_percent|max_memory_percent|max_processes|version|
    |-|-|-|-|-|-|
    |2|значение по умолчанию|100|20|0|2|
 
3.  При таких параметрах сервера по умолчанию внешней среде выполнения, вероятно, будет недостаточно ресурсов для выполнения большинства задач. Чтобы улучшить ресурсы, необходимо изменить использование ресурсов сервера следующим образом.
  
    -   уменьшить максимальный объем памяти компьютера, который может использоваться ядром СУБД;
  
    -   увеличить максимальный объем памяти компьютера, который может использоваться внешними процессами.

## <a name="modify-server-resource-usage"></a>Изменение использования ресурсов сервера

1.  В [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] выполните следующую инструкцию, чтобы ограничить объем используемой сервером [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] памяти, задав для параметра максимальной памяти сервера значение **60 %** .

    ```sql
    ALTER RESOURCE POOL "default" WITH (max_memory_percent = 60);
    ```
  
2. Выполните следующую инструкцию, чтобы ограничить использование памяти внешними процессами до **40 %** общих ресурсов компьютера.
  
    ```sql
    ALTER EXTERNAL RESOURCE POOL "default" WITH (max_memory_percent = 40);
    ```
  
3.  Чтобы применить эти изменения, необходимо перенастроить и перезапустить регулятор ресурсов следующим образом:
  
    ```sql
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    ```
  
    > [!NOTE]
    >  Это базовые рекомендуемые параметры. Следует оценить задачи машинного обучения в свете других серверных процессов, чтобы определить правильный баланс для вашей среды и рабочей нагрузки.

## <a name="create-a-user-defined-external-resource-pool"></a>Создание внешнего пула ресурсов, определяемого пользователем
  
1.  Все изменения конфигурации Resource Governor применяются к серверу в целом. Изменения затрагивают рабочие нагрузки, использующие пулы по умолчанию для сервера, а также рабочие нагрузки, использующие внешние пулы.
  
     Чтобы обеспечить более точный контроль над тем, какие рабочие нагрузки должны иметь приоритет, можно создать новый внешний пул ресурсов, определяемый пользователем. Определите функцию классификации и присвойте ее пулу внешних ресурсов. Ключевое слово **EXTERNAL** является новым.
  
     Создание нового *внешнего пула ресурсов, определяемого пользователем*. В следующем примере пулу присваивается имя **ds_ep**.
  
    ```sql
    CREATE EXTERNAL RESOURCE POOL ds_ep WITH (max_memory_percent = 40);
    ```

2.  Создайте группу рабочей нагрузки с именем `ds_wg`, которая будет использоваться для управления запросами сеанса. Для SQL-запросов будет использоваться пул по умолчанию. Для всех запросов внешних процессов будет использоваться пул `ds_ep`.
  
    ```sql
    CREATE WORKLOAD GROUP ds_wg WITH (importance = medium) USING "default", EXTERNAL "ds_ep";
    ```
  
     Запросы назначаются группе по умолчанию всякий раз, когда невозможно классифицировать запрос, или при любой другой ошибке классификации.

 
Дополнительные сведения см. в статьях [Resource Governor Workload Group](../../relational-databases/resource-governor/resource-governor-workload-group.md) (Группа рабочей нагрузки регулятора ресурсов) и [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md) (Инструкция CREATE WORKLOAD GROUP (Transact-SQL)).
  
## <a name="create-a-classification-function-for-machine-learning"></a>Создание функции классификации для машинного обучения
  
Функция классификации проверяет входящие задачи. Она определяет, можно ли запустить задачу с использованием текущего пула ресурсов. Задачи, которые не отвечают критериям функции классификации, назначаются обратно в пул ресурсов по умолчанию на сервере.
  
1. Начните с указания, что регулятор ресурсов должен использовать функцию классификации для определения пулов ресурсов. В качестве заполнителя для функции классификации можно назначить значение **null**.
  
    ```sql
    ALTER RESOURCE GOVERNOR WITH (classifier_function = NULL);
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    ```
  
     Дополнительные сведения см. в разделе [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md).
  
2.  В функции классификации для каждого пула ресурсов можно определить тип инструкций или входящих запросов, которые должны быть назначены пулу ресурсов.
  
     Например, следующая функция возвращает имя схемы, назначенной внешнему пулу ресурсов, определяемому пользователем, если приложением, отправившим запрос, является Microsoft R Host, RStudio или Mashup. В противном случае возвращается пул ресурсов по умолчанию.
  
    ```sql
    USE master
    GO
    CREATE FUNCTION is_ds_apps()
    RETURNS sysname
    WITH schemabinding
    AS
    BEGIN
        IF program_name() in ('Microsoft R Host', 'RStudio', 'Mashup') RETURN 'ds_wg';
        RETURN 'default'
        END;
    GO
    ```
  
3.  После создания функции перенастройте группу ресурсов, чтобы назначить новую функцию классификации внешней группе ресурсов, определенной ранее.
  
    ```sql
    ALTER RESOURCE GOVERNOR WITH  (classifier_function = dbo.is_ds_apps);
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    GO
    ```

## <a name="verify-new-resource-pools-and-affinity"></a>Проверка новых пулов ресурсов и сопоставления

Проверьте конфигурацию памяти и ЦП сервера для каждой группы рабочих нагрузок. Проверьте, были ли изменены ресурсы экземпляра, изучив следующее.

+ пул по умолчанию для сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];
+ пул ресурсов по умолчанию для внешних процессов;
+ пользовательский пул для внешних процессов.

1. Выполните следующую инструкцию, чтобы просмотреть все группы рабочей нагрузки:

    ```sql
    SELECT * FROM sys.resource_governor_workload_groups;
    ```

    **Образец результатов**

    |group_id|name|importance|request_max_memory_grant_percent|request_max_cpu_time_sec|request_memory_grant_timeout_sec|max_dop|group_max_requests pool_id|pool_idd|external_pool_id|
    |-|-|-|-|-|-|-|-|-|-|
    |1|internal|Средний|25|0|0|0|0|1|2|
    |2|default|Средний|25|0|0|0|0|2|2|
    |256|ds_wg|Средний|25|0|0|0|0|2|256|
  
2.  Чтобы просмотреть все внешние пулы ресурсов, используйте новое представление каталога [sys.resource_governor_external_resource_pools (Transact-SQL)](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md).
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pools;
    ```

    **Образец результатов**
    
    |external_pool_id|name|max_cpu_percent|max_memory_percent|max_processes|version|
    |-|-|-|-|-|-|
    |2|default|100|20|0|2|
    |256|ds_ep|100|40|0|1|
  
     Дополнительные сведения см. в статье [Resource Governor Catalog Views (Transact-SQL)](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md) (Представления каталога регулятора ресурсов (Transact-SQL)).
  
3.  Выполните следующую инструкцию, чтобы получить сведения о ресурсах компьютеров, сопоставленных с внешним пулом ресурсов, если таковые имеются.
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pool_affinity;
    ```
  
     Сведения не будут отображаться, так как пулы были созданы со сходством в режиме AUTO. Дополнительные сведения см. в статье о [представлении sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pool-affinity-transact-sql.md).

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения об управлении ресурсами сервера см. в следующих статьях:

+ [Регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md) 
+ [Динамические административные представления регулятора ресурсов (Transact-SQL)](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

Общие сведения об управлении ресурсами для машинного обучения см. в следующих статьях:

+ [Управление рабочими нагрузками Python и R с помощью Resource Governor в службах машинного обучения SQL Server](resource-governor.md)
