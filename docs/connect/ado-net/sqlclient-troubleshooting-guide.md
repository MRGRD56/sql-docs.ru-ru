---
title: Руководство по устранению неполадок с SqlClient
description: Страница с описанием способов решения распространенных проблем.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 725d1522f98e721f581a7d4b7614181fd2632962
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464807"
---
# <a name="sqlclient-troubleshooting-guide"></a>Руководство по устранению неполадок с SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>Исключения при подключении к SQL Server

Есть несколько причин, по которым не удается установить подключение. Ниже приведено несколько советов по устранению неполадок. Эти советы можно использовать для анализа и решения многих проблем.

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>Не удалось загрузить нативную библиотеку SNI (указание имени сервера)

#### <a name="issues-in-net-framework-applications"></a>Проблемы в приложениях .NET Framework

Отслеживаемая трассировка стека:

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI — это нативная библиотека C++, на основе которой в SqlClient выполняются различные сетевые операции при работе в Windows. В приложениях .NET Framework, созданных с помощью пакета SDK для проекта MSBuild, управление нативными библиотеками DLL не осуществляется с помощью команд восстановления. Таким образом, файл .targets включается в пакет NuGet Microsoft.Data.SqlClient.SNI, который определяет необходимые операции копирования.

Для включаемого файла .targets при установке прямой зависимости от библиотеки Microsoft.Data.SqlClient создается автоматическая ссылка. В сценариях, где создается транзитная (косвенная) ссылка, необходимо вручную ссылаться на этот файл .targets, чтобы при необходимости могли выполняться операции копирования.

**Рекомендуемое решение.** Убедитесь, что в файле .csproj приложения есть ссылка на файл .targets, чтобы обеспечить выполнение операций копирования.

Среди этих целевых объектов только известные и часто используемые целевые объекты Майкрософт. Если внешний инструмент или приложение определяет пользовательские целевые объекты для копирования двоичных файлов, то средства обслуживания инструмента должны определить новые целевые объекты, чтобы обеспечить копирование нативных DLL-библиотек SNI вместе с двоичными файлами Microsoft.Data.SqlClient.dll и доступность таких библиотек при выполнении клиентских приложений.

#### <a name="issues-in-net-core-applications"></a>Проблемы в приложениях .NET Core

Отслеживаемая трассировка стека:

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> Эта ошибка может возникать только в Windows-приложениях. Если это происходит в среде Unix, убедитесь, что приложение создано для среды выполнения UNIX, а не для Windows.

SNI — это нативная библиотека C++, на основе которой в SqlClient выполняются различные сетевые операции при работе в Windows. Microsoft.Data.SqlClient не управляет загрузкой этой библиотеки в ПО .NET Core и ее выгрузкой из него.

**Рекомендуемое решение.** Обеспечьте для файловой системы, где нативные библиотеки среды выполнения загружаются в процесс .NET Core, разрешения на выполнение. Если это не помогло решить проблему, можно зарегистрировать ее в репозитории [dotnet/runtime](https://github.com/dotnet/runtime), чтобы получить дальнейшую поддержку.

#### <a name="native-sni-pdb-not-found-errors"></a>Ошибки нативной функции SNI (не удалось найти PDB)

Отслеживаемая трассировка стека:

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

**Рекомендуемое решение.** Убедитесь, что клиентское приложение ссылается на минимальную версию [2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0) пакета Microsoft.Data.SqlClient. При использовании EF Core добавьте ссылку на эту версию пакета Microsoft.Data.SqlClient напрямую, чтобы переопределить зависимость.

### <a name="hostname-resolution-errors"></a>Ошибки разрешения имени узла

Отслеживаемая трассировка стека:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>Возможные причины

- В SQL Server не включен протокол TCP или протокол именованных каналов

  **Рекомендуемое решение.** Включите протокол TCP или протокол именованных каналов в экземпляре SQL Server из консоли диспетчера конфигурации SQL Server.

- Имя узла неизвестно

  **Рекомендуемое решение.** Убедитесь, что имя узла разрешается в IP-адрес сервера от клиента, в котором инициируется подключение.


### <a name="login-phase-errors"></a>Ошибки на этапе входа

Отслеживаемые трассировки стека:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>Возможные причины и решения

- SQL Server не поддерживает TLS 1.2

  Эта ошибка обычно происходит в клиентских средах, таких как контейнеры образов Docker, клиенты Unix или клиенты Windows, где TLS 1.2 является минимальной поддерживаемой версией протокола TLS.

  **Рекомендуемое решение.** Установите последние обновления для поддерживаемых версий SQL Server <sup>1</sup> и убедитесь, что на сервере включен протокол TLS 1.2.

  _<sup>1</sup> Список поддерживаемых версий SQL Server с разными версиями Microsoft.Data.SqlClient см. в статье [Жизненный цикл поддержки драйвера SqlClient](sqlclient-driver-support-lifecycle.md)._

  **Небезопасное решение.** Настройте параметры TLS/SSL в среде образа или клиента Docker для подключения к TLS 1.0.

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > При подключении к Microsoft.Data.SqlClient версии 2.0 и выше из среды Windows или Linux с использованием TLS 1.0 или TLS 1.1 будет активировано предупреждение системы безопасности, если целевому объекту SQL Server и клиенту не удается согласовать минимальную версию TLS 1.2 при установке соединения: `Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- Принудительное шифрование SQL Server

  Если целевой сервер является экземпляром SQL Azure или локальным экземпляром SQL Server с включенным свойством принудительного шифрования, будет создано зашифрованное подключение, для которого клиент должен установить отношения доверия с сервером.

  **Рекомендуемое решение.** Есть два варианта устранения этой проблемы:

    1. Установите сертификат TLS/SSL для целевого экземпляра SQL Server в клиентской среде. Будет выполнена проверка на предмет того, нужно ли шифрование.
    2. Задайте свойство Trust Server Certificate = true в строке подключения.

  **Небезопасное решение.** Отключите параметр принудительного шифрования в SQL Server.

- Сертификаты TLS/SSL не подписаны с помощью SHA-256 или более поздней версии.

  **Рекомендуемое решение.** Создайте новый сертификат TLS/SSL для сервера, хэш которого подписывается по меньшей мере с помощью алгоритма хэширования SHA-256.

- Строго ограниченный список комплектов шифров TLS для .NET 5 и более поздних версий в Linux

  В .NET 5 представлено критическое изменение для клиентов Linux, которое заключается в использовании строго ограниченного списка разрешенных комплектов шифров по умолчанию. Если необходимо расширить список комплектов шифров по умолчанию для работы с устаревшими клиентами (или для взаимодействия с устаревшими серверами), укажите значение `CipherSuitePolicy` или измените файл конфигурации _OpenSSL_.
  
  Дополнительные сведения о рекомендуемых действиях см. в статье [Комплекты шифров TLS по умолчанию для .NET в Linux](/dotnet/core/compatibility/cryptography/5.0/default-cipher-suites-for-tls-on-linux).

### <a name="connection-pool-exhaustion-errors"></a>Проблема, вызванная тем, что пул подключений исчерпан

Отслеживаемая трассировка стека:

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>Возможные причины и решения

Клиентское приложение открывает больше подключений, чем пул соединений может сохранить в текущий момент.

**Рекомендуемое решение.** Задайте для свойства соединения Max Pool Size большее значение и своевременно закрывайте неиспользуемые подключения.

## <a name="contact-support"></a>Обращение в службу поддержки

Если это не помогло устранить проблемы с подключением, вы можете просмотреть сведения о существующих проблемах в репозитории [dotnet/sqlclient](https://github.com/dotnet/SqlClient) и при необходимости открыть новую проблему.
