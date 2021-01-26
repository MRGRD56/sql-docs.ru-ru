---
title: Настройка параметра конфигурации сервера locks | Документы Майкрософт
description: Изучите параметр locks. Узнайте, как с его помощью можно ограничить объем памяти, используемый ядром СУБД SQL Server для блокировок.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- locks option [SQL Server]
ms.assetid: b0cf0f86-7652-4574-a9fb-908e10d03973
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a30c2cb2955ef82380bde00714098afec153ed95
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783698"
---
# <a name="configure-the-locks-server-configuration-option"></a>Настройка параметра конфигурации сервера locks
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описывается настройка параметра конфигурации сервера **locks** в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Параметр **locks** устанавливает максимальное число доступных блокировок, ограничивая таким образом объем памяти, используемый для них в компоненте [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Значение 0 (по умолчанию) позволяет компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] динамически выделять и освобождать структуры блокировок в зависимости от изменяющихся системных требований.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Задание параметра locks с помощью различных средств.**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После настройки параметра locks](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Это расширенный параметр, и изменять его следует только опытным администраторам баз данных или сертифицированным по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] специалистам.  
  
-   Когда сервер запускается с параметром **locks** , установленным в значение 0, диспетчер блокировок запрашивает у компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] объем памяти, достаточный для начального пула в 2500 структур блокировки. Если пул блокировки будет исчерпан, для пула будет запрошена дополнительная память.  
  
     Вообще, если требуется больше памяти, чем доступно для пула блокировки в пуле памяти компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , и доступно больше компьютерной памяти (пороговое значение **максимальная память сервера** не было достигнуто), компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] динамически распределяет память, чтобы удовлетворить потребности запроса блокировок. Однако, если распределение этой памяти вызовет подкачку страниц на уровне операционной системы (например, если другое приложение выполняется на том же самом компьютере как экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и использует эту память), большего объема пространства для блокировки не будет выделено. Динамический пул блокировки не получит больше чем 60 процентов памяти, выделенной для компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. После того как пул блокировки достигнет 60 процентов памяти, запрошенной экземпляром компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], или выяснится, что на компьютере нет больше доступной памяти, последующие запросы блокировок будут формировать ошибку.  
  
     Разрешение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использовать блокировки динамически является рекомендуемой конфигурацией. Однако можно установить параметр **locks** и отключить динамическое распределение ресурсов блокировок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Когда параметр **locks** имеет значение, отличное от 0, [!INCLUDE[ssDE](../../includes/ssde-md.md)] не может распределить больше блокировок, чем указано в этом параметре **locks**. Увеличьте это значение, если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отображает сообщение о том, что превышено количество доступных блокировок. Так как каждая блокировка занимает память (96 байт на блокировку), увеличение этого значения может потребовать увеличения объема памяти, выделенной для сервера.  
  
-   Параметр **locks** также оказывает влияние при укрупнении блокировки. Когда параметр **locks** установлен в 0, укрупнение блокировки происходит тогда, когда память, используемая текущими структурами блокировки, достигает 40 процентов от пула памяти компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Если параметр **locks** установлен не в значение 0, укрупнение блокировки происходит, когда количество блокировок достигает 40 процентов от значения, указанного для параметра **locks**.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Разрешения на выполнение хранимой процедуры **sp_configure** без параметров или только с первым параметром по умолчанию предоставляются всем пользователям. Для выполнения процедуры **sp_configure** с обоими параметрами для изменения параметра конфигурации или запуска инструкции RECONFIGURE необходимо иметь разрешение ALTER SETTINGS на уровне сервера. Разрешение ALTER SETTINGS неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-configure-the-locks-option"></a>Задание параметра locks  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите пункт **Свойства**.  
  
2.  Щелкните узел **Дополнительно** .  
  
3.  В разделе **Параллелизм** введите нужное значение параметра **locks** .  
  
     Используйте параметр **locks** для указания максимального числа доступных блокировок и, соответственно, для ограничения объема памяти, которую им выделяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-configure-the-locks-option"></a>Задание параметра locks  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере описывается использование процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) для присвоения параметру `locks` значения, позволяющего установить число блокировок, доступных всем пользователям, равным `20000`.  
  
```sql  
Use AdventureWorks2012 ;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'locks', 20000;  
GO  
RECONFIGURE;  
GO  
```  
  
 Дополнительные сведения см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-locks-option"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки параметра locks  
 Чтобы изменения вступили в силу, необходимо перезапустить сервер.  
  
## <a name="see-also"></a>См. также:  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
