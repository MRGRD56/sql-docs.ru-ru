---
title: Установка и удаление служб Master Data Services | Документация Майкрософт
description: Этот процесс удаления позволяет удалить папки и файлы Master Data Services, а также удалить диспетчер конфигурации Master Data Services с локального компьютера.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: efc2431c-588b-42e7-b23b-c875145a33f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4f57ed3d099afa9f85dcc72f489bac6bbcb713fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336105"
---
# <a name="uninstall-and-remove-master-data-services"></a>Удаление служб Master Data Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

   Чтобы удалить компонент [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните действия из раздела [Удаление существующего экземпляра SQL Server (программа установки)](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md) и укажите [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] в качестве компонента для удаления на странице **Выбор компонентов**. В процессе удаления удаляются файлы и папки [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , а также удаляется [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] на локальном компьютере.  
  
 Чтобы избежать потери данных или нарушения работы других компьютеров в системе, некоторые компоненты при удалении не удаляются или не изменяются. Чтобы определить, следует ли оставить или удалить элементы, просмотрите следующую таблицу.  
  
|Элемент|Описание|  
|----------|-----------------|  
|Файлы и папки|В процессе удаления удаляется большинство файлов и папок, расположенных в пути установки.<br /><br /> В процессе удаления в пути установки не удаляются папки Master Data Services и MDSTempDir. После завершения процесса удаления эти папки можно удалить из файловой системы вручную. Дополнительные сведения см. в разделе [Разрешения для папок и файлов (службы Master Data Services)](../../master-data-services/folder-and-file-permissions-master-data-services.md).|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] сборки|Процесс удаления удаляет сборки [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] из глобального кэша сборок (GAC).|  
|База данных|Процесс удаления не затрагивает базу данных [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . База данных на экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] остается неизменной, поэтому все данные, включая основные данные, объекты моделей, разрешения пользователей и групп, бизнес-правила и т. п., сохраняются.<br /><br /> Если база данных больше не нужна и не предвидится ее соединение с другим веб-сайтом или приложением [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] в будущем, можно удалить базу данных на экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , на котором она размещена. Дополнительные сведения см. в разделе [Удаление базы данных](../../relational-databases/databases/delete-a-database.md).|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] и Web.config|В процессе удаления в файловой системе удаляется папка WebApplication. Папка WebApplication содержит файлы веб-приложения и файл Web.config, используемые [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].<br /><br /> **\*\* Важно! \*\*** Перед удалением можно скопировать файл Web.config в другое расположение, чтобы сохранить любые пользовательские настройки или другие содержащиеся в файле сведения. После завершения процесса удаления восстановить файл Web.config невозможно.|  
|Элементы служб Internet Information Services (IIS)|Процесс удаления не затрагивает пулы приложений, веб-сайты и веб-приложения в IIS на локальном компьютере. Так как процесс удаления удаляет папку WebApplication и файл Web.config для [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], любые веб-приложения [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] , которым требуются эти файлы, прекратят работу. Если пользователь попытается обратиться к такому веб-приложению, он получит сообщение об ошибке HTTP 500.19 — внутренняя ошибка сервера: "Запрашиваемая страница не доступна из-за неверной конфигурации данных для этой страницы".<br /><br /> Если веб-сайт или приложение больше не нужны, а сайт или приложение предоставляются пулом приложений, для их удаления можно воспользоваться средствами IIS. Дополнительные сведения см. в [Руководстве по использованию IIS 7](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732976(v=ws.10)) на сайте [!INCLUDE[msCoName](../../includes/msconame-md.md)] TechNet.|  
|Группа **MDS_ServiceAccounts**|После завершения процесса удаления группа Windows **MDS_ServiceAccounts** и любые добавленные в нее учетные записи служб сохраняются. Если эта группа и учетные записи больше не нужны, можно удалить их.|  
|Реестр|В процессе удаления из реестра Windows удаляются все разделы, связанные со службами [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|  
  
## <a name="see-also"></a>См. также:  
 [Установка служб Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
