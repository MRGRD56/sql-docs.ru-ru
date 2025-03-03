---
title: Настройка разрешений файловой системы для доступа к компоненту ядра СУБД | Документы Майкрософт
description: Сведения об идентификаторах безопасности по службе. Сведения о том, как предоставить им разрешения для доступа к расположению, в котором хранятся файлы базы данных, чтобы обеспечить ядру СУБД доступ к ним.
ms.custom: ''
ms.date: 06/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- file system permissions
- service account [SQL Server], file system permissions
- permissions [SQL Server], file system
ms.assetid: 78bba43c-4edb-4216-84ac-d6246ae5546d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8a183fd9c0f291de299529379f2dede22737006c
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557587"
---
# <a name="configure-file-system-permissions-for-database-engine-access"></a>Настройка разрешений файловой системы для доступа к компоненту ядра СУБД
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описана процедура предоставления компоненту [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] доступа к расположению в файловой системе, где хранятся файлы базы данных. Служба компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] должна иметь разрешение файловой системы Windows для доступа к папке, в которой хранятся файлы базы данных. Разрешение на расположение по умолчанию задается во время установки. Если файла базы данных размещаются в другом расположении, то необходимо выполнить эти действия, чтобы предоставить компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] разрешение полного доступа к этому расположению.  
  
 Начиная с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , разрешения назначаются идентификатору безопасности каждой из служб. Эта система позволяет обеспечить изоляцию и всестороннюю защиту службы. Идентификатор безопасности службы создается на основе имени службы и является уникальным для каждой службы. В разделе [Настройка учетных записей и разрешений службы Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) описывается идентификатор безопасности каждой службы доступа, а имена перечисляются в разделе **Права и привилегии Windows**. Разрешение на доступ к расположению файла назначается именно идентификатору безопасности службы.  
  
## <a name="to-grant-file-system-permission-to-the-per-service-sid"></a>Предоставление разрешение на доступ к файловой системе идентификатору безопасности службы  
  
1.  С помощью проводника Windows перейдите в папку файловой системы, в которой находятся файлы базы данных. Правой кнопкой мыши щелкните эту папку и выберите пункт **Свойства**.  
  
2.  На вкладке **Безопасность** щелкните **Изменить** и затем ― **Добавить**.  
  
3.  В диалоговом окне **Выбор пользователей, компьютеров, учетных записей служб или групп** щелкните **Расположения**, в начале списка расположений выберите имя своего компьютера и нажмите кнопку **ОК**.  
  
4.  В поле **Введите имена объектов для выбора** введите имя идентификатора безопасности службы, указанное в разделе [**Настройка учетных записей службы и разрешений Windows**](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)электронной документации. (В качестве идентификатора безопасности службы компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] используйте **NT SERVICE\MSSQLSERVER** для экземпляра по умолчанию или **NT SERVICE\MSSQL$InstanceName** — для именованного экземпляра.)  
  
5.  Щелкните **Проверить имена** , чтобы проверить введенные данные. Проверка зачастую выявляет ошибки, по ее окончании может появиться сообщение о том, что имя не найдено. При нажатии кнопки **ОК** открывается диалоговое окно **Обнаружено несколько имен** . Теперь выберите идентификатор безопасности службы **MSSQLSERVER** или **NT SERVICE\MSSQL$InstanceName** и нажмите кнопку **ОК**.  Снова нажмите кнопку **ОК** , чтобы вернуться в диалоговое окно **Разрешения** .   
6.  В поле имен **Группа или пользователь** выберите имя идентификатора безопасности службы, а затем в поле **Разрешения для** \<name> установите флажок **Разрешить** для параметра **Полный доступ**.  
  
7. Нажмите кнопку **Применить**, а затем дважды кнопку **ОК** , чтобы выполнить выход.  
  
## <a name="see-also"></a>См. также:  
 [Управление службами компонента Database Engine](../../database-engine/configure-windows/manage-the-database-engine-services.md)   
 [Перемещение системных баз данных](../../relational-databases/databases/move-system-databases.md)   
 [Перемещение пользовательских баз данных](../../relational-databases/databases/move-user-databases.md)  
  
  
