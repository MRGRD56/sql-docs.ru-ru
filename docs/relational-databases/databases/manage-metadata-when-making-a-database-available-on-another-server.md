---
description: Управление метаданными при предоставлении доступа к базе данных на другом сервере
title: Управление метаданными при предоставлении доступа к базе данных на другом сервере
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 5d98cf2a-9fc2-4610-be72-b422b8682681
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
helpviewer_keywords:
- cross-database queries [SQL Server]
- logins [SQL Server], recreating on another server instance
- triggers [SQL Server], DLL
- user-defined error messages [SQL Server]
- permissions [SQL Server], metadata access
- Full-Text Engine [SQL Server]
- metadata [SQL Server], databases available to other instances
- jobs [SQL Server Agent], recreating on another server instance
- failover [SQL Server], managing metadata
- event notifications [SQL Server], metadata
- database mirroring [SQL Server], metadata
- startup options [SQL Server]
- restoring [SQL Server], onto another server instance
- linked servers [SQL Server], metadata
- WMI Provider for Server Events, metadata
- attaching databases [SQL Server]
- log shipping [SQL Server], metadata
- encryption [SQL Server], metadata
- server configuration [SQL Server]
- distributed queries [SQL Server], metadata
- extended stored procedures [SQL Server], metadata
- credentials [SQL Server], metadata
- copying databases
ms.openlocfilehash: 8502271ba3b04e66fd6fcc0b2723e0c8e4533f96
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890687"
---
# <a name="manage-metadata-when-making-a-database-available-on-another-server"></a>Управление метаданными при предоставлении доступа к базе данных на другом сервере
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Информация в этой статье применима в следующих ситуациях:  
  
-   Настройка реплик доступности группы готовности [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .  
  
-   Настройка зеркального отображения базы данных.  
  
-   Подготовка смены ролей между сервером-источником и сервером-получателем в конфигурации доставки журналов.  
  
-   Восстановление базы данных на другом экземпляре сервера.  
  
-   Присоединение копии базы данных к другому экземпляру сервера.  
  
 Некоторые приложения зависят от информации, сущностей или объектов, которые находятся вне области однопользовательской базы данных. Как правило, приложение зависит от баз данных **master** , **msdb** и пользовательской базы данных. Что-либо сохраненное вне пользовательской базы данных, которая требуется для правильного функционирования другой базы данных, должно быть доступно на экземпляре целевого сервера. Например, имена входа для приложений сохраняются как метаданные в базе данных **master** и должны быть созданы заново на целевом сервере. Если приложение или план обслуживания базы данных зависит от заданий агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , метаданные которого сохранены в базе данных **msdb** , необходимо создать заново эти задания в экземпляре целевого сервера. Точно так же метаданные сохраняются в базе данных **master** для триггера уровня сервера.  
  
 При перемещении базы данных для приложения в другой экземпляр сервера необходимо повторно создать все метаданные зависимых сущностей и объектов в базах данных **master** и **msdb** в экземпляре целевого сервера. Например, если в приложении базы данных используются триггеры уровня сервера, то простого присоединения или восстановления базы данных в новой системе будет недостаточно. Функциональность базы данных не будет соответствовать ожидаемой, пока метаданные для этих триггеров в базе данных **master** не будут повторно созданы вручную.  
  
##  <a name="information-entities-and-objects-that-are-stored-outside-of-user-databases"></a><a name="information_entities_and_objects"></a> Информация, сущности и объекты, сохраненные вне пользовательской базы данных  
 В заключении этой статьи приводятся потенциальные проблемы, возникающие при работе с базой данных, которая доступна в другом экземпляре сервера. Возможно, потребуется повторно создать один или несколько типов информации, сущностей или объектов, перечисленных в следующем списке. Чтобы просмотреть итоговую информацию, щелкните ссылку элемента.  
  
-   [Параметры конфигурации сервера](#server_configuration_settings)  
  
-   [Учетные данные](#credentials)  
  
-   [Запросы баз данных](#cross_database_queries)  
  
-   [Владелец базы данных](#database_ownership)  
  
-   [Распределенные запросы и связанные серверы](#distributed_queries_and_linked_servers)  
  
-   [Зашифрованные данные](#encrypted_data)  
  
-   [Определяемые пользователем сообщения об ошибках](#user_defined_error_messages)  
  
-   [Уведомления о событиях и события инструментария управления Windows (WMI) на уровне сервера](#event_notif_and_wmi_events)  
  
-   [Расширенные хранимые процедуры](#extended_stored_procedures)  
  
-   [Свойства средства полнотекстового поиска для SQL Server](#ifts_service_properties)  
  
-   [Задания](#jobs)  
  
-   [Имена входа](#logins)  
  
-   [Разрешения](#permissions)  
  
-   [Параметры репликации](#replication_settings)  
  
-   [Приложения компонента Service Broker](#sb_applications)  
  
-   [Стартовые процедуры](#startup_procedures)  
  
-   [Триггеры уровня сервера](#triggers)  
  
##  <a name="server-configuration-settings"></a><a name="server_configuration_settings"></a> Server Configuration Settings  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и более поздние версии позволяют выборочно устанавливать и запускать ключевые службы и компоненты. Это помогает сократить уязвимую контактную зону системы. В конфигурации по умолчанию для новых экземпляров многие из функций отключены. Если в базе данных имеется зависимость от какой-либо отключенной по умолчанию службы или свойства, то их необходимо включить на целевом экземпляре сервера.  
  
 Дополнительные сведения об этих параметрах и их включении и отключении см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
  
##  <a name="credentials"></a><a name="credentials"></a> Учетные данные  
 Учетные данные являются записью, которая содержит сведения для проверки подлинности, необходимые для подключения к ресурсу извне [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Как правило, учетные данные представляют собой имя входа и пароль Windows.  
  
 Дополнительные сведения об этой функции см. в разделе [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).  
  
> **ПРИМЕЧАНИЕ.** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Получить идентификационный номер участника-посредника можно в системной таблице [sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md) .  
  
  
##  <a name="cross-database-queries"></a><a name="cross_database_queries"></a> Cross-Database Queries  
 Параметры базы данных DB_CHAINING и TRUSTWORTHY по умолчанию принимают значение OFF. Если в исходной базе данных какой-либо из этих параметров имеет значение ON, то может потребоваться его включение в целевом экземпляре сервера. Дополнительные сведения см. в разделе [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 Операции присоединения и отсоединения приводят к отмене межбазовых цепочек владения для базы данных. Сведения о том, как включить цепочки владения, см. в разделе [Параметр конфигурации сервера "cross db ownership chaining"](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md).  
  
 Дополнительные сведения см. также в разделе [Настройка зеркальной базы данных на использование свойства TRUSTWORTHY (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
  
##  <a name="database-ownership"></a><a name="database_ownership"></a> Database Ownership  
 При восстановлении базы данных на другом компьютере имя входа пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или пользователя Windows, начавшего процесс восстановления, автоматически становится владельцем базы данных. При восстановлении базы данных системный администратор или владелец новой базы данных могут сменить ее владельца.  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distributed_queries_and_linked_servers"></a> Распределенные запросы и связанные серверы  
 Распределенные запросы и связанные серверы поддерживаются приложениями OLE DB. Распределенные запросы получают доступ к данным из нескольких разнородных источников, расположенных на одних и тех же или разных компьютерах. Конфигурация связанных серверов позволяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнять команды в источниках данных OLE DB на удаленных серверах. Дополнительные сведения об этих функциях см. в разделе [Связанные серверы (компонент Database Engine)](../../relational-databases/linked-servers/linked-servers-database-engine.md).  
  
  
##  <a name="encrypted-data"></a><a name="encrypted_data"></a> Encrypted Data  
 Если в базе данных, к которой осуществляется доступ с другого экземпляра сервера, содержатся зашифрованные данные, а на исходном сервере главный ключ базы данных защищен главным ключом службы, может потребоваться повторное шифрование главного ключа службы. *Главный ключ базы данных* — это симметричный ключ, который применяется для защиты закрытых ключей сертификатов и асимметричных ключей, имеющихся в базе данных. При создании этот ключ зашифровывается с помощью алгоритма Triple DES и пользовательского пароля.  
  
 Чтобы разрешить автоматическое шифрование главного ключа базы данных на экземпляре сервера, копия этого ключа зашифровывается с использованием главного ключа службы. Эта зашифрованная копия хранится как в рабочей базе данных, так и в базе данных **master**. Как правило, копия, которая хранится в базе данных **master** , обновляется без взаимодействия с пользователем при каждом изменении главного ключа. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сначала пытается расшифровать главный ключ базы данных с использованием главного ключа службы экземпляра. Если расшифровка заканчивается неудачей, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет в хранилище учетных данных поиск учетных данных главного ключа, имеющих идентификатор GUID того же семейства, что и у базы данных, для которой нужен главный ключ. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пытается расшифровать главный ключ базы данных с помощью всех подходящих учетных данных, пока не удастся расшифровать ключ или пока не кончатся учетные данные. Главный ключ, который не зашифрован с помощью главного ключа службы, следует открывать с помощью инструкции OPEN MASTER KEY и пароля.  
  
 При копировании, восстановлении или присоединении зашифрованного ключа базы данных на новом экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]в базе данных **master** целевого сервера не содержится копия главного ключа базы данных, зашифрованного с использованием главного ключа службы. На целевом экземпляре сервера необходимо открыть главный ключ базы данных. Сделать это можно, выполнив следующую инструкцию: OPEN MASTER KEY DECRYPTION BY PASSWORD **='** _password_ **'** . После этого рекомендуется включить автоматическую расшифровку главного ключа базы данных, выполнив следующую инструкцию: ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY. Эта инструкция передает экземпляру сервера копию главного ключа базы данных, зашифрованного с использованием главного ключа службы. Дополнительные сведения см. в разделах [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) и [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
 Сведения о включении автоматической расшифровки главного ключа базы данных в зеркальной базе данных см. в разделе [Настройка зашифрованной зеркальной базы данных](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
 Дополнительные сведения см. также в следующих разделах:  
  
-   [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [Настройка зашифрованной зеркальной базы данных](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Создание идентичных симметричных ключей на двух серверах](../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
##  <a name="user-defined-error-messages"></a><a name="user_defined_error_messages"></a> User-defined Error Messages  
 Определяемые пользователем сообщения об ошибках настраиваются в представлении каталога [sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) , которое хранится в базе данных **master**. Если приложение базы данных зависит от определяемых пользователем сообщений об ошибках и если эта база данных доступна на другом экземпляре сервера, то для добавления на целевой экземпляр сервера уже имеющихся определяемых пользователем сообщений об ошибках следует пользоваться хранимой процедурой [sp_addmessage](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md) .  

  
##  <a name="event-notifications-and-windows-management-instrumentation-wmi-events-at-server-level"></a><a name="event_notif_and_wmi_events"></a> Уведомления о событиях и события инструментария управления Windows (WMI) на уровне сервера  
  
### <a name="server-level-event-notifications"></a>Уведомления о событии на уровне сервера  
 Уведомления о событиях уровня сервера хранятся в базе данных **msdb**. Поэтому, если приложение базы данных находится в зависимости от уведомления о событии уровня сервера, это уведомление необходимо создать и в целевом экземпляре сервера. Для просмотра уведомлений о событиях в экземпляре сервера используется представление каталога [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) . Дополнительные сведения см. в разделе [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 В дополнение уведомления о событиях доставляются с помощью компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Маршруты входящих сообщений не включаются в базу данных, содержащую службу. Вместо этого явные маршруты хранятся в базе данных **msdb**. Если служба использует явный маршрут в базе данных **msdb** для перенаправления входящих сообщений, при присоединении базы данных к другому экземпляру необходимо заново создать этот маршрут.  
  
### <a name="windows-management-instrumentation-wmi-events"></a>События инструментария управления Windows (WMI)  
 Поставщик инструментария WMI для событий сервера позволяет использовать Инструментарий управления Windows (WMI) для контроля событий в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Все приложения, основанные на событиях уровня сервера, обработанных поставщиком инструментария WMI, который необходим базе данных, должны быть определены на компьютере экземпляра целевого сервера. Поставщик событий инструментария WMI создает уведомления о событиях с целевой службой, описанной в базе данных **msdb**.  
  
> **ПРИМЕЧАНИЕ.** Дополнительные сведения см. в разделе [Основные понятия о поставщике WMI для событий сервера](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 **Создание оповещения WMI в среде SQL Server Management Studio**  
  
-   [Создание предупреждения о событии WMI](../../ssms/agent/create-a-wmi-event-alert.md)  
  
### <a name="how-event-notifications-work-for-a-mirrored-database"></a>Принцип работы уведомлений о событиях зеркальной базы данных  
 Межбазовая доставка уведомлений о событиях, в которой участвует зеркально отображенная база данных, по определению является удаленной, потому что зеркально отображенная база данных может выполнить переход на другой ресурс. [!INCLUDE[ssSB](../../includes/sssb-md.md)] поддерживает зеркально отображенные базы данных в форме *маршрутов зеркального отображения*. Маршрут зеркального отображения имеет два адреса: один для экземпляра основного сервера и другой — для экземпляра зеркального сервера.  
  
 С помощью настройки маршрутов зеркального отображения создается маршрутизация компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] , учитывающая зеркальное отображение базы данных. Маршруты зеркального отображения позволяют компоненту [!INCLUDE[ssSB](../../includes/sssb-md.md)] явно переадресовывать сеансы связи на текущий экземпляр основного сервера. Например, рассмотрим службу Service_A, которая расположена на зеркальной базе данных Database_A. Предположим, что необходима другая служба Service_B, расположенная в базе данных Database_B, чтобы вести диалог со службой Service_A. Для этого диалога база данных Database_B должна содержать зеркально отображенный маршрут для службы Service_A. Кроме того, база данных Database_A должна содержать незеркальный маршрут протокола TCP к службе Service_B, который в отличие от локального остается допустимым после отработки отказа. Эти маршруты включают ACK для возврата после отработки отказа. Поскольку службу отправителя всегда называют тем же способом, маршрут должен указывать экземпляр брокера.  
  
 Требования для зеркально отображенных маршрутов применяются независимо от того, является ли служба в зеркально отображенной базе данных вызывающей или целевой.  
  
-   Если целевая служба находится в зеркально отображенной базе данных, вызывающая служба должна иметь обратный, зеркально отображенный маршрут к целевой службе. Однако целевая служба может иметь постоянный маршрут к вызывающей службе.  
  
-   Если вызывающая служба находится в зеркально отображенной базе данных, целевая служба должна иметь обратный, зеркально отображенный маршрут к вызывающей службе для доставки подтверждений и ответов. Однако вызывающая служба может иметь постоянный обратный маршрут к целевой службе.  
  
  
##  <a name="extended-stored-procedures"></a><a name="extended_stored_procedures"></a> Extended Stored Procedures  
  
> **ВАЖНО!** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Используйте вместо этого [интеграцию со средой CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
 Программирование расширенных хранимых процедур осуществляется с помощью API-интерфейса расширенных хранимых процедур [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Участник предопределенной роли сервера **sysadmin** может зарегистрировать расширенную хранимую процедуру на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и выдать пользователям разрешение на ее выполнение. Расширенные хранимые процедуры будут добавляться только в базу данных **master** .  
  
 Расширенные хранимые процедуры запускаются непосредственно в адресном пространстве экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]и могут приводить к утечкам памяти или другим проблемам, снижающим производительность и надежность сервера. Целесообразно хранить расширенные хранимые процедуры в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , отдельном от экземпляра, содержащего данные, на которые они ссылаются. Следует также рассмотреть возможность использования распределенных запросов для получения доступа к базе данных.  
  
  > [!IMPORTANT]
  > Прежде чем добавлять расширенные хранимые процедуры на сервер и предоставлять разрешение EXECUTE на них другим пользователям, системный администратор должен тщательно проверить каждую расширенную хранимую процедуру, чтобы убедиться, что она не содержит вредоносного или злонамеренного кода.  
  
 Дополнительные сведения см. в разделах [Предоставление разрешений для объекта (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md), [DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md) и [REVOKE, отмена разрешения (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md).  
  
  
##  <a name="full-text-engine-for-sql-server-properties"></a><a name="ifts_service_properties"></a> Full-Text Engine for SQL Server Properties  
 Свойства средства полнотекстового поиска устанавливаются процедурой [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md). Убедитесь, что на целевом экземпляре сервера настроены необходимые для этих свойств параметры. Дополнительные сведения об этих свойствах см. в разделе [FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md).  
  
 Кроме того, если на исходном и целевом экземплярах сервера установлены разные версии [средств разбиения по словам и парадигматических модулей](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) или [фильтров полнотекстового поиска](../../relational-databases/search/configure-and-manage-filters-for-search.md) , то функциональность полнотекстового индекса и запросов также может отличаться. Кроме того, [тезаурус](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md) хранится в файлах конкретного экземпляра. Нужно либо переместить копию этих файлов в соответствующее место на целевом экземпляре сервера, либо повторно создать их.  
  
> **ПРИМЕЧАНИЕ.** Когда базу данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] с файлами полнотекстовых каталогов присоединяют к экземпляру сервера [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , то присоединение файлов каталогов выполняется из их предыдущего расположения вместе с другими файлами баз данных, как и в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Дополнительные сведения см. в разделе [Обновление полнотекстового поиска](../../relational-databases/search/upgrade-full-text-search.md).  
  
 Дополнительные сведения см. также в следующих разделах:  
  
-   [Создание резервных копий и восстановление полнотекстовых каталогов и индексов](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
-   [Зеркальное отображение баз данных и полнотекстовые каталоги (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  

  
##  <a name="jobs"></a><a name="jobs"></a> Задания  
 Если база данных использует задания агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , их необходимо создать повторно на целевом экземпляре сервера. Задания находятся в зависимости от своей среды. Если планируется повторное создание существующего задания на целевом экземпляре сервера, последний должен быть изменен для обеспечения соответствия среде задания на исходном экземпляре сервера. Важными являются следующие факторы среды.  
  
-   Имя входа, используемое заданием  
  
     Для создания или выполнения заданий агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на целевой экземпляр сервера сначала нужно добавить необходимые ему имена входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Настройка пользователя для создания заданий агента SQL Server и управления заданиями](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Стартовая учетная запись службы агента  
  
     Стартовая учетная запись службы определяет учетную запись [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, с которой запускается агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , а также его сетевые разрешения. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется как заданная учетная запись пользователя. Контекст службы агента влияет на настройки задания и его среды выполнения. Учетной записи должен быть предоставлен доступ к необходимым для задания сетевым и другим ресурсам. Сведения о выборе и изменении стартовой учетной записи службы см. в разделе [Выбор учетной записи для службы агента SQL Server](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md).  
  
     Для обеспечения правильности работы стартовые учетные записи служб должны быть настроены на верный домен, файловую систему и разрешения реестра. Кроме этого, заданию может потребоваться общий сетевой ресурс, который также необходимо настроить для учетной записи службы. Сведения см. в разделе [Настройка учетных записей службы Windows и разрешений](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , связанной с определенным экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имеется собственный раздел реестра, настройки которого могут быть зависимостями заданий этой службы. Чтобы обеспечить требуемую функциональность, заданиям необходимы соответствующие параметры реестра. Если с помощью скрипта задание создается повторно для другой службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , у нее может не оказаться необходимых для этого задания параметров. Чтобы обеспечить требуемую функциональность заданий, повторно созданных на целевом экземпляре сервера, у исходной и целевой служб агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должны быть одинаково настроены параметры реестра.  
  
    > [!CAUTION]  
    >  Изменение настроек реестра целевой службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для обработки повторно созданного задания может вызвать проблемы, если текущие настройки используются другими заданиями. Кроме того, неправильное изменение реестра может серьезно повредить систему. Перед внесением изменений в реестр рекомендуется создать резервную копию всех важных данных.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Учетные записи-посредники агента  
  
     Учетная запись-посредник агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определяет контекст безопасности для указанного шага задания. Для задания, выполняющегося на целевом экземпляре сервера, все необходимые заданию учетные записи-посредники должны быть повторно созданы на этом экземпляре вручную. Дополнительные сведения см. в разделах [Создание учетной записи-посредника агента SQL Server](../../ssms/agent/create-a-sql-server-agent-proxy.md) и [Устранение неполадок, связанных с многосерверными заданиями, использующими учетные записи-посредники](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md).  
  
 Дополнительные сведения см. также в следующих разделах:  
  
-   [Реализация заданий](../../ssms/agent/implement-jobs.md)  
  
-   [Управление именами входа и заданиями после переключения ролей (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md) (для зеркального отображения базы данных)  
  
-   [Настройка учетных записей службы Windows и разрешений](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) (при установке экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Настройка агента SQL Server](../../ssms/agent/configure-sql-server-agent.md) (при установке экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  
  
 **Просмотр существующих заданий и их свойств**  
  
-   [Наблюдение за активностью заданий](../../ssms/agent/monitor-job-activity.md)  
  
-   [sp_help_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)  
  
-   [View Job Step Information](../../ssms/agent/view-job-step-information.md)  
  
-   [dbo.sysjobs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
  
 **Создание задания**  
  
-   [Создание задания](../../ssms/agent/create-a-job.md)  
  
#### <a name="best-practices-for-using-a-script-to-re-create-a-job"></a>Рекомендуемые методы использования скриптов для повторного создания заданий  
 Рекомендуется начать с написания скрипта для простого задания, затем попробовать создать задание повторно для другой службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и запустить задание, чтобы убедиться в правильности его работы. Это позволит обнаружить несовместимости и попробовать их исправить. Если созданное задание не работает в новой среде, как положено, рекомендуется создать подобное задание, которое будет правильно работать в этой среде.  
  

##  <a name="logins"></a><a name="logins"></a> Имена входа  
 Для подключения к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо верное имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Оно используется при проверке подлинности для определения того, разрешено ли участнику подключаться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Пользователь базы данных, соответствующее имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] которого для экземпляра сервера не определено или задано неправильно, не сможет подключиться к этому экземпляру. Такой пользователь называется *утратившим связь с учетной записью* базы данных на этом экземпляре сервера. Пользователь базы данных может утратить связь с учетной записью после восстановления, присоединения или копирования на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Для создания скрипта для нескольких или всех объектов исходной копии базы данных можно воспользоваться мастером создания скриптов и в диалоговом окне **Выбор параметров скрипта** установить значение **TRUE** для параметра **Внести в скрипт имена входа**.  
  
> **ПРИМЕЧАНИЕ.** Сведения о настройке имен входа для зеркально отображаемой базы данных см. в разделах [Настройка учетных записей входа для зеркального отображения баз данных или групп доступности AlwaysOn (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md) и [Управление именами входа и заданиями после переключения ролей &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
  
##  <a name="permissions"></a><a name="permissions"></a> Permissions  
 При открытии доступа к базе данных на другом экземпляре сервера могут быть применены следующие типы разрешений:  
  
-   Разрешения GRANT, REVOKE или DENY на системные объекты.  
  
-   Разрешения GRANT, REVOKE или DENY на экземпляр сервера (*разрешения уровня сервера*).  
  
### <a name="grant-revoke-and-deny-permissions-on-system-objects"></a>Разрешения GRANT, REVOKE или DENY на системные объекты  
 Разрешения на такие системные объекты, как хранимые процедуры, расширенные хранимые процедуры, функции и представления, хранятся в базе данных **master** и должны быть сконфигурированы на целевом экземпляре сервера.  
  
 Для создания скрипта для нескольких или всех объектов исходной копии базы данных можно воспользоваться мастером создания скриптов и в диалоговом окне **Выбор параметров скрипта** установить значение **TRUE** для параметра **Внести в скрипт разрешения уровня объектов**.  
  
   > [!IMPORTANT]
   > При внесении в скрипт имен входа соответствующие им пароли в скрипт не заносятся. При наличии имен входа, использующих проверку подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , необходимо изменить скрипт на целевом экземпляре сервера.  
  
 Системные объекты отображаются в представлении каталога [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md). Разрешения на доступ к системным объектам отображаются в представлении каталога [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) в базе данных **master**. Сведения о запросе этих представлений каталога и выдаче разрешений на уровне объектов см. в разделе [GRANT, предоставление разрешения на системный объект (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md). Дополнительные сведения см. в разделах [REVOKE, отмена разрешения на системные объекты (Transact-SQL)](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md) и [DENY, запрет разрешений на системные объекты (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md).  
  
### <a name="grant-revoke-and-deny-permissions-on-a-server-instance"></a>Разрешения GRANT, REVOKE или DENY на экземпляр сервера  
 Разрешения в области сервера хранятся в базе данных **master** и должны быть сконфигурированы на целевом экземпляре сервера. Чтобы получить сведения о разрешениях сервера в экземпляре сервера, запросите представление каталога [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) . Сведения об участниках сервера можно получить из представления каталога [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md), а сведения о членстве ролей сервера содержатся в представлении каталога [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) .  
  
 Дополнительные сведения см. в разделах [GRANT, предоставление разрешений на сервер (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md), [REVOKE, отмена разрешений сервера (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md) и [DENY, запрет разрешений на сервере (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md).  
  
#### <a name="server-level-permissions-for-a-certificate-or-asymmetric-key"></a>Разрешения уровня сервера на сертификат или асимметричный ключ  
 На сертификат или асимметричный ключ напрямую выдать разрешения на уровне сервера невозможно. Вместо этого разрешения на уровне сервера выдаются сопоставленному имени входа, созданному специально для указанного сертификата или асимметричного ключа. Поэтому для каждого сертификата или асимметричного ключа, требующего разрешений на уровне сервера, необходимо наличие *имени входа, сопоставленного с сертификатом* , или *имени входа, сопоставленного с асимметричным ключом*. Разрешение на сертификат или асимметричный ключ предоставляется сопоставленному с ним имени входа.  
  
> **ПРИМЕЧАНИЕ.** Сопоставленное имя входа используется только для проверки правильности кода, подписанного соответствующим сертификатом или асимметричным ключом. Сопоставленные учетные записи не могут быть использованы для проверки правильности.  
  
 Как сами имена входа, так и выданные им разрешения хранятся в базе данных **master**. Если сертификат или асимметричный ключ находятся в иной базе данных, нежели в базе данных **master**, необходимо создать их копии в базе данных **master** и сопоставить с новым именем входа. При перемещении, копировании или восстановлении базы данных в другом экземпляре сервера необходимо повторно создать ее сертификат или асимметричный ключ в базе данных **master** в целевом экземпляре сервера, сопоставить его с новым именем входа и выдать последнему требуемые разрешения.  
  
 **Создание сертификата или асимметричного ключа**  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
 **Сопоставление сертификата или асимметричного ключа**  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
 **Выдача разрешений сопоставленному имени входа**  
  
-   [GRANT, предоставление разрешений на сервер (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)  
  
 Дополнительные сведения о сертификатах и асимметричных ключах см. в разделе [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
## <a name="trustworthy-property"></a>Свойство TRUSTWORTHY
Свойство TRUSTWORTHY базы данных используется для указания того, доверяет ли этот экземпляр SQL Server базе данных и ее содержимому. При подключении базы данных в целях безопасности это свойство по умолчанию устанавливается в значение OFF, даже если на исходном сервере оно имело значение ON. Дополнительные сведения об этом свойстве см. в статье [Свойство TRUSTWORTHY базы данных](../security/trustworthy-database-property.md). Сведения о его включении см. в статье [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  


##  <a name="replication-settings"></a><a name="replication_settings"></a> Replication Settings  
 Если восстановить резервную копию реплицированной базы данных на другом сервере или в другой базе данных, то станет невозможным сохранение настроек репликации. В этом случае после восстановления из резервной копии потребуется повторно создать все публикации и подписки. Для облегчения этого процесса можно создать скрипты для текущих настроек репликации, а также для разрешения и отключения репликации. Чтобы облегчить повторное создание всех настроек репликации, произведите копирование этих скриптов и измените в них имя сервера для работы с целевым экземпляром.  
  
 Дополнительные сведения см. в разделах [Создание резервных копий реплицируемых баз данных и восстановление из них](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md), [Зеркальное отображение и репликация баз данных (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md) и [Репликация и доставка журналов (SQL Server)](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
  
##  <a name="service-broker-applications"></a><a name="sb_applications"></a> Service Broker Applications  
 Многие аспекты приложения компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)] перемещаются вместе с базой данных. Однако некоторые из них в новом местоположении необходимо создать или настроить повторно.  По умолчанию в целях безопасности параметры *is_broker_enabled* и *is_honor_broker_priority_on* устанавливаются в значение OFF при подключении базы данных с другого сервера. Сведения о том, как установить эти параметры в значение ON, см. в статье [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
  
##  <a name="startup-procedures"></a><a name="startup_procedures"></a> Startup Procedures  
 Стартовыми являются хранимые процедуры, помеченные для автоматического выполнения и выполняемые каждый раз при запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Если в базе данных имеются зависимости от автоматически запускаемых процедур, их необходимо определить на целевом экземпляре сервера и сконфигурировать для автоматического выполнения при запуске.  

  
##  <a name="triggers-at-server-level"></a><a name="triggers"></a> Triggers (at Server Level)  
 Триггеры DDL вызывают хранимые процедуры в ответ на ряд событий языка DDL. Эти события в основном соответствуют инструкциям языка [!INCLUDE[tsql](../../includes/tsql-md.md)] , начинающимся ключевыми словами CREATE, ALTER или DROP. Системные хранимые процедуры, выполняющие операции, подобные операциям DDL, также могут запускать триггеры DDL.  
  
 Дополнительные сведения об этой возможности см. в разделе [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md).  
  
  
## <a name="see-also"></a>См. также:  
 [Автономные базы данных](../../relational-databases/databases/contained-databases.md)   
 [Копирование баз данных на другие серверы](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Присоединение и отсоединение базы данных (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Переход на вторичный сервер доставки журналов (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)   
 [Переключение ролей во время сеанса зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Настройка зашифрованной зеркальной базы данных](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Диспетчер конфигурации SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Диагностика пользователей, утративших связь с учетной записью (SQL Server)](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
  
