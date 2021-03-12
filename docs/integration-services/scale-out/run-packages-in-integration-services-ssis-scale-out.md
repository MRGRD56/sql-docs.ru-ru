---
title: Выполнение пакетов в развертывании SQL Server Integration Services (SSIS) с горизонтальным увеличением масштаба | Документы Майкрософт
description: Узнайте о различных методах запуска пакетов SQL Server Integration Services (SSIS) в Scale Out.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
f1_keywords:
- sql13.ssis.ssms.ispackageexecuteinscaleout.f1
ms.openlocfilehash: 25f550f711364760de9cc07608b46532cfa34219
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839027"
---
# <a name="run-packages-in-integration-services-ssis-scale-out"></a>Выполнение пакетов в развертывании SQL Server Integration Services (SSIS) с горизонтальным увеличением масштаба

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


После развертывания пакетов на сервере служб Integration Services их можно запускать в развертывании с горизонтальным увеличением масштаба одним из следующих способов:

-   [Диалоговое окно "Выполнение пакета в сценарии горизонтального увеличения масштаба"](#scale_out_dialog)

-   [Хранимые процедуры](#stored_proc)

-   [задания агента SQL Server](#sql_agent)

## <a name="run-packages-with-the-execute-package-in-scale-out-dialog-box"></a><a name="scale_out_dialog"></a> Выполнение пакетов с помощью диалогового окна "Выполнение пакета в сценарии горизонтального увеличения масштаба"

1. Откройте диалоговое окно "Выполнение пакета в сценарии горизонтального увеличения масштаба".

    В [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)]подключитесь к серверу служб Integration Services. В обозревателе объектов разверните дерево для отображения узлов в разделе **Каталоги служб Integration Services**. Щелкните правой кнопкой мыши узел **SSISDB** либо проект или пакет, который требуется запустить, и выберите пункт **Execute in Scale Out**(Выполнить в развертывании с горизонтальным увеличением масштаба).

2. Выберите пакеты и задайте параметры.

    На странице **Выбор пакетов** выберите один или несколько пакетов для выполнения. Для каждого пакета задайте среду, параметры, диспетчеры подключений и дополнительные параметры. Чтобы задать эти параметры, щелкните пакет.
    
    На вкладке **Дополнительно** укажите, сколько раз будут совершаться попытки выполнения пакета в случае сбоя, задав параметр горизонтального увеличения масштаба **Число повторных попыток**.

    > [!NOTE]
    > Параметр **Дамп при ошибках** действует только в том случае, если учетная запись, с которой выполняется служба рабочей роли горизонтального увеличения масштаба, является администратором локального компьютера.

3. Выберите компьютеры рабочей роли.

    На странице **Выбор компьютера** выберите компьютеры рабочей роли горизонтального увеличения масштаба для выполнения пакетов. По умолчанию выполнять пакеты разрешено любому компьютеру. 

   > [!NOTE] 
   > Пакеты выполняются с учетными данными служб рабочих ролей горизонтального увеличения масштаба. Они приведены на странице **Выбор компьютера**. По умолчанию используется учетная запись `NT Service\SSISScaleOutWorker140`.

   > [!WARNING]
   > Пакеты, выполнение которых инициируется разными пользователями одной рабочей роли, запускаются с одними и теми же учетными данными. Границы безопасности между ними не устанавливаются. 

4. Выполните пакеты и просмотрите отчеты.

    Нажмите кнопку **ОК** , чтобы начать выполнение пакетов. Чтобы просмотреть для пакета отчет о выполнении, щелкните пакет правой кнопкой мыши в обозревателе объектов, выберите **Отчеты**, **Все выполнения** и найдите выполнение.
    
## <a name="run-packages-with-stored-procedures"></a><a name="stored_proc"></a> Выполнение пакетов с помощью хранимых процедур

1.  Создайте выполнения.

    Вызовите процедуру `[catalog].[create_execution]` для каждого пакета. Присвойте параметру **\@runinscaleout** значение `True`. Если выполнять пакет разрешено не всем компьютерам рабочей роли горизонтального увеличения масштаба, задайте для параметра **\@useanyworker** значение `False`. Дополнительные сведения об этой хранимой процедуре и параметре **\@useanyworker** см. в разделе [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md). 

2. Задайте параметры выполнения.

    Вызовите процедуру `[catalog].[set_execution_parameter_value]` для каждого выполнения.

3. Задайте рабочие роли горизонтального увеличения масштаба.

    Вызовите процедуру `[catalog].[add_execution_worker]`. Если выполнять пакет разрешено всем компьютерам, вызывать эту хранимую процедуру не требуется. 

4. Запустите выполнения.

    Вызовите процедуру `[catalog].[start_execution]`. Задайте параметр **\@retry_count**, указывающий, сколько раз будет предприниматься повторная попытка выполнения пакета в случае сбоя.
    
### <a name="example"></a>Пример
В приведенном ниже примере выполняются два пакета — `package1.dtsx` и `package2.dtsx` — в развертывании с горизонтальным увеличением масштаба с использованием одной рабочей роли.  

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder1', @project_name=N'project1', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO

Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package2.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder2', @project_name=N'project2', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO
```

### <a name="permissions"></a>Разрешения
Для выполнения пакетов в развертывании с горизонтальным увеличением масштаба требуется одно из следующих разрешений:

-   Членство в роли базы данных **ssis_admin**  

-   Членство в роли базы данных **ssis_cluster_executor**  
  
-   Членство в роли сервера **sysadmin**  

## <a name="set-default-execution-mode"></a>Установка режима выполнения по умолчанию
Чтобы задать для пакетов режим выполнения по умолчанию **Горизонтальное увеличение масштаба**, выполните указанные ниже действия.

1.  В SQL Server Management Studio в обозревателе объектов щелкните правой кнопкой мыши узел **SSISDB** и выберите пункт **Свойства**.

2.  В диалоговом окне **Свойства каталога** присвойте свойству **Серверный режим выполнения по умолчанию** значение **Горизонтальное увеличение масштаба**.

После задания этого режима выполнения по умолчанию больше не нужно указывать параметр **\@runinscaleout** при вызове хранимой процедуры `[catalog].[create_execution]`. Пакеты выполняются в развертывании с горизонтальным увеличением масштаба автоматически. 

![Режим выполнения](media/exe-mode.PNG)

Чтобы отключить выполнение в режиме горизонтального увеличения масштаба по умолчанию, присвойте параметру **Серверный режим выполнения по умолчанию** значение **Сервер**.

## <a name="run-package-in-sql-server-agent-job"></a><a name="sql_agent"></a> Выполнение пакета в рамках задания агента SQL Server
Пакет служб SSIS может выполняться как один из этапов задания агента SQL Server. Чтобы выполнить пакет в развертывании с горизонтальным увеличением масштаба, задайте **Горизонтальное увеличение масштаба** в качестве режима выполнения по умолчанию. После того, как настроен режим выполнения по умолчанию **Горизонтальное увеличение масштаба**, пакеты в заданиях агента SQL Server будут выполняться в режиме горизонтального увеличения масштаба.

> [!NOTE]
> Вы не можете остановить выполнение пакета в режиме горизонтального увеличения масштаба, отменив задание агента SQL Server. Для остановки горизонтального увеличения масштаба рекомендуется использовать хранимую процедуру catalog.stop_operation или область **Активные операции**. 

## <a name="next-steps"></a>Дальнейшие действия
-   [Устранение неполадок горизонтального увеличения масштаба](troubleshooting-scale-out.md)
