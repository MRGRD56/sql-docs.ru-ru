---
title: Журнал ошибок и сведений соединителя SQL Server
description: В этой статье описывается включение журнала ошибок и сведений для соединителя SQL Server путем изменения записей реестра
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 53e0a4957fdb3f60f63d82c2ba89b3a98af16887
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492105"
---
# <a name="sql-server-connector-error-and-information-logging"></a>Журнал ошибок и сведений соединителя SQL Server

В этой статье описывается изменение записей реестра для включения журнала ошибок и сведений соединителя SQL Server.

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>Соединитель SQL Server для хранилища ключей Microsoft Azure

[Соединитель SQL Server для Microsoft Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45344) обеспечивает шифрование SQL Server для использования Microsoft Azure Key Vault в качестве поставщика расширенного управления ключами (EKM) для защиты ключей шифрования.

[Загружаемые файлы](https://www.microsoft.com/download/details.aspx?id=45344) состоят из соединителя SQL Server, а также примеров сценариев, позволяющих администратору SQL Server научиться настраивать соединитель SQL Server и использовать шифрование SQL Server. Дополнительные сведения см. в статье [Расширенное управление ключами с помощью Azure Key Vault (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md).

Задать вопросы, поделиться ценными сведениями и обсудить соединитель SQL Server можно на форуме по [Azure Key Vault](https://social.msdn.microsoft.com/Forums/AzureKeyVault).

> [!NOTE]
> Во время нормальной работы соединитель библиотека DLL SQL Server динамически создает записи реестра для установки подключения к Azure Key Vault, чтобы создать ключ `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]`. Учетная запись для запуска SQL Server должна иметь права локального администратора или его служебная учетная запись должна быть **NT SERVICE\MSSQLSERVER**.

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>Обновление соединителя SQL Server до последней версии

Чтобы обновить соединитель SQL Server (версия: 1.0.5.0, дата: сентябрь 2020 г.) до последней версии библиотеки DLL поставщика служб шифрования, выполните следующие действия.

### <a name="upgrade"></a>Обновление

1. Остановите работу службы SQL Server с помощью диспетчера конфигурации SQL Server
1. Удалите старую версию с помощью раздела **Панель управления\Программы\Программы и компоненты**
    1. Имя приложения: Соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.300.96
    1. Дата файла DLL: 01.30.2018 15:00
1. Установите (обновите) новый соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.2000.367
    1. Дата файла DLL: 09.11.2020 05:17
1. Запустите службу SQL Server
1. Проверьте, что зашифрованные базы данных доступны

### <a name="rollback"></a>Откат

1. Остановите работу службы SQL Server с помощью диспетчера конфигурации SQL Server
1. Удалите новую версию с помощью раздела **Панель управления\Программы\Программы и компоненты**
    1. Имя приложения: Соединитель SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.2000.367
    1. Дата файла DLL: 09.11.2020 05:17
1. Установите старую версию соединителя SQL Server для хранилища ключей Microsoft Azure
    1. Версия: 15.0.300.96
    1. Дата файла DLL: 01.30.2018 15:00
1. Запустите службу SQL Server
1. Проверьте, что зашифрованные базы данных доступны

> [!NOTE]
> - Версии соединителя SQL Server 1.0.0.440 и старше были заменены и больше не поддерживаются в рабочих средах. Дополнительные сведения об устранении неполадок с соединителем SQL Server см. в разделе [Обслуживание и устранение неполадок соединителя SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).
> - Начиная с версии 1.0.3.0, соединитель SQL Server передает соответствующие сообщения об ошибках в журналы событий Windows для устранения неполадок.
> - Начиная с версии [1.0.4.0 (версия 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi) доступна поддержка частных облаков Azure, включая Azure для Китая, Azure для Германии и Azure для государственных организаций.
> - В версии 1.0.5.0 внесено критическое изменение, касающееся алгоритма отпечатков. После обновления до версии 1.0.5.0 восстановление базы данных может завершаться сбоем. Дополнительные сведения см. в [статье базы знаний 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - **Начиная с версии 1.0.5.0 (с датой файла сентябрь 2020 г.) соединитель SQL Server поддерживает фильтрацию сообщений и логику повторных попыток сетевого запроса.**
> - *Устаревшей также является версия соединителя SQL Server: [1.0.5.0 (версия 15.0.300.96), дата файла январь 2018 г.](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)* . Выполните обновление до последней версии соединителя SQL Server при возникновении каких либо проблем.

**Требования к системе** — поддерживаемые версии SQL Server:

- SQL Server 2019 RTM Enterprise или Standard, 64-разрядная версия;
- SQL Server 2017 RTM Enterprise, 64-разрядная версия
- SQL Server 2016 RTM Enterprise, 64-разрядная версия
- SQL Server 2014 RTM Enterprise, 64-разрядная версия
- SQL Server 2012 SP2 Enterprise, 64-разрядная версия
- SQL Server 2012 SP1 CU6 Enterprise, 64-разрядная версия
- SQL Server 2008 R2 SP2 CU8 Enterprise, 64-разрядная версия

В SQL Server 2008 и 2012 версий ниже перечисленных выше, необходимо установить исправление, указанное в следующей статье базы знаний: [https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713).

Для соединителя SQL Server для Microsoft Azure Key Vault также требуется .NET версии 4.5.1 на виртуальной машине в Azure с Microsoft SQL Server. Эту библиотеку следует установить перед установкой соединителя SQL Server.

Установите подходящую версию Распространяемого компонента Visual Studio C++ с учетом используемой версии SQL Server:

- Для SQL Server версий 2008, 2008 R2, 2012 и 2014 установите Распространяемый компонент Visual C++ 2013.

- Для SQL Server 2016 установите Распространяемый компонент Visual C++ 2015.

## <a name="modify-windows-registry-steps"></a>Изменение реестра Windows

Измените записи реестра, чтобы включить события журнала ошибок и сведений соединителя SQL Server в журнале событий приложений Windows.

> [!CAUTION]
> Выполняйте действия, описанные в этом разделе, осторожно и на свой страх и риск. При неправильном изменении реестра могут возникнуть серьезные проблемы. Прежде чем вносить изменения в реестр, [выполните резервное копирование реестра для восстановления](https://support.microsoft.com/help/322756) на случай возникновения проблем.

1. В Windows 10 открыть редактор реестра можно двумя способами:
    - В поле поиска на панели задач введите **regedit**. Затем выберите верхний результат "Редактор реестра (классическое приложение)".

    ![открытие regedit для](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "открытие regedit для")
    - Нажмите и удерживайте или щелкните правой кнопкой мыши кнопку "Пуск", а затем выберите пункт "Выполнить". Введите **regedit** в диалоговом окне и нажмите кнопку **ОК**.

   ![запуск regedit для](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "запуск regedit для")

1. Перейдите к следующему разделу реестра:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\**

    ![ekm regedit akv](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "ekm regedit akv")  

1. Добавьте в **Azure Key Vault** новый раздел с именем `Log`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "ekm regedit akv")  

1. В разделе **Log** добавьте значение типа DWORD (32 бит) с именем `Level`:

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![ekm regedit akv log dword](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "ekm regedit akv log dword")  

1. Задайте значение DWORD в соответствие с желаемым уровнем ведения журнала (0, 1, 2):
   1. 0 (информация) — **по умолчанию**
   1. 1 (ошибки)
   1. 2 (без журнала)

   ![ekm regedit akv уровень ведения журнала](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "ekm regedit akv уровень ведения журнала")  

Записи реестра, описанные в этой статье, находятся в следующем разделе:

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

При необходимости для создания ключа можно использовать командную строку:

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

Записи журнала событий приложений, в которых отсутствуют сообщения, можно также исправить с помощью записи в реестре. В журнале событий может содержаться сообщение `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`.  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>Похожие статьи

- Дополнительные примеры сценариев см. в блоге [Прозрачное шифрование данных и расширенное управление ключами SQL Server с помощью Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).
- [Расширенное управление ключами (EKM)](extensible-key-management-ekm.md)  
- [Расширенное управление ключами с помощью Azure Key Vault](extensible-key-management-using-azure-key-vault-sql-server.md)
- [Соединитель SQL Server, приложение](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)
