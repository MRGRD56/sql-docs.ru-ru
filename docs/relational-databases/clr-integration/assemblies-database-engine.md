---
title: Сборки (ядро СУБД) | Документация Майкрософт
description: Экземпляр SQL Server может размещать сборки, которые развертывают функции, процедуры, триггеры и определяемые пользователем агрегаты и типы, написанные на языке CLR.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- assemblies [CLR integration]
- assemblies [CLR integration], about assemblies
- managed code [SQL Server], assemblies
ms.assetid: 4b146437-3061-47f6-9e8c-26eeea10b54e
author: rothja
ms.author: jroth
ms.openlocfilehash: c3a507f250dcecf62560284916d543ca2f6d53c0
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810240"
---
# <a name="assemblies-database-engine"></a>Сборки (компонент Database Engine)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе содержатся сведения, которые помогут понять, сконструировать и применить сборки.  
  
 Сборки — это файлы DLL, используемые в экземпляре служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для развертывания функций, хранимых процедур, триггеров, определяемых пользователем агрегатов и определяемых пользователем типов, написанных на одном из языков управляемого кода, размещенных в среде CLR [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , а не в [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Сборка в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] представляет собой объект, который ссылается на управляемый модуль приложений (DLL-файл), созданный в среде CLR [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Сборка содержит метаданные класса и управляемый код. Передача сборки на экземпляр SQL Server — это первый шаг к созданию любого из следующих объектов базы данных.  
  
-   Функции среды CLR. Дополнительные сведения см. в разделе [Создание функций CLR](../../relational-databases/user-defined-functions/create-clr-functions.md).  
  
-   Хранимые процедуры среды CLR. Дополнительные сведения см. в разделе [хранимые процедуры CLR](/dotnet/framework/data/adonet/sql/clr-stored-procedures).  
  
-   Триггеры среды CLR. Дополнительные сведения см. в разделе [Создание триггеров CLR](../../relational-databases/triggers/create-clr-triggers.md).  
  
-   Определяемые пользователем агрегатные функции. Дополнительные сведения см. в разделе [Создание определяемых пользователем статистических функций](../../relational-databases/user-defined-functions/create-user-defined-aggregates.md).  
  
-   Определяемые пользователем типы. Дополнительные сведения см. в статье [Использование пользовательских типов](../../relational-databases/native-client/features/using-user-defined-types.md).  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сборки выполняют следующие функции.  
  
-   Содержат управляемый код, который выполняет функциональность одного или нескольких объектов базы данных среды CLR, перечисленных ранее.  
  
-   Содержат метаданные, которые включают в себя номер версии и культуру сборки, дополнительный открытый ключ, который уникально идентифицирует список классов сборки, методы, определенные в сборке, и архитектуру процессора сборки.  
  
-   Управляют степенью, к которой управляемый код может получить доступ внешних ресурсов с помощью регулирования разрешений кода доступа.  
  
-   Содержат метаданные о зависимостях от других сборок, на которые ссылается сборка.  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Конструирование сборок](../../relational-databases/clr-integration/assemblies-designing.md)|Объясняет, что необходимо учесть перед созданием сборки. Включает в себя упаковку сборок, разрешения кода доступа и другие ограничения.|  
|[Реализация сборок](../../relational-databases/clr-integration/assemblies-implementing.md)|Объясняет, как правильно создать и удалить сборку, как и когда необходимо изменить сборку, а также как получить метаданные о сборке.|  
|[Получение сведений о сборках](../../relational-databases/clr-integration/assemblies-getting-information.md)|Перечисляет представления и функции каталога, которые могут запрашиваться для метаданных о сборках.|  
  
## <a name="see-also"></a>См. также:  
 [Основные понятия о программировании интеграции со средой (CLR)](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)  
  
