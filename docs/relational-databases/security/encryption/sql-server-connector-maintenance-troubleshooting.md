---
title: Обслуживание и устранение неполадок соединителя SQL Server
description: Узнайте, как выполнять обслуживание и устранение общих неполадок соединителя SQL Server.
ms.custom: seo-lt-2019
ms.date: 10/08/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7bbd8aee97ccd31649661032c76729ad89b7fe5a
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622760"
---
# <a name="sql-server-connector-maintenance--troubleshooting"></a>Соединитель SQL Server, приложение

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Эта статья содержит вспомогательные сведения о Соединителе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения о Соединителе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] см. в статьях [Расширенное управление ключами с помощью хранилища ключей Azure &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Этапы настройки расширенного управления ключами с использованием хранилища ключей Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) и [Использование Соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
##  <a name="a-maintenance-instructions-for-ssnoversion-connector"></a><a name="AppendixA"></a> A. Инструкции по обслуживанию Соединителя [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
### <a name="key-rollover"></a>Смена ключей  
  
> [!IMPORTANT]  
> Соединитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] требует, чтобы имя ключа содержало только символы a–z, A–Z, 0–9 и "-" и имело длину не более 26 знаков.
> Разные версии ключа, имеющие одинаковое имя в хранилище ключей Azure, не будут работать с Соединителем [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Чтобы сменить ключ хранилища ключей Azure, используемый [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], необходимо создать ключ с новым именем.  
  
 Асимметричные ключи сервера для шифрования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], как правило, должны меняться каждые 1–2 года. Следует отметить, что несмотря на то, что хранилище ключей предоставляет возможность управления версиями ключей, клиентам не следует использовать эту функцию для реализации управления версиями. Соединитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не может работать с изменениями в версии ключа хранилища ключей. Чтобы реализовать смену версии ключа, создайте новый ключ в хранилище ключей и повторно зашифруйте ключ шифрования данных в [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 При прозрачном шифровании данных это можно сделать следующим образом.  
  
- **С помощью PowerShell**. Создайте в Key Vault асимметричный ключ (его имя должно отличаться от текущего асимметричного ключа прозрачного шифрования данных).  
  
    ```powershell  
    Add-AzKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
- **С помощью [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] или sqlcmd.exe**. Используйте следующие инструкции, как показано на шаге 3 в разделе 3.  
  
     Импортируйте новый асимметричный ключ.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]
    FROM PROVIDER [EKM]
    WITH PROVIDER_KEY_NAME = 'Key2',
    CREATION_DISPOSITION = OPEN_EXISTING
    GO  
    ```  
  
     Создайте новое имя входа, которое должно быть связано с новым асимметричным ключом (как показано в разделе с инструкциями по прозрачному шифрованию данных).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Создайте учетные данные, сопоставляемые с новым именем входа.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789='
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Выберите базу данных, ключ шифрования которой требуется повторно зашифровать.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Повторно зашифруйте ключ шифрования базы данных.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-ssnoversion-connector"></a>Обновление Соединителя [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Версии 1.0.0.440 и старше были заменены и больше не поддерживаются в рабочих средах. Версии 1.0.1.0 и более новые поддерживаются в рабочих средах. Воспользуйтесь приведенными ниже инструкциями, чтобы выполнить обновление до последней версии в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=45344).

### <a name="upgrade"></a>Обновление

1. Остановите работу службы SQL Server с помощью диспетчера конфигурации SQL Server
1. Удалите старую версию с помощью раздела "Панель управления\Программы\Программы и компоненты"
    1. Имя приложения: Соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.300.96 (или более старая)
    1. Дата файла DLL: 01.30.2018 15:00 (или старше)
1. Установите (обновите) новый соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.2000.367
    1. Дата файла DLL: 09.11.2020 05:17
1. Запустите службу SQL Server
1. Проверьте, что зашифрованные базы данных доступны

### <a name="rollback"></a>Откат

1. Остановите работу службы SQL Server с помощью диспетчера конфигурации SQL Server

1. Удалите новую версию с помощью раздела "Панель управления\Программы\Программы и компоненты"
    1. Имя приложения: Соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.2000.367
    1. Дата файла DLL: 09.11.2020 05:17

1. Установите старую версию соединителя SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.300.96
    1. Дата файла DLL: 01.30.2018 15:00
1. Запустите службу SQL Server

1. Проверьте, что базы данных, использующие прозрачное шифрование, доступны.  
  
1. После проверки работы обновления можно удалить папку старого соединителя [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (если вы решили переименовать его вместо удаления в шаге 3).

### <a name="older-versions-of-the-sql-server-connector"></a>Старые версии соединителя SQL Server
  
Прямые ссылки на старые версии соединителя SQL Server

- Актуально. [1.0.5.0 (версия 15.0.2000.367), дата файла 11 сентября 2020 г.](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/1033_15.0.2000.367/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.5.0 (версия 15.0.300.96), дата файла 30 января 2018 г.](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.4.0 (версия 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)

### <a name="rolling-the-ssnoversion-service-principal"></a>Смена субъекта-службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использует субъекты-службы, созданные в Azure Active Directory, в качестве учетных данных для доступа к хранилищу ключей. Субъект-служба имеет идентификатор клиента и ключ проверки подлинности. Учетные данные [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] настраиваются с использованием параметра **VaultName**, **идентификатора клиента** и **ключа проверки подлинности**. **Ключ проверки подлинности** действителен в течение определенного периода времени (1–2 года). До истечения этого периода времени в Azure AD должен быть создан новый ключ для субъекта-службы. Затем учетные данные необходимо изменить в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] поддерживает кэш учетных данных в текущем сеансе, поэтому при изменении учетных данных [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] следует перезапустить.  
  
### <a name="key-backup-and-recovery"></a>Резервное копирование и восстановление ключей

Для хранилища ключей необходимо регулярно создавать резервную копию. В случае утери асимметричного ключа в хранилище его можно восстановить из резервной копии. Восстанавливать ключ следует с тем же именем, которое он имел раньше. Именно это и делает команда Restore PowerShell (см. шаги, описанные ниже).  
Если хранилище было утрачено, необходимо заново создать хранилище и восстановить асимметричный ключ к хранилищу, используя то же имя, какое было раньше. Имя хранилища может отличаться (можно сохранить прежнее). Установите разрешения доступа для нового хранилища, чтобы предоставить субъекту-службе SQL Server доступ, необходимый для сценариев шифрования SQL Server, а затем настроить учетные данные SQL Server так, чтобы они отражали новое имя хранилища.

В целом необходимо выполнить следующие действия.  
  
- Создать резервную копию ключа хранилища (с помощью командлета PowerShell Backup-AzureKeyVaultKey).  
- В случае сбоя хранилища создать новое хранилище в той же географической области. Пользователь, создающий это хранилище, должен быть в том же каталоге по умолчанию, который был настроен субъектом-службой для SQL Server.  
- Восстановить ключ к новому хранилищу с помощью командлета PowerShell Restore-AzureKeyVaultKey, который восстановит ключ с прежним именем. Если ключ с таким именем уже существует, восстановление завершается со сбоем.  
- Предоставить разрешения субъекту-службе SQL Server на использование этого нового хранилища.
- Изменить учетные данные SQL Server, используемые ядром СУБД, чтобы отразить новое имя хранилища (при необходимости).  
  
Резервные копии ключей можно восстанавливать в разных регионах Azure при условии, что они остаются в одной географической области или одном национальном облаке: в США, Канаде, Японии, Австралии, Индии, Азиатско-Тихоокеанском регионе, Европе, Бразилии, Китае, Германии или регионе US Gov.  
  
##  <a name="b-frequently-asked-questions"></a><a name="AppendixB"></a> Б. Часто задаваемые вопросы

### <a name="on-azure-key-vault"></a>В хранилище ключей Azure
  
**Как выполняются основные операции с хранилищем ключей Azure?**  
 Асимметричный ключ в хранилище ключей используется для защиты ключей шифрования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Только открытая часть асимметричного ключа покидает хранилище, закрытая же часть никогда не экспортируется. Все криптографические операции, использующие асимметричный ключ, выполняются в службе хранилища ключей Azure и защищаются ее системой безопасности.  
  
 **Что такое URI ключа?**  
 Каждый ключ в хранилище ключей Azure имеет универсальный код ресурса (URI), который можно использовать для ссылки на ключ из приложения. Используйте формат `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` для получения текущей версии и формат `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` для получения конкретной версии.  
  
### <a name="on-configuring-ssnoversion"></a>При настройке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**К каким конечным точкам нужен доступ соединителю SQL Server?**
Соединитель взаимодействует с двумя конечными точками, которые необходимо внести в список разрешений. Для исходящей связи с этими службами нужен только один порт: 443 (HTTPS).

- login.microsoftonline.com/*:443
- *.vault.azure.net/* :443

**Как подключиться к Azure Key Vault через прокси-сервер HTTP(S)?**
Соединитель использует параметры конфигурации прокси-сервера Internet Explorer. Эти параметры можно изменять в разделе [Групповая политика](/archive/blogs/askie/how-to-configure-proxy-settings-for-ie10-and-ie11-as-iem-is-not-available) или через реестр, но важно отметить, что они не действуют на уровне системы и направлены на учетную запись службы, на которой выполняется экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если администратор базы данных просматривает или редактирует параметры в Internet Explorer, это повлияет только на учетную запись администратора базы данных, а не на подсистему [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Интерактивное ведение журнала с использованием учетной записи службы не рекомендуется и блокируется во многих безопасных средах. Чтобы изменения настроенных параметров прокси-сервера вступили в силу, может потребоваться перезапуск экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], когда соединитель впервые пытается подключиться к хранилищу ключей.

**Какие размеры ключей в Azure Key Vault поддерживает соединитель SQL Server?**
Последняя сборка соединителя SQL Server поддерживает ключи Azure Key Vault размером 2048 и 3072.
  
 > [!NOTE] 
 > Представление "sys.asymmetric_keys" указывает размер ключа как 2048, даже если используется размер ключа 3072. Это известный недочет в данном представлении, и команда разработчиков SQL Server устранит его в будущем выпуске.

**Какие минимальные уровни разрешений необходимы для каждого этапа конфигурации в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]?**  
 Хотя все действия по настройке можно выполнить от имени члена предопределенной роли сервера sysadmin, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] рекомендует минимизировать используемые разрешения. В приведенном ниже списке указаны минимальные уровни разрешений для каждого действия.  
  
- Для создания поставщика служб шифрования требуется разрешение `CONTROL SERVER` или членство в предопределенной роли сервера **sysadmin** .  
  
- Для изменения параметра конфигурации и выполнения инструкции `RECONFIGURE` должно быть предоставлено разрешение `ALTER SETTINGS` на уровне сервера. Разрешение `ALTER SETTINGS` неявным образом предоставлено предопределенным ролям сервера sysadmin и **serveradmin** .  
  
- Для создания учетных данных требуется разрешение `ALTER ANY CREDENTIAL` .  
  
- Для добавления учетных данных к имени входа требуется разрешение `ALTER ANY LOGIN` .  
  
- Для создания асимметричного ключа требуется разрешение `CREATE ASYMMETRIC KEY` .  

**Как изменить Active Directory по умолчанию, чтобы хранилище ключей было создано для той же подписки и того же Active Directory, что и субъект-служба, созданный для соединителя [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] ?**

![шаги по изменению каталога aad по умолчанию](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Перейдите на классический портал Azure: [https://manage.windowsazure.com](https://manage.windowsazure.com)  
2. В левом меню щелкните **Параметры**.
3. Выберите подписку Azure, которую используете в данный момент, и щелкните элемент **Изменить каталог** в области команд в нижней части экрана.
4. Во всплывающем окне воспользуйтесь раскрывающимся списком **Каталог** для выбора нужного Active Directory. При этом данный каталог становится используемым по умолчанию.
5. Убедитесь, что вы являетесь глобальным администратором для вновь выбранного Active Directory. Если вы не являетесь глобальным администратором, то можете потерять разрешения на управление в результате смены каталогов.
6. Если после закрытия всплывающего окна вы не видите ни одной из своих подписок, возможно, требуется изменить элемент **Фильтровать по каталогу** фильтра **Подписки**, который расположен в правом верхнем меню, чтобы просмотреть подписки с использованием обновленной службы Active Directory.

    > [!NOTE] 
    > У вас могут отсутствовать разрешения на смену каталога по умолчанию в рамках своей подписки Azure. В этом случае создайте субъект-службу AAD в своем каталоге по умолчанию, чтобы он находился в одном каталоге с хранилищем ключей, которое планируется использовать.

Дополнительные сведения об Active Directory см. в разделе [How Azure subscription are related to Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)(Как подписка Azure связана с Azure Active Directory).
  
##  <a name="c-error-code-explanations-for-ssnoversion-connector"></a><a name="AppendixC"></a> В. Описания кодов ошибок для Соединителя [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

**Коды ошибок поставщика:**  
  
Код ошибки  |Символ  |Описание
---------|---------|---------  
0 | scp_err_Success | Операция завершилась успешно.
1 | scp_err_Failure | Операция завершилась ошибкой.
2 | scp_err_InsufficientBuffer | Эта ошибка сообщает подсистеме, что нужно выделить больше памяти для буфера.
3 | scp_err_NotSupported | Операция не поддерживается. Например, указанный алгоритм или тип ключа не поддерживается поставщиком расширенного управления ключами.
4 | scp_err_NotFound | Поставщику расширенного управления ключами не удалось найти указанный ключ и алгоритм.
5 | scp_err_AuthFailure | Сбой проверки подлинности с поставщиком расширенного управления ключами.
6 | scp_err_InvalidArgument | Предоставлен недопустимый аргумент.
7 | scp_err_ProviderError | В поставщике расширенного управления ключами произошла неопределенная ошибка, перехваченная ядром SQL.
401 | acquireToken | Сервер вернул ответ 401 на запрос. Убедитесь, что идентификатор и секрет клиента верны, а строка учетных данных представляет собой объединение идентификатора и секрета клиента AAD без дефисов.
404 | getKeyByName | Сервер вернут ответ 404, так как не удалось найти имя ключа. Убедитесь, что такое имя ключа присутствует в вашем хранилище.
2049 | scp_err_KeyNameDoesNotFitThumbprint | Имя ключа слишком длинное и не умещается в отпечаток ядра SQL. Длина имени ключа не должна превышать 26 символов.
2050 | scp_err_PasswordTooShort | Секретная строка, являющаяся объединением идентификатора клиента и секрета AAD, короче 32 символов.
2051 | scp_err_OutOfMemory | В ядре SQL возникла нехватка памяти, и не удалось выделить память для поставщика расширенного управления ключами.
2052 | scp_err_ConvertKeyNameToThumbprint | Не удалось преобразовать имя ключа в отпечаток.
2053 | scp_err_ConvertThumbprintToKeyName|  Не удалось преобразовать отпечаток в имя ключа.
2058 | scp_err_FailureInRegistry|  Не удалось выполнить операцию в реестре. Учетная запись службы SQL Server не имеет разрешения на создание раздела реестра.
3000 | ErrorSuccess | Операция с хранилищем ключей Azure завершилась успешно.
3001 | ErrorUnknown | Сбой операции с хранилищем ключей Azure из-за неизвестной ошибки.
3002 | ErrorHttpCreateHttpClientOutOfMemory | Не удается создать HttpClient для операции с Azure Key Vault из-за нехватки памяти.
3003 | ErrorHttpOpenSession | Не удается открыть сеанс HTTP из-за ошибки сети.
3004 | ErrorHttpConnectSession | Не удается подключиться к сеансу HTTP из-за ошибки сети.
3005 | ErrorHttpAttemptConnect | Не удается предпринять попытку подключения из-за ошибки сети.
3006 | ErrorHttpOpenRequest | Не удается открыть запрос из-за ошибки сети.
3007 | ErrorHttpAddRequestHeader | Не удается добавить заголовок запроса.
3008 | ErrorHttpSendRequest | Не удается отправить запрос из-за ошибки сети.
3009 | ErrorHttpGetResponseCode | Не удается получить запрос из-за ошибки сети.
3010 | ErrorHttpResponseCodeUnauthorized | Сервер вернул ответ 401 на запрос.
3011 | ErrorHttpResponseCodeThrottled | Сервер выполнил регулирование запроса.
3012 | ErrorHttpResponseCodeClientError | С соединителя отправлен недопустимый запрос. Обычно это означает, что имя ключа является недопустимым или содержит недопустимые символы.
3013 | ErrorHttpResponseCodeServerError | Сервер возвратил код ответа от 500 до 600.
3014 | ErrorHttpQueryHeader | Не удается запросить заголовок ответа.
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | Не удается скопировать заголовок ответа из-за нехватки памяти.
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | Не удается запросить заголовок ответа из-за нехватки памяти при перераспределении буфера.
3017 | ErrorHttpQueryHeaderNotFound | Не удается найти заголовок запроса в ответе.
3018 | ErrorHttpQueryHeaderUpdateBufferLength | Не удается обновить длину буфера при запросе заголовка ответа.
3019 | ErrorHttpReadData | Не удается считать данные ответа из-за ошибки сети.
3076 | ErrorHttpResourceNotFound | Сервер вернут ответ 404, так как не удалось найти имя ключа. Убедитесь, что такое имя ключа присутствует в вашем хранилище.
3077 | ErrorHttpOperationForbidden | Сервер вернул ответ 403, так как у пользователя нет необходимых разрешений на выполнение данного действия. Убедитесь, что у вас есть разрешения для указанной операции. Для правильной работы соединителю требуются по меньшей мере разрешения get, list, wrapKey, unwrapKey.
3100 | ErrorHttpCreateHttpClientOutOfMemory               | Не удается создать HttpClient для операции с хранилищем ключей Azure из-за нехватки памяти.
3101 | ErrorHttpOpenSession                               | Не удается открыть сеанс HTTP из-за ошибки сети.
3102 | ErrorHttpConnectSession                            | Не удается подключиться к сеансу HTTP из-за ошибки сети.
3103 | ErrorHttpAttemptConnect                            | Не удается предпринять попытку подключения из-за ошибки сети.
3104 | ErrorHttpOpenRequest                               | Не удается открыть запрос из-за ошибки сети.
3105 | ErrorHttpAddRequestHeader                          | Не удается добавить заголовок запроса.
3106 | ErrorHttpSendRequest                               | Не удается отправить запрос из-за ошибки сети.
3107 | ErrorHttpGetResponseCode                           | Не удается получить запрос из-за ошибки сети.
3108 | ErrorHttpResponseCodeUnauthorized                  | Сервер вернул ответ 401 на запрос. Убедитесь, что идентификатор и секрет клиента верны, а строка учетных данных представляет собой объединение идентификатора и секрета клиента AAD без дефисов.
3109 | ErrorHttpResponseCodeThrottled                     | Сервер выполнил регулирование запроса.
3110 | ErrorHttpResponseCodeClientError                    | Недопустимый запрос. Обычно это означает, что имя ключа является недопустимым или содержит недопустимые символы.
3111 | ErrorHttpResponseCodeServerError                   | Сервер возвратил код ответа от 500 до 600.
3112 | ErrorHttpResourceNotFound                          | Сервер вернут ответ 404, так как не удалось найти имя ключа. Убедитесь, что такое имя ключа присутствует в вашем хранилище.
3113 | ErrorHttpOperationForbidden                         | Сервер вернул ответ 403, так как у пользователя нет необходимых разрешений на выполнение данного действия. Убедитесь, что у вас есть разрешения для указанной операции. Минимально требуются разрешения на выполнение операций get, wrapKey, unwrapKey.
3114 | ErrorHttpQueryHeader                               | Не удается запросить заголовок ответа.
3115 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader          | Не удается скопировать заголовок ответа из-за нехватки памяти.
3116 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer       | Не удается запросить заголовок ответа из-за нехватки памяти при перераспределении буфера.
3117 | ErrorHttpQueryHeaderNotFound                       | Не удается найти заголовок запроса в ответе.
3118 | ErrorHttpQueryHeaderUpdateBufferLength             | Не удается обновить длину буфера при запросе заголовка ответа.
3119 | ErrorHttpReadData                                  | Не удается считать данные ответа из-за ошибки сети.
3120 | ErrorHttpGetResponseOutOfMemoryCreateTempBuffer    | Не удается получить текст ответа из-за нехватки памяти при создании временного буфера.
3121 | ErrorHttpGetResponseOutOfMemoryGetResultString     | Не удается получить текст ответа из-за нехватки памяти при получении результирующей строки.
3122 | ErrorHttpGetResponseOutOfMemoryAppendResponse      | Не удается получить текст ответа из-за нехватки памяти при добавлении ответа.
3200 | ErrorGetAADValuesOutOfMemoryConcatPath | Не удается получить значения заголовка запроса Azure Active Directory из-за нехватки памяти при объединении пути.
3201 | ErrorGetAADDomainUrlStartPosition | Не удается найти начальную точку для URL-адреса домена Azure Active Directory в неправильном заголовке запроса ответа.
3202 | ErrorGetAADDomainUrlStopPosition | Не удается найти конечную точку для URL-адреса домена Azure Active Directory в неправильном заголовке запроса ответа.
3203 | ErrorGetAADDomainUrlMalformatted | Неправильный заголовок запроса ответа Azure Active Directory не содержит URL-адрес домена AAD.
3204 | ErrorGetAADDomainUrlOutOfMemoryAlloc | Недостаточно памяти при выделении буфера для URL-адреса домена Azure Active Directory.
3205 | ErrorGetAADTenantIdOutOfMemoryAlloc | Недостаточно памяти при выделении буфера для идентификатора арендатора домена Azure Active Directory.
3206 | ErrorGetAKVResourceUrlStartPosition | Не удается найти начальную точку для URL-адреса ресурса Azure Key Vault в неправильном заголовке запроса ответа.
3207 | ErrorGetAKVResourceUrlStopPosition | Не удается найти конечную точку для URL-адреса ресурса Azure Key Vault в неправильном заголовке запроса ответа.
3208 | ErrorGetAKVResourceUrlOutOfMemoryAlloc | Недостаточно памяти при выделении буфера для URL-адреса ресурса Azure Key Vault.
3300 | ErrorGetTokenOutOfMemoryConcatPath | Не удается получить токен из-за нехватки памяти при объединении пути запроса.
3301 | ErrorGetTokenOutOfMemoryConcatBody | Не удается получить токен из-за нехватки памяти при объединении текста ответа.
3302 | ErrorGetTokenOutOfMemoryConvertResponseString | Не удается получить токен из-за нехватки памяти при преобразовании строки ответа.
3303 | ErrorGetTokenBadCredentials | Не удается получить токен из-за неправильных учетных данных. Убедитесь, что строка учетных данных или сертификат действительны.
3304 | ErrorGetTokenFailedToGetToken | Хотя учетные данные правильны, операция по-прежнему не смогла получить действительный токен.
3305 | ErrorGetTokenRejected | Токен действителен, но отклонен сервером.
3306 | ErrorGetTokenNotFound | Не удается найти токен в ответе.
3307 | ErrorGetTokenJsonParser | Не удается проанализировать ответ JSON сервера.
3308 | ErrorGetTokenExtractToken | Не удается извлечь токен из ответа JSON.
3400 | ErrorGetKeyByNameOutOfMemoryConvertResponseString | Не удается получить ключ по имени из-за нехватки памяти при преобразовании строки ответа.
3401 | ErrorGetKeyByNameOutOfMemoryConcatPath | Не удается получить ключ по имени из-за нехватки памяти при объединении пути.
3402 | ErrorGetKeyByNameOutOfMemoryConcatHeader | Не удается получить ключ по имени из-за нехватки памяти при объединении заголовка.
3403 | ErrorGetKeyByNameNoResponse | Не удается получить ключ по имени из-за отсутствия ответа от сервера.
3404 | ErrorGetKeyByNameJsonParser | Не удается получить ключ по имени, так как не удалось проанализировать ответ JSON.
3405 | ErrorGetKeyByNameExtractKeyNode | Не удается получить ключ по имени, так как не удалось извлечь узел ключа из ответа.
3406 | ErrorGetKeyByNameExtractKeyId | Не удается получить ключ по имени, так как не удалось извлечь идентификатор ключа из ответа.
3407 | ErrorGetKeyByNameExtractKeyType | Не удается получить ключ по имени, так как не удалось извлечь тип ключа из ответа.
3408 | ErrorGetKeyByNameExtractKeyN | Не удается получить ключ по имени, так как не удалось извлечь N ключа из ответа.
3409 | ErrorGetKeyByNameBase64DecodeN | Не удается получить ключ по имени, так как в Base64 не удалось декодировать N.
3410 | ErrorGetKeyByNameExtractKeyE | Не удается получить ключ по имени, так как не удалось извлечь E ключа из ответа.
3411 | ErrorGetKeyByNameBase64DecodeE | Не удается получить ключ по имени, так как в Base64 не удалось декодировать E.
3412 | ErrorGetKeyByNameExtractKeyUri | Не удается извлечь URI ключа из ответа.
3500 | ErrorBackupKeyOutOfMemoryConvertResponseString | Не удается выполнить резервное копирование ключа из-за нехватки памяти при преобразовании строки ответа.
3501 | ErrorBackupKeyOutOfMemoryConcatPath | Не удается выполнить резервное копирование ключа из-за нехватки памяти при объединении пути.
3502 | ErrorBackupKeyOutOfMemoryConcatHeader | Не удается выполнить резервное копирование ключа из-за нехватки памяти при объединении заголовка запроса.
3503 | ErrorBackupKeyNoResponse | Не удается выполнить резервное копирование ключа из-за отсутствия ответа от сервера.
3504 | ErrorBackupKeyJsonParser | Не удается выполнить резервное копирование ключа, так как не удалось проанализировать ответ JSON.
3505 | ErrorBackupKeyExtractValue | Не удается выполнить резервное копирование ключа, так как не удалось извлечь значение из ответа JSON.
3506 | ErrorBackupKeyBase64DecodeValue | Не удается выполнить резервное копирование ключа, так как в Base64 не удалось декодировать поле значения.
3600 | ErrorWrapKeyOutOfMemoryConvertResponseString | Не удается упаковать ключ из-за нехватки памяти при преобразовании строки ответа.
3601 | ErrorWrapKeyOutOfMemoryConcatPath | Не удается упаковать ключ из-за нехватки памяти при объединении пути.
3602 | ErrorWrapKeyOutOfMemoryConcatHeader | Не удается упаковать ключ из-за нехватки памяти при объединении заголовка.
3603 | ErrorWrapKeyOutOfMemoryConcatBody | Не удается упаковать ключ из-за нехватки памяти при объединении текста.
3604 | ErrorWrapKeyOutOfMemoryConvertEncodedBody | Не удается упаковать ключ из-за нехватки памяти при преобразовании закодированного текста.
3605 | ErrorWrapKeyBase64EncodeKey | Не удается упаковать ключ из-за сбоя кодирования ключа в Base64.
3606 | ErrorWrapKeyBase64DecodeValue | Не удается упаковать ключ из-за сбоя декодирования значения ответа в Base64.
3607 | ErrorWrapKeyJsonParser | Не удается упаковать ключ, так как не удалось проанализировать ответ JSON.
3608 | ErrorWrapKeyExtractValue | Не удается упаковать ключ, так как не удалось извлечь значение из ответа.
3609 | ErrorWrapKeyNoResponse | Не удается упаковать ключ из-за отсутствия ответа от сервера.
3700 | ErrorUnwrapKeyOutOfMemoryConvertResponseString | Не удается распаковать ключ из-за нехватки памяти при преобразовании строки ответа.
3701 | ErrorUnwrapKeyOutOfMemoryConcatPath | Не удается распаковать ключ из-за нехватки памяти при объединении пути.
3702 | ErrorUnwrapKeyOutOfMemoryConcatHeader | Не удается распаковать ключ из-за нехватки памяти при объединении заголовка.
3703 | ErrorUnwrapKeyOutOfMemoryConcatBody | Не удается распаковать ключ из-за нехватки памяти при объединении текста.
3704 | ErrorUnwrapKeyOutOfMemoryConvertEncodedBody | Не удается распаковать ключ из-за нехватки памяти при преобразовании закодированного текста.
3705 | ErrorUnwrapKeyBase64EncodeKey | Не удается распаковать ключ из-за сбоя кодирования ключа в Base64.
3706 | ErrorUnwrapKeyBase64DecodeValue | Не удается распаковать ключ из-за сбоя декодирования значения ответа в Base64.
3707 | ErrorUnwrapKeyJsonParser | Не удается распаковать ключ, так как не удалось извлечь значение из ответа.
3708 | ErrorUnwrapKeyExtractValue | Не удается распаковать ключ, так как не удалось извлечь значение из ответа.
3709 | ErrorUnwrapKeyNoResponse | Не удается распаковать ключ из-за отсутствия ответа от сервера.
3800 | ErrorSecretAuthParamsGetRequestBody | Ошибка при создании текста запроса с использованием идентификатора клиента и секрета AAD.
3801 | ErrorJWTTokenCreateHeader | Ошибка при создании заголовка маркера JWT для проверки подлинности с помощью AAD.
3802 | ErrorJWTTokenCreatePayloadGUID | Ошибка при создании идентификатора GUID для полезных данных токена JWT для проверки подлинности с помощью AAD.
3803 | ErrorJWTTokenCreatePayload | Ошибка при создании полезных данных токена JWT для проверки подлинности с помощью AAD.
3804 | ErrorJWTTokenCreateSignature | Ошибка при создании сигнатуры маркера JWT для проверки подлинности с помощью AAD.
3805 | ErrorJWTTokenSignatureHashAlg | Ошибка при получении алгоритма хэширования SHA256 для проверки подлинности в AAD.
3806 | ErrorJWTTokenSignatureHash | Ошибка при создании хэша SHA256 для проверки подлинности токенов JWT с помощью AAD.
3807 | ErrorJWTTokenSignatureSignHash | Ошибка при подписании хэша маркера JWT для проверки подлинности в AAD.
3808 | ErrorJWTTokenCreateToken | Ошибка при создании маркера JWT для проверки подлинности с помощью AAD.
3809 | ErrorPfxCertAuthParamsImportPfx | Ошибка при импорте сертификата Pfx для проверки подлинности в AAD.
3810 | ErrorPfxCertAuthParamsGetThumbprint | Произошла ошибка при получении отпечатка из сертификата Pfx для проверки подлинности с помощью AAD.
3811 | ErrorPfxCertAuthParamsGetPrivateKey | Произошла ошибка при получении закрытого ключа из сертификата Pfx для проверки подлинности с помощью AAD.
3812 | ErrorPfxCertAuthParamsSignAlg | Ошибка при получении алгоритма подписи RSA для проверки подлинности сертификата Pfx с помощью AAD.
3813 | ErrorPfxCertAuthParamsImportForSign | Ошибка при импорте закрытого ключа Pfx для подписи RSA для проверки подлинности в AAD.
3814 | ErrorPfxCertAuthParamsCreateRequestBody | Ошибка при создании текста запроса из сертификата Pfx для проверки подлинности с помощью AAD.
3815 | ErrorPEMCertAuthParamsGetThumbprint | Ошибка при декодировании отпечатка Base64 для проверки подлинности с помощью AAD.
3816 | ErrorPEMCertAuthParamsGetPrivateKey | Ошибка при получении закрытого ключа RSA из PEM для проверки подлинности в AAD.
3817 | ErrorPEMCertAuthParamsSignAlg | Ошибка при получении алгоритма подписи RSA для проверки подлинности с закрытым ключом PEM с помощью AAD.
3818 | ErrorPEMCertAuthParamsImportForSign | Ошибка при импорте закрытого ключа PEM для подписи RSA для проверки подлинности в AAD.
3819 | ErrorPEMCertAuthParamsCreateRequestBody | Ошибка при создании текста запроса из закрытого ключа PEM для проверки подлинности в AAD.
3820 | ErrorLegacyPrivateKeyAuthParamsSignAlg | Ошибка при получении алгоритма подписи RSA для проверки подлинности с устаревшим закрытым ключом с помощью AAD.
3821 | ErrorLegacyPrivateKeyAuthParamsImportForSign | Ошибка при импорте устаревшего закрытого ключа для подписи RSA для проверки подлинности в AAD.
3822 | ErrorLegacyPrivateKeyAuthParamsCreateRequestBody        | Ошибка при создании текста запроса из устаревшего закрытого ключа для проверки подлинности в AAD.
3900 | ErrorAKVDoesNotExist | Ошибка "Не удалось разрешить интернет-имя". Обычно это означает, что Azure Key Vault удален.
4000 | ErrorCreateKeyVaultRetryManagerOutOfMemory | Не удается создать RetryManager для операции AKV из-за нехватки памяти.

Если вы не видите код ошибки в этой таблице, ниже приведены некоторые другие причины для ее возникновения.
  
- Возможно, у вас отсутствует доступ в Интернет, из-за чего Azure Key Vault недоступно. Проверьте подключение к Интернету.  
  
- Возможно, служба хранилища ключей Azure не работает. Повторите попытку позднее.  
  
- Возможно, вы удалили асимметричный ключ из хранилища ключей Azure или [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Восстановите ключ.  
  
- Если отображается сообщение об ошибке "Не удается загрузить библиотеку", убедитесь, что установлена подходящая с учетом используемой версии SQL Server версия распространяемого пакета Visual Studio C++. В следующей таблице указано, какую версию следует установить из центра загрузки Майкрософт.

Журнал событий Windows также регистрирует ошибки, связанные с соединителем SQL Server, что предоставляет дополнительный контекст для ошибки. Источником в журнале событий приложений Windows будет "Соединитель SQL Server для Microsoft Azure Key Vault".
  
Версия SQL Server  |Ссылка для установки распространяемого пакета
---------|---------
2008, 2008 R2, 2012, 2014 | [Распространяемые пакеты Visual C++ для Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
2016 | [Распространяемый пакет Visual C++ для Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)
  
## <a name="additional-references"></a>Дополнительные ссылки

 Подробнее о расширенном управлении ключами:  
  
- [Расширенное управление ключами &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 Виды шифрования SQL с поддержкой расширенного управления ключами:  
  
- [Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
- [Шифрование резервной копии](../../../relational-databases/backup-restore/backup-encryption.md)  
  
- [Создание зашифрованной резервной копии](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Связанные команды [!INCLUDE[tsql](../../../includes/tsql-md.md)] :  
  
- [sp_configure (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
- [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
- [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
- [CREATE SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Документация по хранилищу ключей Azure.  
  
- [Что такое хранилище ключей Azure?](/azure/key-vault/general/basic-concepts)  
  
- [Приступая к работе с хранилищем ключей Azure](/azure/key-vault/general/overview)  
  
- Справочник по [командлетам PowerShell для работы с хранилищем ключей Azure](/powershell/module/azurerm.keyvault/)  
  
## <a name="see-also"></a>См. также:

 [Расширенное управление ключами с помощью хранилища ключей Azure](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Использование соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)  
 [Включенный параметр конфигурации сервера поставщика расширенного управления ключами](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
 [Этапы настройки расширенного управления ключами с использованием хранилища ключей Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)

Дополнительные примеры сценариев см. в блоге [Прозрачное шифрование данных и расширенное управление ключами SQL Server с помощью Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
