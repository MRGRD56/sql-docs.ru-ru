---
title: Подготовка баз данных Access для миграции (Акцесстоскл) | Документация Майкрософт
description: Узнайте, как определить, какие базы данных Access необходимо перенести в SQL Server или базу данных SQL Azure, и убедиться, что эти базы данных готовы к миграции.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7c52b13a2a71afc005f19d1733e8dff435d2a582
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066589"
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>Подготовка баз данных Access для миграции (Акцесстоскл)
Перед миграцией баз данных Access в необходимо [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определить, какие базы данных необходимо перенести, и убедиться, что эти базы данных готовы к миграции.  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>Определение необходимости перехода на SQL Server  
Ядро СУБД Jet, используемое в качестве ядра СУБД для доступа, является гибким и простым в использовании решением для управления данными. Тем не менее, поскольку базы данных становятся более крупными и критически важными, многие пользователи находят, что они нуждаются в повышении производительности, безопасности или доступности. Для приложений, которым требуется более надежная платформа данных, рассмотрите возможность перемещения базовых баз данных для этих приложений в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения о принятии решения о том, когда следует выполнять миграцию, см. на [странице сведений о миграции](https://go.microsoft.com/fwlink/?LinkId=68571) на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] веб-сайте.  
  
После миграции баз данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно продолжать использовать доступ с помощью связанных таблиц или вручную перенести приложения в [!INCLUDE[msCoName](../../includes/msconame_md.md)] код на основе платформа .NET Framework, который взаимодействует напрямую с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="determining-which-databases-to-migrate"></a>Определение баз данных для переноса  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Помощник по миграции (SSMA) для доступа может искать базы данных Access. Затем можно экспортировать метаданные об этих базах данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения о том, как экспортировать и запрашивать метаданные, см. [в разделе Экспорт данных инвентаризации доступа](exporting-an-access-inventory-accesstosql.md).  

   > [!NOTE]
   > Не все функции и параметры доступа поддерживаются или могут быть легко преобразованы в, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Перед началом миграции баз данных см. раздел [несовместимые функции доступа](incompatible-access-features-accesstosql.md).
  
## <a name="preparing-for-migration"></a>Подготовка к переносу  
Используйте следующие рекомендации, чтобы подготовить базы данных Access к миграции в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="upgrading-older-access-databases"></a>Обновление баз данных Access прежних версий  
SSMA для Access поддерживает доступ к 97 и более поздним версиям. Если у вас есть базы данных из более ранних версий Access, откройте и сохраните базы данных в Access 97 или более поздней версии.  
  
### <a name="removing-workgroup-protection"></a>Удаление защиты рабочей группы  
SSMA не может перенести базы данных, использующие защиту рабочей группы. Чтобы удалить защиту рабочей группы из базы данных Access, выполните следующие действия.  
  
1.  Скопируйте файл базы данных Access в другое расположение.  
  
2.  Откройте скопированную базу данных.  
  
3.  В меню **Сервис** укажите пункт **Безопасность**, а затем выберите пункт **разрешения пользователей и групп**.  
  
4.  Выберите пункт **Пользователи** , выберите пользователя с **правами администратора** и убедитесь, что выбрано разрешение **Администрирование** .  
  
5.  Выберите пункт **группы** , выберите группу **Пользователи** и убедитесь, что выбрано разрешение **Администрирование** .  
  
6.  Нажмите кнопку **ОК**, а затем в меню **файл** выберите команду **выход**.  
  
Теперь можно использовать SSMA для переноса скопированной базы данных. После загрузки схемы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно вручную защитить базу данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="backing-up-databases"></a>Резервное копирование баз данных  
Перед миграцией баз данных Access в необходимо [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создать резервные копии баз данных Access, которые будут перенесены, а также [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] баз данных, в которые будут переноситься объекты и данные Access.  
  
Чтобы создать резервную копию базы данных Access, в меню **Сервис** выберите пункт **Служебные программы**, а затем выберите пункт **резервное копирование базы данных**.  
  
Сведения о резервном копировании [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] баз данных см. в разделе «Резервное копирование и восстановление баз данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] » в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] электронной документации по.  
  
### <a name="documenting-databases"></a>Документирование баз данных  
Кроме того, может потребоваться документировать свойства баз данных Access, например списки объектов, размер файлов и разрешения. Чтобы создать эту документацию в Access, в меню **Сервис** выберите пункт **анализ**, а затем — **Документация**.  
  
## <a name="see-also"></a>См. также раздел  
[Миграция баз данных Access в SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Связывание приложений Access с SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
