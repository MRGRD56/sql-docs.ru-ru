---
title: Настройка параметра конфигурации сервера nested triggers | Документы Майкрософт
description: Описание параметра nested triggers. Сведения о том, как использовать его для настройки количества уровней триггеров AFTER, которые могут срабатывать каскадным образом в SQL Server.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- nested triggers option
ms.assetid: 29d7372b-d406-4a5b-80c6-a2d231d25211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5c5330bad05b0949c4df32f5593aa3fe39e2ad50
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783660"
---
# <a name="configure-the-nested-triggers-server-configuration-option"></a>Настройка конфигурации сервера nested triggers
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описывается настройка параметра конфигурации сервера **nested triggers** в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Параметр **nested triggers** определяет, допустимо ли каскадирование триггеров AFTER. Под этим подразумевается выполнение действия, вызывающего срабатывание другого триггера, который может инициировать другой триггер, и т. д. Когда параметр **nested triggers** принимает значение 0, триггеры AFTER не могут вызывать каскадные действия. Если параметр **nested triggers** равен 1 (значение по умолчанию), триггеры AFTER могут выполнять каскадные действия глубиной до 32 уровней. Триггеры INSTEAD OF могут быть вложенными вне зависимости от этого параметра.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Настройка параметра nested triggers с использованием следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После настройки параметра nested triggers](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Разрешения на выполнение хранимой процедуры **sp_configure** без параметров или только с первым параметром по умолчанию предоставляются всем пользователям. Для выполнения процедуры **sp_configure** с обоими параметрами для изменения параметра конфигурации или запуска инструкции RECONFIGURE необходимо иметь разрешение ALTER SETTINGS на уровне сервера. Разрешение ALTER SETTINGS неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-configure-the-nested-triggers-option"></a>Настройка параметра nested triggers  
  
1.  В **обозревателе объектов** щелкните сервер правой кнопкой мыши и выберите пункт **Свойства**.  
  
2.  На странице **Дополнительно** выберите значение **True** (по умолчанию) или **False** для параметра **Разрешить триггерам активировать другие триггеры**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-configure-the-nested-triggers-option"></a>Настройка параметра nested triggers  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере описывается использование процедуры [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) для задания значения параметра `nested triggers` равным `0`.  
  
```wmimof  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'nested triggers', 0 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Дополнительные сведения см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-nested-triggers-option"></a><a name="FollowUp"></a> Дальнейшие действия. После настройки параметра nested triggers  
 Параметр вступает в силу немедленно, без перезапуска сервера.  
  
## <a name="see-also"></a>См. также:  
 [Создание вложенных триггеров](../../relational-databases/triggers/create-nested-triggers.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
