---
description: Установка однопользовательского режима базы данных
title: Установка однопользовательского режима базы данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], database option
ms.assetid: fb5254eb-b635-4b39-8361-136fd36f2b1f
author: stevestein
ms.author: sstein
ms.openlocfilehash: 68d806e83c389c256737da8054a788697f0bd773
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812989"
---
# <a name="set-a-database-to-single-user-mode"></a>Установка однопользовательского режима базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается, как установить однопользовательский режим в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при помощи среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Однопользовательский режим указывает, что одновременный доступ к базе данных получает только один пользователь. Это в основном используется для операций обслуживания.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Предварительные требования](#Prerequisites)  
  
     [Безопасность](#Security)  
  
-   **Установка однопользовательского режима базы данных с помощью**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Если в процессе установки однопользовательского режима к базе данных подключены другие пользователи, то их подключения к базе данных будут закрыты без предупреждения.  
  
-   База данных остается в однопользовательском режиме даже и в том случае, если пользователь, установивший этот параметр, отключился. В этот момент к базе данных могут подключаться и другие пользователи, но одновременно может быть подключен только один.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Перед заданием параметра SINGLE_USER проверьте, чтобы параметру AUTO_UPDATE_STATISTICS_ASYNC было присвоено значение OFF. Если этот параметр имеет значение ON, то фоновый поток, используемый для обновления статистики, соединится с базой данных и доступ к базе данных в однопользовательском режиме будет невозможен. Дополнительные сведения см. в разделе [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Установка однопользовательского режима базы данных  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]и разверните его.  
  
2.  Щелкните правой кнопкой мыши базу данных, которую нужно изменить, и выберите пункт **Свойства**.  
  
3.  В диалоговом окне **Свойства базы данных** выберите страницу **Параметры** .  
  
4.  Для параметра **Ограничение доступа** выберите **Один**.  
  
5.  Если к базе данных подключены другие пользователи, то появится сообщение **Открытые соединения** . Чтобы изменить свойство и закрыть все другие подключения, нажмите кнопку **Да**.  
  
 С помощью этой процедуры можно также установить режим одновременного или ограниченного доступа к базе данных. Дополнительные сведения о параметрах ограниченного доступа см. в разделе [Свойства базы данных (страница "Параметры")](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Установка однопользовательского режима базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере база данных устанавливается в режим `SINGLE_USER` для получения монопольного доступа. Затем состояние базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] устанавливается в `READ_ONLY` и возвращается доступ к базе данных всем пользователям. Параметр прекращения `WITH ROLLBACK IMMEDIATE` указывается в первой инструкции `ALTER DATABASE` . Произойдет откат всех незавершенных транзакций, а любые другие соединения с базой данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] будут немедленно разорваны.  
  
 [!code-sql[DatabaseDDL#AlterDatabase8](../../relational-databases/databases/codesnippet/tsql/set-a-database-to-single_1.sql)]  
  
## <a name="see-also"></a>См. также:  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
  
