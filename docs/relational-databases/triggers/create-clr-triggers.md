---
description: Создание триггеров CLR
title: Создание триггеров CLR | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- CRL triggers
- DML triggers, CLR triggers
- DDL triggers, CLR triggers
ms.assetid: 31f41703-134d-49fc-9850-76c297351c2c
author: rothja
ms.author: jroth
ms.openlocfilehash: a2c78991ea408f6b6b23169524280d834cf5c27a
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809302"
---
# <a name="create-clr-triggers"></a>Создание триггеров CLR
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Внутри [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно создавать объекты базы данных, запрограммированные в составе сборки, созданной в среде CLR платформы [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Объекты базы данных, способные эффективно использовать многофункциональную модель программирования, реализованную в среде CLR, включают триггеры DML и DDL, хранимые процедуры, функции, агрегатные функции и типы.  
  
 Шаги создания триггера CLR (DML или DDL) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следующие.  
  
-   Определите триггер как класс языка, поддерживаемого платформой .NET Framework. Дополнительные сведения о программировании триггеров CLR см. в разделе [Триггеры CLR](/dotnet/framework/data/adonet/sql/clr-triggers). После этого следует скомпилировать класс, чтобы создать сборку в [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , используя компилятор соответствующего языка.  
  
-   Зарегистрируйте сборку в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью инструкции CREATE ASSEMBLY. Дополнительные сведения о работе со сборками в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Сборки (компонент Database Engine)](../../relational-databases/clr-integration/assemblies-database-engine.md).  
  
-   Создайте триггер со ссылкой на зарегистрированную сборку.  
  
> [!NOTE]
>  Развертывание проекта SQL Server в [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] регистрирует сборку в базе данных, указанной для проекта. Развертывание проекта также создает триггеры CLR в базе данных для всех методов, аннотированных в атрибуте **SqlTrigger** . Дополнительные сведения см. в статье [Deploying CLR Database Objects](../../relational-databases/clr-integration/deploying-clr-database-objects.md).  
> 
> [!NOTE]
>  Возможность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнять код CLR по умолчанию отключена. Можно создавать, изменять и удалять объекты базы данных, которые ссылаются на модули управляемого кода, но эти ссылки не будут выполнены в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , пока параметр [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) не получит значение True с помощью хранимой процедуры [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 **Создание, изменение или удаление сборки**  
  
-   [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)  
  
-   [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md)  
  
-   [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)  
  
 **Добавление триггера CRL**  
  
-   [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
  
## <a name="see-also"></a>См. также  
 [Триггеры DML](../../relational-databases/triggers/dml-triggers.md)   
 [Основные понятия о программировании интеграции со средой (CLR)](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)   
 [Доступ к данным из объектов среды CLR для работы с базами данных](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)  
  
