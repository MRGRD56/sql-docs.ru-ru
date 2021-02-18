---
description: Создание центрального сервера управления и группы серверов
title: Создание центрального сервера управления
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- configuration server
ms.assetid: da265482-3953-440a-ac23-0ab7e42a55eb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 843411069a27d658cf9e2b157170d8ca0f14c91c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350552"
---
# <a name="create-a-central-management-server-and-server-group"></a>Создание центрального сервера управления и группы серверов

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этом разделе описывается, как назначить экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве сервера централизованного управления в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. На серверах централизованного управления хранится список экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , организованных в одну или несколько групп серверов централизованного управления. Действия, производимые с помощью группы серверов централизованного управления, влияют на все серверы в группе. Это включает соединение с сервером при помощи обозревателя объектов, а также выполнение инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] и применение политик управления на основе политик одновременно на нескольких серверах.  
  
> [!NOTE]  
>  Версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ранее [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] нельзя назначить в качестве сервера централизованного управления.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Для создания сервера централизованного управления и группы серверов используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 В базе данных msdb доступ к серверам централизованного управления предоставляют две роли базы данных. Сервером централизованного управления могут управлять только члены роли ServerGroupAdministratorRole. Для подключения к серверу централизованного управления требуется членство в роли ServerGroupReaderRole.  
  
 Поскольку соединения, поддерживаемые сервером централизованного управления, выполняются в контексте пользователя с применением проверки подлинности Windows, действующие разрешения на зарегистрированные серверы могут быть различными. Например, пользователь может входить в предопределенную роль сервера sysadmin на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] А, но иметь ограниченные разрешения на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Б.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Ниже описывается, как выполнить следующие шаги.  
  
1.  Создание сервера централизованного управления.  
  
2.  Добавление одной или нескольких групп серверов на центральный сервер управления или добавление одного или нескольких зарегистрированных серверов в группы серверов.  
  
#### <a name="create-a-central-management-server"></a>Создание сервера централизованного управления  
  
1.  В среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]в меню **Вид** выберите пункт **Зарегистрированные серверы**.  
  
2.  В окне "Зарегистрированные серверы" разверните узел **Компонент Database Engine**, щелкните правой кнопкой мыши **Серверы централизованного управления** и выберите пункт **Зарегистрировать сервер централизованного управления**.  
  
3.  В диалоговом окне **Регистрация нового сервера** выберите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который необходимо сделать сервером централизованного управления из раскрывающегося списка серверов. Для сервера централизованного управления необходимо использовать проверку подлинности Windows.  
  
4.  В поле **Зарегистрированный сервер** введите имя сервера и описание (необязательно).  
  
5.  На вкладке **Свойства подключения** просмотрите или измените свойства сети и подключения. Дополнительные сведения см. в статье [Соединение с сервером (страница "Свойства подключения"), компонент Database Engine](../f1-help/connect-to-server-connection-properties-page-database-engine.md).  
  
6.  Нажмите кнопку **Проверка**, чтобы проверить соединение.  
  
7.  Нажмите **Сохранить**. Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] появится в папке **Серверы централизованного управления** .  
  
#### <a name="create-a-new-server-group-and-add-servers-to-the-group"></a>Создание группы серверов и добавление в нее серверов  
  
1.  В окне **Зарегистрированные серверы** разверните узел **Серверы централизованного управления**. Щелкните правой кнопкой мыши экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , добавленный в предыдущей процедуре, и выберите пункт **Создать группу серверов**.  
  
2.  В окне **Свойства новой группы серверов** введите имя группы и описание (необязательно).  
  
3.  В окне **Зарегистрированные серверы** щелкните правой кнопкой мыши группу серверов и выберите команду **Регистрация нового сервера**.  
  
4.  В окне регистрации сервера выберите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Создание нового зарегистрированного сервера (среда SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md). При необходимости добавьте дополнительные серверы.  
  
#### <a name="to-execute-queries-against-several-configuration-targets-at-the-same-time"></a>Выполнение запросов одновременно к нескольким целям конфигурации  
  
-   После создания сервера централизованного управления, одной или нескольких групп серверов и одного или нескольких зарегистрированных серверов можно выполнять запросы одновременно ко всей группе. Дополнительные сведения о выполнении инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] сразу на нескольких серверах в группе серверов см. в статье [Выполнение инструкции на нескольких серверах одновременно (среда SQL Server Management Studio)](./execute-statements-against-multiple-servers-simultaneously.md).  
  
## <a name="see-also"></a>См. также:  
 [Администрирование нескольких серверов с использованием центральных серверов управления](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
