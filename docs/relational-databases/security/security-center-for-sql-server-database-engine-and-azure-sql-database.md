---
title: Документация по безопасности SQL Server и базы данных SQL Azure
description: Справочник по безопасности и защите для SQL Server и базы данных SQL Azure.
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799f00bfd7ddf0763212cefb00d8fea72a7dce27
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739924"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Центр обеспечения безопасности для базы данных Azure SQL и SQL Server Database Engine
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  На этой странице представлены ссылки, помогающие найти сведения, касающиеся безопасности и защиты в компоненте [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 **Условные обозначения**  
  
 ![Снимок экрана: условные обозначения, поясняющие значение значков доступности компонентов.](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> Проверка подлинности: кто вы?  
  
|||  
|-|-|  
|**Кто выполняет проверку подлинности?**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Проверка подлинности Windows<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Active Directory|Кто выполняет проверку подлинности? (Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Выбор режима проверки подлинности](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Подключение к базе данных SQL с использованием аутентификации Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)|  
|**Где выполняется проверка подлинности?**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: В базе данных master: имена для входа и пользователи базы данных<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: В пользовательской базе данных: включенные пользователи базы данных|Аутентификация в базе данных master (имена для входа и пользователи базы данных)<br /><br /> [создать имя входа SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Управление базами данных и учетными записями в Базе данных SQL Azure](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [Создание пользователя базы данных](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> Проверка подлинности в пользовательской базе данных<br /><br /> [Пользователи автономной базы данных: создание переносимой базы данных](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Использование других идентификаторов**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Учетные данные<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Выполнение в контексте другого имени входа<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Выполнение от имени другого пользователя базы данных|[Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Выполнение в контексте другого имени входа](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Выполнение от имени другого пользователя базы данных](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> Авторизация: что можно делать?  
  
|||  
|-|-|  
|**Предоставление, отмена и запрет разрешений**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Защищаемые классы<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Детализированные разрешения SQL Server<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Детализированные разрешения базы данных|[Иерархия разрешений (компонент Database Engine)](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [Разрешения](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Защищаемые объекты](../../relational-databases/security/securables.md)<br /><br /> [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Роли безопасности**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Роли уровня сервера<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Роли уровня базы данных|[Роли уровня сервера](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Роли уровня базы данных](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Ограничение доступа к данным для выбранных элементов**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Ограничение доступа к данным с помощью представлений и процедур<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Безопасность на уровне строк<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Динамическое маскирование данных<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Подписанные объекты|Ограничение доступа к данным с помощью [представлений](../../relational-databases/views/views.md) и [процедур](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)<br /><br /> [Безопасность на уровне строк (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [Безопасность на уровне строк (база данных SQL Azure)](./row-level-security.md)<br /><br /> [Динамическое маскирование данных (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Динамическое маскирование данных (база данных Azure SQL)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [Подписанные объекты](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> Шифрование: хранение секретных данных  
  
|||  
|-|-|  
|**Шифрование файлов**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Шифрование BitLocker (уровень диска)<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Шифрование NTFS (уровень папки)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Прозрачное шифрование данных (уровень файла)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Шифрование резервной копии (уровень файла)|[BitLocker (уровень диска)](https://support.microsoft.com/kb/2855131)<br /><br /> [Шифрование NTFS (уровень папки)](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [Прозрачное шифрование данных (уровень файла)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Шифрование резервной копии (уровень файла)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Шифрование источников**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Расширяемый модуль управления ключами<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Ключи, хранящиеся в Azure Key Vault<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[Расширяемый модуль управление ключами](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Ключи, хранящиеся в хранилище ключей Azure](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Шифрование столбцов, данных и ключей**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Шифрование по сертификату<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Шифрование симметричным ключом<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Шифрование асимметричным ключом<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Шифрование с парольной фразой|[Шифрование по сертификату](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Шифрование асимметричным ключом](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Шифрование симметричным ключом](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Шифрование с парольной фразой](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Шифрование столбца данных](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> Безопасность подключения: ограничения и обеспечение безопасности  
  
|||  
|-|-|  
|**Защита с помощью брандмауэра**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Параметры брандмауэра Windows<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Параметры брандмауэра службы Azure<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Параметры брандмауэра базы данных|[Настройка брандмауэра Windows для доступа к компоненту Database Engine](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Параметры брандмауэра базы данных Azure SQL](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Параметры брандмауэра службы Azure](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Шифрование данных при передаче**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Принудительные подключения SSL<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Необязательные подключения SSL|[Включение зашифрованных соединений для ядра СУБД](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [Включение зашифрованных соединений для ядра СУБД](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md), [Сетевая безопасность](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [Поддержка TLS 1.2 для Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> Аудит: регистрация доступа  
  
|||  
|-|-|  
|**Автоматизированный аудит**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: Аудит [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (уровень сервера и базы данных)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Аудит [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (уровень базы данных)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Обнаружение угроз| <br /><br /> [Подсистема аудита SQL Server (компонент Database Engine)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [Аудит базы данных SQL](/azure/azure-sql/database/auditing-overview)<br /><br /> [Приступая к работе с Расширенной защитой от угроз для базы данных SQL](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [Оценка уязвимостей базы данных SQL](/azure/sql-database/sql-vulnerability-assessment) |  
|**Пользовательский аудит**<br /><br /> Триггеры :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::|Реализация пользовательского аудита: Создание [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) и [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**Соответствие**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Соответствие требованиям|SQL Server: стандарт<br />                        [Common Criteria](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> База данных SQL:<br />                        [Центр управления безопасностью Microsoft Azure: соответствие функций нормативным требованиям](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> Атака путем внедрения кода SQL  
 Внедрение кода SQL — это атака, при которой производится вставка вредоносного кода в строки, передающиеся затем в [!INCLUDE[ssDE](../../includes/ssde-md.md)] для анализа и выполнения. Любая процедура, создающая инструкции SQL, должна рассматриваться на предмет уязвимости к вставке небезопасного кода, поскольку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет все получаемые синтаксически правильные запросы. Все системы базы данных подвержены риску атак путем внедрения кода SQL, и многие уязвимости представлены в приложении, которое запрашивает [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Можно предотвратить атаки путем внедрения кода SQL, используя хранимые процедуры и параметризованные команды, избегая использования динамического кода SQL и ограничив разрешения для всех пользователей.  Дополнительные сведения см. в разделе [SQL Injection](../../relational-databases/security/sql-injection.md).  
  
 Дополнительные ссылки для программистов приложений.  
  
-   [Сценарии безопасности приложений в SQL Server](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [Написание безопасного динамического кода SQL в SQL Server](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [Руководство. Предотвращение внедрения SQL в ASP.NET](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>См. также:  
 [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Обеспечение безопасности SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Сертификаты SQL Server и асимметричные ключи](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [Шифрование SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Настройка контактной зоны](../../relational-databases/security/surface-area-configuration.md)   
 [Надежные пароли](../../relational-databases/security/strong-passwords.md)   
 [Свойство базы данных TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Функции и задачи компонента Database Engine](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [Защита интеллектуальной собственности SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]