---
description: Вопросы безопасности по драйверам Майкрософт для PHP для SQL Server
title: Вопросы безопасности по драйверам Майкрософт для PHP для SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- security considerations
ms.assetid: a8c1a570-9204-454f-b94c-ba34f54d487c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b079801b30b21f16876447218847b5e611e16650
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726731"
---
# <a name="security-considerations-for-the-microsoft-drivers-for-php-for-sql-server"></a>Вопросы безопасности по драйверам Майкрософт для PHP для SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

В этом разделе рассматриваются вопросы безопасности, связанные с разработкой, развертыванием и выполнением приложений, использующих [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Более подробные сведения о безопасности SQL Server см. в статье [Общие сведения о безопасности SQL Server](/dotnet/framework/data/adonet/sql/overview-of-sql-server-security).  
  
## <a name="connect-using-windows-authentication"></a>Подключение с помощью проверки подлинности Windows  
Для подключения к SQL Server следует по возможности использовать проверку подлинности Windows по следующим причинам.  
  
-   **Во время проверки подлинности по сети не передаются никакие учетные данные.** Имена пользователей и пароли не внедряются в строку подключения базы данных. Поэтому злоумышленники не смогут получить учетные данные путем мониторинга сети или просмотра строк подключения в файлах конфигурации.  
  
-   **На пользователей распространяется централизованное управление учетными записями.** Применяются политики безопасности, касающиеся, например, сроков действия паролей, минимальной длины паролей и блокировки учетных записей после нескольких неудачных запросов на вход.  
  
Сведения о том, как подключиться к серверу с использованием проверки подлинности Windows, см. в статье [Практическое руководство. Подключение с использованием проверки подлинности Windows](../../connect/php/how-to-connect-using-windows-authentication.md).  
  
При подключении с использованием проверки подлинности Windows рекомендуется настроить среду таким образом, чтобы SQL Server мог использовать протокол проверки подлинности Kerberos. Дополнительные сведения см. в статье [Подтверждение использования проверки подлинности Kerberos при создании удаленного подключения к экземпляру SQL Server 2005](https://support.microsoft.com/en-ca/help/909801/how-to-make-sure-that-you-are-using-kerberos-authentication-when-you-c) или [SQL Server и проверка подлинности по протоколу Kerberos](/previous-versions/sql/sql-server-2008-r2/cc280744(v=sql.105)).  
  
## <a name="use-encrypted-connections-when-transferring-sensitive-data"></a>Использование зашифрованных соединений при передаче конфиденциальных данных  
Зашифрованные соединения следует использовать всякий раз, когда конфиденциальные данные отправляются в SQL Server или извлекаются оттуда. Дополнительные сведения о том, как включить зашифрованные соединения, см. в статье [Практическое руководство. Включение шифрования соединений в ядре СУБД (диспетчер конфигурации SQL Server)](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md). Чтобы установить безопасное соединение с [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], используйте атрибут соединения Encrypt при подключении к серверу. Дополнительные сведения об атрибутах соединения см. в статье [Connection Options](../../connect/php/connection-options.md).  
  
## <a name="use-parameterized-queries"></a>Использование параметризованных запросов  
Используйте параметризованные запросы для снижения риска атак путем внедрения кода SQL. Примеры выполнения параметризованных запросов см. в статье [How to: Perform Parameterized Queries](../../connect/php/how-to-perform-parameterized-queries.md).  
  
Дополнительные сведения об атаках путем внедрения кода SQL и соответствующих аспектах безопасности см. в статье [Атака путем внедрения кода SQL](/previous-versions/sql/sql-server-2008-r2/ms161953(v=sql.105)).  
  
## <a name="do-not-accept-server-or-connection-string-information-from-end-users"></a>Запрет на получения сведений о сервере или строке подключения от конечных пользователей  
Проектируйте приложения таким образом, чтобы конечные пользователи не могли отправить в приложение сведения о сервере или строке подключения. Обеспечение строгого контроля над сведениями о сервере и строках подключения сокращает контактную зону для вредоносных действий.  
  
## <a name="turn-warningsaserrors-on-during-application-development"></a>Включение WarningsAsErrors во время разработки приложений  
Разрабатывайте приложения, задав для параметра **WarningsAsErrors** значение **true** , чтобы выдаваемые драйвером предупреждения обрабатывались как ошибки. Это позволит изучить предупреждения перед развертыванием приложения. Дополнительные сведения см. в статье [Handling Errors and Warnings](../../connect/php/handling-errors-and-warnings.md).  
  
## <a name="secure-logs-for-deployed-application"></a>Защита журналов для развернутого приложения  
Для развернутых приложений убедитесь, что журналы записываются в безопасное расположение либо что ведение журналов отключено. Это помогает предотвратить доступ конечных пользователей к данным, записанным в файл журнала. Дополнительные сведения см. в статье [Logging Activity](../../connect/php/logging-activity.md).  
  
## <a name="see-also"></a>См. также:  
[Руководство по программированию драйверов Microsoft для PHP для SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
