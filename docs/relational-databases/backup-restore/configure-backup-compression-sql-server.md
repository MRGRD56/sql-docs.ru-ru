---
title: Настройка сжатия резервных копий (SQL Server) | Документация Майкрософт
description: В этой статье описано, как переопределить настройку по умолчанию на уровне сервера при создании отдельной резервной копии или во время планирования ряда регулярных операций резервного копирования в SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 430905eb-d218-458c-bd48-aeee6fbb7446
author: cawrites
ms.author: chadam
ms.openlocfilehash: 31376d9ff735c30a0374acade23e3a8a89804baa
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127029"
---
# <a name="configure-backup-compression-sql-server"></a>Настройка сжатия резервных копий (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  При установке сжатие резервных копий по умолчанию отключено. По умолчанию способ сжатия резервных копий определяется параметром конфигурации **backup compression default** уровня сервера. Однако настройку по умолчанию на уровне сервера можно переопределить при создании отдельной резервной копии или во время планирования ряда регулярных операций резервного копирования. Чтобы изменить это параметр, см. раздел [Параметр конфигурации сервера "Просмотр или настройка параметра сжатия резервных копий по умолчанию"](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).  
  
## <a name="override-the-backup-compression-default"></a>Переопределение сжатия резервных копий по умолчанию  
 Способ сжатия резервных копий вы можете изменить для отдельной резервной копии, задания резервного копирования или для конфигурации доставки журналов.  
  
-   **[!INCLUDE[tsql](../../includes/tsql-md.md)]**  
  
     Для переопределения настроек сжатия резервных копий по умолчанию используйте параметр WITH NO_COMPRESSION или WITH COMPRESSION в инструкции [BACKUP](../../t-sql/statements/backup-transact-sql.md) в процессе создания резервной копии.  
  
     Для конфигурации доставки журналов поведение сжатия резервных копий можно контролировать с помощью хранимой процедуры [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)[sp_change_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md).  
  
-   **[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
     Сведения о просмотре или настройке параметра сжатия резервных копий по умолчанию для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Параметр конфигурации сервера "Просмотр или настройка параметра сжатия резервных копий по умолчанию"](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).  
  
     Можно переопределить сжатие резервных копий по умолчанию для сервера, выбрав **Сжимать резервные копии** или **Не сжимать резервные копии** в одном из следующих диалоговых окон:  
  
    -   [Резервное копирование базы данных (страница «Параметры»)](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
         При резервном копировании базы данных можно управлять сжатием резервной копии для отдельной базы данных, файла или журнала.  
  
    -   [Использование мастера планов обслуживания](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
         Мастер планов обслуживания позволяет управлять сжатием резервных копий для каждого запланированного набора полных или разностных резервных копий базы данных или журнала.  
  
    -   Integration Services (SSIS) [Задача "Резервное копирование базы данных"](../../integration-services/control-flow/back-up-database-task.md)  
  
         Сжатием резервных копий можно управлять при создании пакета для резервного копирования отдельной базы данных или нескольких баз данных.  
  
    -   [Настройки резервного копирования журналов транзакций для доставки журналов](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md)  
  
         Можно управлять поведением сжатия резервных копий журналов.  
  
  
## <a name="see-also"></a>См. также:  
 [Сжатие резервных копий (SQL Server)](../../relational-databases/backup-restore/backup-compression-sql-server.md)  
  
  
