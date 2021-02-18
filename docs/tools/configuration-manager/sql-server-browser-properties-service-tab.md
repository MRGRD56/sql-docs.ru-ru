---
title: Свойства браузер SQL Server (вкладка «Службы»)
description: Узнайте о параметрах на вкладке "Служба" в диалоговом окне свойств обозревателя SQL Server, таких как путь к двоичному файлу, имя узла и режим запуска.
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 98ace9b0-72d5-4b72-9b7b-11fbc490981a
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 31f663f7a4ce8de0d0eb11f95b50eddea0f8405e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345203"
---
# <a name="sql-server-browser-properties-service-tab"></a>Свойства браузер SQL Server (вкладка «Службы»)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Программа браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] работает в качестве службы на сервере. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прослушивает входящие запросы к ресурсам [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и предоставляет сведения об экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], установленных на компьютере.  
  
 Используйте вкладку **Службы** в диалоговом окне **Свойства браузера SQL Server** , чтобы просмотреть следующие параметры. Все свойства, кроме свойства **Режим запуска** , доступны только для чтения.  
  
## <a name="options"></a>Параметры  
 **Путь к двоичным файлам**  
 Отображает расположение программных файлов, используемых этой службой.  
  
 **Контроль ошибок**  
 1 означает `SERVICE_ERROR_NORMAL`. Если службе не удалось запуститься при запуске компьютера, программа запуска заносит в журнал сообщение об ошибке и выдает всплывающее сообщение об ошибке, но продолжает выполнять операцию запуска. Это значение невозможно изменить.  
  
 **Код завершения**  
 При возникновении ошибки ее номер выводится в этом окне. Этот номер обеспечивает возможность устранения ошибок, используйте его для поиска в базе знаний [!INCLUDE[msCoName](../../includes/msconame-md.md)] или сообщите его службе технической поддержки.  
  
 **Host Name**  
 Отображает имя компьютера или кластера, где запущена служба браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **имя**;  
 Указывает отображаемое имя службы.  
  
 **Идентификатор процесса**  
 Отображает идентификатор процесса Windows.  
  
 **Тип службы**  
 Отображает тип службы, предоставленной для вызывающих процессов. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливает несколько служб.  
  
 **Режим запуска**  
 Установите для этой службы один из следующих вариантов:  
  
-   Вручную: Эта служба не запускается автоматически при запуске компьютера. Необходимо запустить службу при помощи диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или другого средства.  
  
-   Автоматически. Выполняется попытка запуска службы при запуске компьютера.  
  
-   Отключено. Служба не может быть запущена.  
  
 **Состояние**  
 Указывает, была ли служба запущена, остановлена или отключена. **…** указывает, что ожидается изменение состояния.  
  
## <a name="see-also"></a>См. также:  
 [Служба обозревателя SQL Server](../../tools/configuration-manager/sql-server-browser-service.md)  
  
  
