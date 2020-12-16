---
title: Обновление служб Data Quality Services | Документы Майкрософт
description: Эта статья содержит сведения о том, как обновить существующую установку служб SQL Server Data Quality Services (DQS).
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: f396666b-7754-4efc-9507-0fd114cc32d5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1be8d8e6b320af96bc3acf7a28ec2c017a92914a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480325"
---
# <a name="upgrade-data-quality-services"></a>Обновление служб Data Quality Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Эта статья содержит сведения о том, как обновить существующую установку служб [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Services (DQS). В процессе обновления сервера служб DQS в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] необходимо обновить схему базы данных служб DQS.  
  
> [!IMPORTANT]
>  -   Необходимо создать резервную копию баз данных DQS, прежде чем обновлять DQS, чтобы предотвратить любую случайную потерю данных при обновлении схемы. Дополнительные сведения о создании резервной копии баз данных DQS см. в разделе [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
> -   Для выполнения задач по обеспечению качества данных вы можете подключиться к серверу DQS [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], используя текущую или более раннюю версию клиента DQS или [преобразование "Очистка DQS"](../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md) в службах Integration Services.  
> -   После обновления служб Data Quality Services и Master Data Services более ранние версии надстройки служб Master Data Services для Excel больше не будут работать. Можно скачать надстройку служб Master Data Services для Excel версии [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] по [этой ссылке](../../master-data-services/master-data-services-installation-and-configuration.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Необходимо выполнить вход от имени члена группы администраторов на компьютере [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   Учетная запись пользователя Windows должна входить в предопределенную роль сервера sysadmin на экземпляре SQL Server, где установлен сервер [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
##  <a name="upgrading-dqs"></a><a name="Upgrade"></a> Обновление DQS  
 Обновление DQS:  
  
1.  Создайте резервные копии баз данных DQS перед началом процесса обновления. Дополнительные сведения о создании резервной копии баз данных DQS см. в разделе [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
2.  Обновите экземпляр SQL Server, на котором установлен компонент DQS.  
  
    1.  Запустите мастер установки [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    2.  На панели слева щелкните **Установка**.  
  
    3.  В области справа щелкните **Обновление с предыдущей версии SQL Server**.  
  
    4.  Завершите работу мастера установки.  
  
        > [!NOTE]  
        >  Это обновит экземпляр SQL Server в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , а также задает последнюю Data Quality Client, если клиент DQS был ранее установлен на этом компьютере. При наличии клиента служб DQS установлены на других компьютерах, необходимо выполнить шаги по в шаге 2 на этих компьютерах, чтобы установить текущую версию клиента служб DQS. Мастер установки установит текущую версию служб Data Quality Client вместе с текущей версией клиента служб DQS. После обновления схемы баз данных DQS можно подключиться к [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] версию сервера служб DQS с помощью текущего или более ранней версией клиента служб DQS.  
  
3.  Обновите схему баз данных DQS.  
  
    1.  Откройте командную строку от имени администратора.  
  
    2.  В командной строке перейдите в папку, где находится файл DQSInstaller.exe. Для экземпляра SQL Server по умолчанию файл DQSInstaller.exe будет находиться в папке C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn:  

        >[!NOTE]
        >В пути к папке замените [nn] на номер версии [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].
        >- Для SQL Server 2016: 13
        >- Для SQL Server 2017: 14
    
        ```console
        cd C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn  
        ```  
  
    3.  В командной строке введите следующую команду и нажмите клавишу ВВОД:  
  
        ```console
        dqsinstaller.exe -upgrade  
        ```  
  
    4.  Установщик предложит создать резервную копию базы данных DQS, прежде чем продолжить. Если резервное копирование баз данных DQS уже выполнено, введите **Y** или **Yes** и нажмите клавишу ВВОД, чтобы продолжить обновление.  
  
    5.  После успешного обновления схемы баз данных DQS отображается сообщение о завершении.  
  
##  <a name="verifying-the-dqs-databases-schema-upgrade"></a><a name="Verify"></a> Проверка обновления баз данных DQS схемы  
 Для проверки, что схема баз данных DQS обновлена успешно, можно проверить в текущей версии базы данных DQS_MAIN и DQS_PROJECTS с помощью запроса к таблице A_DB_VERSION в каждой базе данных. Для этого:  
  
1.  Запустите среду SQL Server Management Studio и установите соединение с экземпляром SQL Server, содержащий обновленные базы данных DQS схемы.  
  
2.  Выполните следующий запрос:  
  
    ```sql
    SELECT * FROM DQS_MAIN.dbo.A_DB_VERSION WHERE STATUS=2;  
    SELECT * FROM DQS_PROJECTS.dbo.A_DB_VERSION WHERE STATUS=2;  
    ```  
  
3.  Выводится запись для каждой операции обновления, а также дата создания обновления. Максимальное VERSION_ID и ASSEMBLY_VERSION на самой последней даты текущую версию. Значение 2 в столбце STATUS означает успешное выполнение процедуры. Если возникла ошибка, то ОШИБКА перечисляются в столбец. Образец вывода:  
  
    |ID|UPGRADE_DATE|VERSION_ID|ASSEMBLY_VERSION|USER_NAME|Состояние|ошибка|  
    |--------|-------------------|-----------------|-----------------------|----------------|------------|-----------|  
    |1000|2013-08-11 05:26:39.567|1200|11.0.3000.0|\<DOMAIN\UserName>|2||  
    |1001|2013-09-19 15:09:37.750|1600|12.0.xxxx.0|\<DOMAIN\UserName>|2||  
  
## <a name="see-also"></a>См. также:  
 [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Удаление объектов сервера служб Data Quality](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Обновление SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
