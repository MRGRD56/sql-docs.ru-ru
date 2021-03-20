---
title: Настройка параметра конфигурации сервера user connections | Документы Майкрософт
description: Сведения о параметре user connections. Узнайте, как с его помощью можно избежать перегрузки экземпляра SQL Server из-за слишком большого числа одновременных подключений.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- simultaneous connections [SQL Server]
- user connections option [SQL Server]
- users [SQL Server], simultaneous connections
- maximum number of simultaneous user connections
- connections [SQL Server], simultaneous
ms.assetid: 53beee6e-59fe-4276-9abb-8f1cec2a3508
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5a6e6b2a47151a967980b6e75766bdec39e88ae
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673781"
---
# <a name="configure-the-user-connections-server-configuration-option"></a>Настройка параметра конфигурации сервера user connections
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описывается задание параметра конфигурации сервера **user connections** в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Параметр **user connections** указывает максимальное количество одновременных пользовательских соединений, допустимых в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Допустимое количество пользовательских соединений также зависит от используемой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и возможностей приложений или оборудования. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] допускает максимально 32 767 одновременных соединений пользователей. Поскольку параметр **user connections** является динамическим (самонастраиваемым), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически регулирует максимальное число пользовательских соединений по мере необходимости, до достижения максимального значения. Например, если к серверу подключилось 10 пользователей, будет выделено 10 объектов пользовательских соединений. В большинстве случаев не требуется изменять значение этого параметра. По умолчанию его значение равно нулю, то есть разрешено  максимальное число соединений (32 767).  
  
 Чтобы определить максимальное количество пользовательских соединений, допустимых в системе, можно выполнить команду [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) или воспользоваться запросом представления каталога [sys.configuration](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Настройка параметра пользовательских соединений с помощью следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После настройки параметра user connections](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Это расширенный параметр, и изменять его следует только опытным администраторам баз данных или сертифицированным по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] специалистам.  
  
-   Параметр **user connections** позволяет избежать перегрузки сервера в ситуации, когда слишком много пользователей одновременно пытаются соединиться с системой. При оценке количества соединений следует учитывать возможности системы и требования пользователей. Например, в системе с большим количеством пользователей обычно не всем из них требуется уникальное соединение. Несколько пользователей могут совместно использовать одно соединение. Приложениям OLE DB требуется отдельное соединение для каждого открытого объекта соединения, приложениям ODBC — соединение для каждого активного дескриптора соединения в приложении и, наконец, приложениям, работающим с DB-Library, — соединение для каждого запущенного процесса, вызывающего функцию **dbopen** из DB-Library.  
  
    > [!IMPORTANT]  
    >  Если параметр необходимо использовать, не устанавливайте слишком большое значение, поскольку каждое соединение требует затрат независимо от того, используется оно или нет. При превышении максимального количества пользовательских соединений отображается сообщение об ошибке и к серверу нельзя подключиться, пока соединения не станут доступными.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Разрешения на выполнение хранимой процедуры **sp_configure** без параметров или только с первым параметром по умолчанию предоставляются всем пользователям. Для выполнения процедуры **sp_configure** с обоими параметрами для изменения параметра конфигурации или запуска инструкции RECONFIGURE необходимо иметь разрешение ALTER SETTINGS на уровне сервера. Разрешение ALTER SETTINGS неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-configure-the-user-connections-option"></a>Настройка параметра пользовательских соединений  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите **Свойства**.  
  
2.  Выберите узел **Соединения** .  
  
3.  В разделе **Соединения** в поле **Максимальное число одновременных соединений** введите или выберите значение от 0 до 32 767, чтобы задать максимальное число пользователей, которые смогут одновременно подключиться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Перезапустите [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-configure-the-user-connections-option"></a>Настройка параметра пользовательских соединений  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере показано использование процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) для задания значения параметра `user connections` равным `325` пользователям.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'user connections', 325 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Дополнительные сведения см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-user-connections-option"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки параметра user connections  
 Чтобы изменения вступили в силу, необходимо перезапустить экземпляр SQL Server.  
  
## <a name="see-also"></a>См. также:  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
