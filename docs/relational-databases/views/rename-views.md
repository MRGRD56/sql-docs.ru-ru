---
description: Переименование представлений
title: Переименование представлений | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], renaming
- renaming views
ms.assetid: 5eed0488-81d2-40e8-8fdf-b0a640a591d0
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9132b99b3217cf99bdcf6073c02a0c1798a1502d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749244"
---
# <a name="rename-views"></a>Переименование представлений
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]
  Представление в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] можно переименовать, используя [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!WARNING]  
>  При переименовании представления фрагменты кода и приложения, использующие это представление, могут привести к сбою. Это касается других представлений, запросов, хранимых процедур, определяемых пользователем функций и клиентских приложений. Следует иметь в виду, что возникновение ошибок происходит каскадом.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Предварительные требования](#Prerequisites)  
  
     [Безопасность](#Security)  
  
-   **Переименование представления с использованием следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Follow Up:**  [After renaming a view](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
 Получение списка всех зависимостей в представлении. Все объекты, скрипты или приложения, которые ссылаются на представление, необходимо изменить в соответствии с новым именем представления. Дополнительные сведения см. в статье [Get Information About a View](../../relational-databases/views/get-information-about-a-view.md). Рекомендуется удалить представление и создать его повторно с новым именем вместо переименования. При повторном создании представления выполняется обновление сведений о зависимостях для объектов, на которые имеются ссылки в представлении.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER для SCHEMA или разрешение CONTROL для OBJECT, а также разрешение CREATE VIEW в базе данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-rename-a-view"></a>Переименование представления  
  
1.  В **обозревателе объектов** разверните базу данных, содержащую представление, которое необходимо переименовать, а затем разверните папку **Представление** .  
  
2.  Щелкните правой кнопкой мыши представление, которое нужно переименовать, и выберите пункт **Переименовать**.  
  
3.  Введите новое имя представления.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 **Переименование представления**  
  
 Хотя можно использовать **sp_rename** для изменения имени представления, рекомендуется удалить существующее представление, а затем повторно создать его с новым именем.  
  
 Дополнительные сведения см. в разделах [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md) и [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md).  
  
##  <a name="follow-up-after-renaming-a-view"></a><a name="FollowUp"></a> Дальнейшие действия. После переименования представления  
 Убедитесь, что все объекты, скрипты и приложения, ссылающиеся на предыдущее имя представления, теперь используют новое имя.  
  
  
