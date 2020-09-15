---
description: Элементы MSReportServer_ConfigurationSetting
title: Члены MSReportServer_ConfigurationSetting | Документы Майкрософт
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- MSReportServer_ConfigurationSetting Members
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- WMI provider [Reporting Services], MSReportServer_ConfigurationSetting class
- MSReportServer_ConfigurationSetting class
ms.assetid: 5e21a53a-a2f8-4b35-a8d9-5483519e3857
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7643435954c74285992c6a21ebd1df5d8998164b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423098"
---
# <a name="msreportserver_configurationsetting-members"></a>Элементы MSReportServer_ConfigurationSetting
  Класс MSReportServer_ConfigurationSetting содержит указанные ниже свойства и методы.  
  
## <a name="public-properties"></a>Открытые свойства  
  
|Свойство|Описание|  
|-|-|  
|[ConnectionPoolSize](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-connectionpoolsize.md)|Возвращает размер пула подключений, который используется сервером отчетов для связи с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], где размещается база данных сервера отчетов. Только для чтения.|  
|[DatabaseLogonAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogonaccount.md)|Задает учетную запись для входа, которую сервер отчетов использует для подключения к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], где размещается база данных сервера отчетов. Только для чтения.|  
|[DatabaseLogonTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogontimeout.md)|Указывает число секунд ожидания перед тем, как попытка входа в базу данных сервера отчетов признается неуспешной. Только для чтения.|  
|[DatabaseLogonType](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogontype.md)|Указывает, использует ли сервер отчетов для доступа к базе данных сервера отчетов учетную запись службы Windows, учетную запись пользователя Windows или имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Только для чтения.|  
|[DatabaseName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databasename.md)|Задает имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , в котором размещается база данных сервера отчетов.|  
|[DatabaseQueryTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databasequerytimeout.md)|Задает число секунд, проходящих до истечения времени ожидания команды или ее завершения с ошибкой. Сервер отчетов определяет время для процесса по базе данных сервера отчетов, а не по источнику данных для отчета.|  
|[DatabaseServerName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaseservername.md)|Задает имя сервера, на котором установлена база данных сервера отчетов.|  
|[Свойство InstallationID](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-installationid.md)|Возвращает уникальный идентификатор для определенного экземпляра сервера отчетов.|  
|[InstanceName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-instancename.md)|Указывает имя экземпляра сервера отчетов на заданном компьютере.|  
|[IsInitialized](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-isinitialized.md)|Указывает, инициализирован ли экземпляр сервера отчетов. Только для чтения.|  
|[IsSharePointIntegrated](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-issharepointintegrated.md)|Показывает, настроен ли сервер отчетов в режиме интеграции для SharePoint.|  
|[IsWebServiceEnabled](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-iswebserviceenabled.md)|Указывает, включена ли веб-служба сервера отчетов. Только для чтения.|  
|[IsWindowsServiceEnabled](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-iswindowsserviceenabled.md)|Указывает, включена ли служба Windows сервера отчетов. Только для чтения.|  
|[Свойство MachineAccountIdentity (WMI)](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-machineaccountidentity.md)|Возвращает удостоверение учетной записи компьютера, на котором установлен сервер отчетов.|  
|[PathName](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-pathname.md)|Задает путь установки для экземпляра сервера отчетов.|  
|[SecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-secureconnectionlevel.md)|Возвращает уровень безопасного соединения, заданный в файле RSReportServer.config.|  
|[SenderEmailAddress](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-senderemailaddress.md)|Возвращает адрес, используемый для отправки электронной почты с сервера отчетов. Только для чтения.|  
|[SendUsingSMTPServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-sendusingsmtpserver.md)|Указывает, имеет ли свойство SendUsing в конфигурации электронной почты значение TRUE.|  
|[SMTPServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-smtpserver.md)|Возвращает свойство SMTP-сервера из файла RSReportServer.config. Только для чтения.|  
|[UnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-unattendedexecutionaccount.md)|Указывает учетную запись пользователя для входа, олицетворение которой выполняет сервер отчетов во время автоматического выполнения отчетов. Только для чтения.|  
|[Версия](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-version.md)|Возвращает версию сервера отчетов.|  
|[Свойство VirtualDirectoryReportManager (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-virtualdirectoryreportmanager.md)|Возвращает виртуальный каталог для приложения диспетчера отчетов.|  
|[Свойство VirtualDirectoryReportServer (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-virtualdirectoryreportserver.md)|Возвращает виртуальный каталог для приложения веб-службы сервера отчетов.|  
|[WindowsServiceIdentityActual](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-windowsserviceidentityactual.md)|Возвращает удостоверение, с которым фактически выполняется служба Windows сервера отчетов. Только для чтения.|  
|[WindowsServiceIdentityConfigured](../../reporting-services/wmi-provider-library-reference/windowsserviceidentityconfigured-property.md)|Возвращает удостоверение, для выполнения с которым была в последний раз настроена служба Windows сервера отчетов. Только для чтения.|  
  
## <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|[BackupEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md)|Создает резервную копию ключа шифрования для экземпляра. Ключ шифрования хранится в зашифрованном виде под защитой пароля.|  
|[CreateSSLCertificateBinding Method (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-createsslcertificatebinding.md)|Создает привязку к TLS/SSL-сертификату.|  
|[DeleteEncryptedInformation](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-deleteencryptedinformation.md)|Удаляет зашифрованные данные из базы данных сервера отчетов.|  
|[DeleteEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-deleteencryptionkey.md)|Удаляет ключи шифрования из базы данных сервера отчетов.|  
|[GenerateDatabaseCreationScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)|Формирует скрипт SQL, который можно использовать для создания базы данных сервера отчетов.|  
|[GenerateDatabaseRightsScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)|Формирует скрипт SQL, с помощью которого можно предоставить пользователю разрешения для базы данных сервера отчетов.|  
|[GenerateDatabaseUpgradeScript](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)|Формирует скрипт SQL, который можно использовать для обновления базы данных сервера отчетов.|  
|[Метод GetAdminSiteUrl (WMI)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-getadminsiteurl.md)|Возвращает абсолютный URL-адрес веб-сайта центра администрирования.|  
|[GetDatabaseVersionDisplayName](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-getdatabaseversiondisplayname.md)|Возвращает отображаемое имя для указанной строки версии базы данных сервера отчетов.|  
|[InitializeReportServer](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-initializereportserver.md)|Инициализирует заданный экземпляр сервера отчетов.|  
|[Метод ListInstalledSharePointVersions (WMI)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listinstalledsharepointversions.md)|Возвращает набор токенов, которые представляют версии [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] [!INCLUDE[offSPServ](../../includes/offspserv-md.md)], [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] или [!INCLUDE[SPS2010](../../includes/sps2010-md.md)], установленные на том же компьютере, что и сервер отчетов.|  
|[Метод ListIPAddresses (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listipaddresses.md)|Выводит список IP-адресов для компьютера.|  
|[ListReportServersInDatabase](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreportserversindatabase.md)|Возвращает список установленных экземпляров сервера отчетов, присутствующих в базе данных сервера отчетов, независимо от того, имеют ли эти экземпляры доступ к защищенным сведениям.|  
|[Метод ListReservedURLs (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listreservedurls.md)|Выводит список URL-адресов, зарезервированных для всех приложений на сервере отчетов.|  
|[Метод ListSSLCertificateBindings (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificatebindings.md)|Выводит список привязок TLS/SSL-сертификатов, существующих в таблице компонента HTTP.SYS, и привязок, ожидаемых от rsreportserver.config.|  
|[Метод ListSSLCertificates (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-listsslcertificates.md)|Выводит список TLS/SSL-сертификатов, установленных на компьютере.|  
|[ReencryptSecureInformation](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reencryptsecureinformation.md)|Создает новый ключ шифрования и повторно шифрует все защищаемые сведения в базе данных сервера отчетов с этим новым ключом.|  
|[Метод RemoveSSLCertificateBindings (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removesslcertificatebinding.md)|Удаляет привязку к TLS/SSL-сертификату.|  
|[RemoveUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeunattendedexecutionaccount.md)|Удаляет учетную запись автоматического выполнения из конфигурации сервера отчетов.|  
|[Метод RemoveURL (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-removeurl.md)|Удаляет URL-адрес, зарезервированный для сервера отчетов.|  
|[Метод ReserveURL (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-reserveurl.md)|Добавляет резервирование URL-адресов для заданного приложения.|  
|[RestoreEncryptionKey](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-restoreencryptionkey.md)|Повторно применяет заданный ключ шифрования к базе данных сервера отчетов.|  
|[SetDatabaseConnection](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaseconnection.md)|Задает подключение к определенной базе данных сервера отчетов.|  
|[SetDatabaseLogonTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabaselogontimeout.md)|Указывает значение времени ожидания по умолчанию для попыток входа в базу данных сервера отчетов.|  
|[SetDatabaseQueryTimeout](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setdatabasequerytimeout.md)|Указывает значение времени ожидания по умолчанию для подключений к базе данных сервера отчетов.|  
|[SetEmailConfiguration](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setemailconfiguration.md)|Настраивает модуль доставки электронной почты, используемый сервером отчетов для отправки электронной почты.|  
|[SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md)|Задает уровень безопасных соединений для сервера отчетов.|  
|[SetServiceState](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setservicestate.md)|Включает и выключает службу Windows и веб-службу сервера отчетов.|  
|[SetUnattendedExecutionAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setunattendedexecutionaccount.md)|Задает учетную запись, используемую для автоматического выполнения отчетов.|  
|[Метод SetVirtualDirectory (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setvirtualdirectory.md)|Задает виртуальный каталог для приложения.|  
|[SetWindowsServiceIdentity](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setwindowsserviceidentity.md)|Обеспечивает выполнение службы Windows сервера отчетов под именем указанного пользователя Windows и предоставляет этой учетной записи разрешения файловой системы, достаточные для работы сервера отчетов.|  
  
## <a name="see-also"></a>См. также:  
 [Класс MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-class.md)  
  
  
