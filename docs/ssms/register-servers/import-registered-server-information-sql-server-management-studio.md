---
description: Импорт сведений о зарегистрированном сервере (среда SQL Server Management Studio)
title: Импорт сведений о зарегистрированном сервере
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.importregisteredservers.f1
helpviewer_keywords:
- transferring registered server information
- Registered Servers [SQL Server], importing
- importing registered server information
ms.assetid: cc497a14-1360-4887-b70c-002f042823b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 23fd49ac140d4e8da98324725e42f2e8ea6e006b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350501"
---
# <a name="import-registered-server-information-sql-server-management-studio"></a>Импорт сведений о зарегистрированном сервере (среда SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этом разделе описывается, как импортировать сохраненные сведения о зарегистрированных серверах в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Экспорт и импорт файлов зарегистрированных серверов позволяет легко настроить для нескольких компьютеров одинаковые зарегистрированные серверы. Это удобно при управлении большим количеством серверов с компьютеров, расположенных в различных местах, или если требуется сконфигурировать базовые настройки соединения для менее опытных пользователей.  
  
> [!NOTE]  
>  В [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] нельзя импортировать сведения о зарегистрированных серверах из более ранних версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-import-registered-server-information"></a>Импорт сведений о зарегистрированных серверах  
  
1.  В окне «Зарегистрированные серверы» выберите обозначение типа сервера на панели инструментов зарегистрированных серверов. Тип сервера должен совпадать с типом экспортированного файла зарегистрированного сервера. Например, если экспортированы сведения о зарегистрированном сервере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , следует выбрать в области «Зарегистрированные серверы» вариант **SQL Server** .  
  
2.  Щелкните правой кнопкой мыши группу серверов и выберите **Импорт**.  
  
3.  В диалоговом окне **Импорт данных о зарегистрированных серверах** выберите зарегистрированные серверы, которые нужно импортировать, и нажмите кнопку **ОК**.  
  
     **Файл импорта**  
     Введите в текстовое поле имя файла импорта или нажмите кнопку обзора (**...**), чтобы найти файл импорта на клиентском компьютере. Если выбрать существующий файл, то сведения о зарегистрированном сервере добавятся в этот файл. Файлом импорта может быть только ранее экспортированный файл зарегистрированных серверов. Файлы зарегистрированных серверов имеют расширение REGSVR.  
  
     **Выберите группу серверов для импорта**  
     Выберите корневой узел дерева или отдельную группу серверов, в которую будут импортированы зарегистрированные серверы. Можно импортировать в файл экспорта все зарегистрированные серверы, зарегистрированные серверы в определенной группе серверов или отдельные зарегистрированные серверы. Возможность импорта является рекурсивной. Например, если группа серверов А содержит группу серверов В, а группа серверов В содержит группы C и D, то при импорте группы А импортируются все серверы из групп А, В, С и D.  
  
     Группа серверов отображает только группы серверов из дерева текущего зарегистрированного сервера.  
  
     Если выбран импорт существующей группы серверов или сервера, выдается запрос на подтверждение перезаписи существующего сервера или группы серверов.  
  
 Регистрации серверов, использующие метод проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , хранят пароли отдельно для каждого пользователя. После импорта регистраций серверов пользователи должны ввести пароль при первом подключении к каждому серверу, сохраняя пароли в списке зарегистрированных серверов. Если серверы зарегистрированы с помощью проверки подлинности Windows, нет необходимости использовать эту операцию.  
  
## <a name="see-also"></a>См. также:  
 [Изменение регистрационных данных сервера (среда SQL Server Management Studio)](./change-a-server-s-registration-sql-server-management-studio.md)   
 [Выполнение экспорта сведений компонента "Зарегистрированные серверы" (среда SQL Server Management Studio)](./export-registered-server-information-sql-server-management-studio.md)   
 [Создание нового зарегистрированного сервера (среда SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)  
  
