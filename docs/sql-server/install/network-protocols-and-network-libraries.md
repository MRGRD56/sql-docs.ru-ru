---
title: Сетевые протоколы и библиотеки | Документация Майкрософт
description: Сервер можно настроить для наблюдения за несколькими сетевыми протоколами. Конфигурацию можно изменить с помощью диспетчера конфигурации SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336160"
---
# <a name="network-protocols-and-network-libraries"></a>Сетевые протоколы и библиотеки
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Сервер может одновременно прослушивать или отслеживать работу нескольких сетевых протоколов. Однако каждый из этих протоколов должен быть настроен. Если какой-нибудь из протоколов не настроен, сервер не сможет его прослушивать. После установки можно изменить конфигурации протоколов при помощи диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="default-sql-server-network-configuration"></a>Сетевая конфигурация SQL Server по умолчанию  
 В экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию настроен порт TCP/IP 1433 и именованный канал \\\\.\pipe\sql\query. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] задаются динамические порты TCP, при этом номер порта назначается операционной системой.  
  
 Если использование динамических номеров портов невозможно (например, если соединения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должны проходить через брандмауэр, настроенный на пропуск пакетов через конкретные номера портов). Выберите незанятый номер порта. Присваиванием номеров портов управляет администрация адресного пространства Интернет (IANA), а списки этих номеров находятся по адресу [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844).  
  
 В целях повышения безопасности при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сетевой обмен данными включен не полностью. Чтобы включить, отключить или настроить сетевые протоколы после завершения программы установки, используется область «Конфигурация сети [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] » диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="server-message-block-protocol"></a>Протокол SMB  
 На серверах в демилитаризованной зоне сети необходимо отключить все ненужные протоколы, в том числе протокол SMB. Веб-серверам и DNS-серверам не нужен протокол SMB. Этот протокол необходимо отключить, чтобы противостоять угрозе сбора сведений о пользователях.  
  
> [!WARNING]
>  Отключение протокола SMB заблокирует доступ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или службы кластеров Windows к удаленной общей папке. Не отключайте протокол SMB, если выполняется или планируется одно из следующих действий:  
> 
>  -   Использование режима кворума большинства общих папок и узлов кластера Windows  
> -   Указание общей папки SMB в качестве каталога данных во время установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
> -   Создание файла базы данных в общей папке SMB  
  
#### <a name="to-disable-smb"></a>Отключение протоколаSMB  
  
1.  В меню **Пуск** выберите **Настройки** и щелкните **Сетевые подключения и коммутируемые соединения**.  
  
     Щелкните правой кнопкой мыши ярлык соединения с Интернетом и выберите пункт **Свойства**.  
  
2.  Установите флажок **Клиент для сетей Microsoft** и нажмите кнопку **Удалить**.  
  
3.  Выполните шаги для удаления.  
  
4.  Выберите **Службу доступа к файлам и принтерам сетей Microsoft** и нажмите кнопку **Удалить**.  
  
5.  Выполните шаги для удаления.  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>Отключение протокола SMB на серверах, доступных из Интернета  
  
-   В окне "Соединение по локальной сети — свойства" используйте диалоговое окно **Свойства: протокол Интернета (TCP/IP)** , чтобы удалить компоненты **Совместный доступ к файлам и принтерам сетей Microsoft** и **Клиент для сетей Microsoft**.  
  
## <a name="endpoints"></a>Конечные точки  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для соединений [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] введена новая концепция. Соединение представляется на стороне сервера в виде [!INCLUDE[tsql](../../includes/tsql-md.md)]*конечной точки*. Для конечных точек [!INCLUDE[tsql](../../includes/tsql-md.md)] можно предоставлять, отменять и запрещать разрешения. По умолчанию все пользователи имеют разрешения на подключение к конечной точке, если только это разрешение не отменено и не запрещено членом группы sysadmin или владельцем конечной точки. В синтаксисе GRANT, REVOKE и DENY ENDPOINT применяется идентификатор, который администратор должен получить из представления каталога конечной точки.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает конечные точки [!INCLUDE[tsql](../../includes/tsql-md.md)] для всех поддерживаемых сетевых протоколов, а также для выделенного административного соединения.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] создаются программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] следующим образом.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] локальный компьютер  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] именованные каналы  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] по умолчанию  
  
 Дополнительные сведения о конечных точках см. в разделах [Настройка ядра СУБД на прослушивание нескольких портов TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) и [Представления каталога конечных точек (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md).  
  
 Дополнительные сведения о сетевой конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в следующих статьях в электронной документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [Сетевая конфигурация сервера](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>См. также:  
 [Настройка контактной зоны](../../relational-databases/security/surface-area-configuration.md)   
 [Вопросы безопасности при установке SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Планирование установки SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  
