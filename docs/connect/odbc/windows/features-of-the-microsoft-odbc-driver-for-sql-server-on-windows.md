---
title: Возможности Microsoft ODBC Driver
description: Узнайте о различных возможностях, поддерживаемых драйвером Microsoft ODBC Driver for SQL Server в Windows.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 5fc07a171e42338ca76d51d66c04af187cb6beda
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "92005909"
---
# <a name="features-of-the-microsoft-odbc-driver-for-sql-server-on-windows"></a>Функции Microsoft ODBC Driver for SQL Server в Windows
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

    
## <a name="microsoft-odbc-driver-174-for-sql-server-on-windows"></a>Драйвер Microsoft ODBC Driver 17.4 for SQL Server в Windows

Драйвер ODBC 17.4 включает возможность настройки параметров поддержания активности TCP. Их можно изменить, добавив значения в разделы реестра драйвера или DSN. Ключи находятся в `HKEY_LOCAL_MACHINE\Software\ODBC\` для системных источников данных и в `HKEY_CURRENT_USER\Software\ODBC\` для источников данных пользователя. Для DSN значения следует добавить в `...\Software\ODBC\ODBC.INI\<DSN Name>`, а для драйвера — в `...\Software\ODBC\ODBCINST.INI\ODBC Driver 17 for SQL Server`.

Дополнительные сведения см. в статье [Registry Entries for ODBC Components](../../../odbc/reference/install/registry-entries-for-odbc-components.md) (Записи реестра для компонентов ODBC).

Возможны следующие значения в формате `REG_SZ`:

- `KeepAlive` управляет частотой попыток протокола TCP проверить работоспособность неактивного подключения путем отправки пакета keep-alive. По умолчанию это 30 секунд.

- `KeepAliveInterval` определяет интервал, разделяющий повторные передачи пакета keep-alive, до получения ответа. Значение по умолчанию — 1 секунда.



## <a name="microsoft-odbc-driver-131-for-sql-server-on-windows"></a>Драйвер Microsoft ODBC Driver 13.1 for SQL Server в Windows

Драйвер ODBC 13.1 для SQL Server содержит все функции предыдущей версии (11) и добавляет поддержку проверки подлинности Always Encrypted и Azure Active Directory при использовании в сочетании с Microsoft SQL Server 2016.  
  
Функция Always Encrypted позволяет клиентам шифровать конфиденциальные данные в клиентских приложениях, не раскрывая ключи шифрования для SQL Server. Драйвер с поддержкой Always Encrypted, установленный на клиентском компьютере, реализует это за счет автоматического шифрования и расшифровки конфиденциальных данных в клиентском приложении SQL Server. Драйвер шифрует данные из конфиденциальных столбцов перед их передачей в SQL Server и автоматически переписывает запросы, чтобы сохранить семантику приложения. Аналогичным образом драйвер прозрачно расшифровывает данные, хранящиеся в столбцах зашифрованной базы данных, которые содержатся в результатах запроса. Дополнительные сведения см. в статье [Использование функции Always Encrypted с драйвером ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md).
 
Azure Active Directory позволяет пользователям, администраторам баз данных и приложениям использовать проверку подлинности Azure Active Directory в качестве механизма подключения к Базе данных SQL Microsoft Azure и Microsoft SQL Server 2016 с помощью удостоверений в Azure Active Directory (Azure AD). Дополнительные сведения см. в статьях [Использование Azure Active Directory с драйвером ODBC](../using-azure-active-directory.md) и [Подключение к Базе данных SQL или Azure Synapse Analytics с помощью проверки подлинности Azure Active Directory](/azure/sql-database/sql-database-aad-authentication).   
  
## <a name="microsoft-odbc-driver-11-for-sql-server-on-windows"></a>Драйвер Microsoft ODBC 11 для SQL Server в Windows  

Драйвер ODBC для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] содержит все функциональные возможности драйвера ODBC Native Client [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], который входит в состав [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Дополнительные сведения см. в статье [Программирование SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md). Драйвер ODBC Native Client [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] основан на драйвере ODBC, который входит в состав операционной системы Windows. Дополнительные сведения см. в статье [Пакет SDK компонентов доступа к данным Windows DAC](/previous-versions/windows/desktop/legacy/aa968814(v=vs.85)).  
  
Этот выпуск драйвера ODBC для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] содержит следующие новые функции:  
  
### <a name="bcpexe--l-option-for-specifying-a-login-timeout"></a>параметр bcp.exe -l для указания времени ожидания входа
 
Параметр -l задает время ожидания (в секундах) для входа `bcp.exe` в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] при попытке соединения с сервером. Значение времени ожидания по умолчанию — 15 секунд. Время ожидания входа должно быть числом в диапазоне от 0 до 65 534. Если указанное значение не является числом или выходит за пределы указанного диапазона, программа `bcp.exe` выдает сообщение об ошибке. Значение 0 указывает на бесконечное время ожидания. Время ожидания входа, которое меньше 10 секунд (приблизительно), не является надежным.  
  
### <a name="driver-aware-connection-pooling"></a>Организация пулов соединений с учетом драйвера  
Драйвер ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает [организацию пулов соединений с учетом драйвера](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md). Дополнительные сведения см. в статье [Организация пулов соединений с учетом драйвера в ODBC Driver for SQL Server](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md).  
  
### <a name="asynchronous-execution-notification-method"></a>Асинхронное выполнение (метод уведомления)  
Драйвер ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает [асинхронное выполнение (метод уведомления)](../../../odbc/reference/develop-app/asynchronous-execution-notification-method.md). Пример использования см. в [примере асинхронного выполнения &#40;метод уведомления&#41;](asynchronous-execution-notification-method-sample.md).  
  
### <a name="connection-resiliency"></a>Устойчивость подключений
Чтобы обеспечить сохранение подключения приложений к Базе данных SQL Microsoft Azure, драйвер ODBC в Windows может восстанавливать неактивные соединения. Дополнительные сведения см. в статье [Устойчивость подключения в драйвере ODBC в Windows](connection-resiliency-in-the-windows-odbc-driver.md).  
  
## <a name="behavior-changes"></a>Изменения в работе

В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client параметр `-y0` для `sqlcmd.exe` привел к усечению выходных данных на 1 МБ при значении ширины экрана "0".
  
Начиная с ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ограничение на объем данных, извлекаемых в одном столбце, при указанном `-y0` отсутствует. Теперь `sqlcmd.exe` осуществляет потоковую передачу столбцов размером до 2 ГБ (максимальное значение для типа данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]).  
  
Другое отличие заключается в том, что указание как `-h`, так и `-y0` теперь приводит к сообщению об ошибке, связанной с несовместимостью опций. Параметр `-h`, который указывает количество выводимых строк между заголовками столбцов, никогда не был совместим с `-y0` и игнорировался, хотя заголовки не выводились.
  
Обратите внимание на то, что `-y0` может значительно снизить производительность сервера и сети в зависимости от объема возвращаемых данных.

## <a name="see-also"></a>См. также:  
[Драйвер Microsoft ODBC Driver for SQL Server в Windows](microsoft-odbc-driver-for-sql-server-on-windows.md)  
