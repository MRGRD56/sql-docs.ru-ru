---
title: Заметки о выпуске драйверов Майкрософт для PHP
description: На этой странице описано, что было изменено в каждой версии драйверов Microsoft для PHP для SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0df2acfa00507f42d3ee03a0a65ff2ae25aafbd1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837189"
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Заметки о выпуске драйверов Майкрософт для PHP для SQL Server

На этой странице описывается, что было добавлено в каждой версии [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

<!--
Hello, We are standardizing the format of content inside our Release Notes (or What's New) articles.
Instead of bullets (or paragraphs), we have shifted to the 2-column format you see for H2 **What's New in Version 5.6**.
It is not necessary to reformat all the older H2 sections in this Release Notes file, but.....

Going forward, please be sure to use the 2-column format.

Also, all Release Notes .md file names now must begin with 'release-notes-*.md'.  And no filler words.
The 5.6 edition of this file is being renamed.....
FROM:  'release-notes-for-the-php-sql-driver.md'
TO  :  'release-notes-php-sql-driver.md'

For any questions, ask GeneMi or CraigG.
Thanks a lot.  2019-03-28  (DevO= 1467988)
-->

## <a name="59"></a>5.9

![скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=?????)
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.9.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.9.0<sup>1</sup>
- Выпущено: 29 января 2021 г.

### <a name="whats-new-in-59"></a>Новые возможности версии 5.9

| Изменения | Сведения |
| :------- | :------ |
| Добавлена поддержка PHP 8.0. | &nbsp; |
| Прекращена поддержка PHP 7.2. | &nbsp; |
| Добавлена поддержка драйвера Microsoft ODBC Driver 17.7 на всех платформах. | &nbsp; |
| Добавлена поддержка macOS Big Sur, Ubuntu 20.04, Ubuntu 20.10 и Alpine 3.12. | Для некоторых из этих ОС требуется драйвер ODBC Driver 17.5 или более поздней версии. |
| Прекращена поддержка macOS High Sierra, Debian 8 и Ubuntu 19.10. | &nbsp; |
| Поддержка языкового стандарта GB18030. | &nbsp; |
| Функция PDO errorinfo усовершенствована и теперь включает дополнительные сообщения ODBC при их наличии. | &nbsp; |
| Поддержка классификации данных со сведениями о ранге. | Требуются SQL Server 2019 и драйвер ODBC 17.4.2 или более поздней версии. |
| Добавлена поддержка проверки подлинности субъекта-службы Azure Active Directory. | Требуется драйвер ODBC 17.7 или более поздней версии. |
| Улучшена производительность при обработке десятичных чисел во входных или выходных данных. Исключены лишние преобразования числовых значений. | &nbsp; |
| Повышена производительность при получении чисел из буферов клиентов. | &nbsp; |
| Время ожидания запроса задается без использования LOCK TIMEOUT, что позволяет избежать лишнего обращения к серверу. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> В этом выпуске требуется драйвер ODBC Driver 17.4.2 или более поздней версии. В противном случае появится предупреждение о невозможности задать атрибут. При использовании более старой версии драйвера ODBC это предупреждение можно отключить. При использовании SQLSRV обратитесь к статье [Практическое руководство. Настройка обработки ошибок и предупреждений с помощью драйвера SQLSRV](./how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver.md). При использовании PDO_SQLSRV предупреждения по умолчанию скрываются, но могут записываться в журнал. Дополнительные сведения см. в статье [Ведение журнала](./logging-activity.md).

## <a name="581"></a>5.8.1

Этот выпуск применим только к Linux и macOS.

[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.1)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.8.1
- Выпущено: 15 апреля 2020 г.

### <a name="whats-new-in-581"></a>Новые возможности версии 5.8.1

| Изменения | Сведения |
| :------- | :------ |
| Исправление ошибок | Исправлены проблемы настройки языкового стандарта по умолчанию в Alpine Linux. |
| Исправление ошибок | Удалена ненужная структура данных для поддержки функции курсоров на стороне клиента в Alpine Linux. |
| Исправление ошибок | Устранены проблемы с ведением журнала, если оба драйвера включены в Alpine Linux. |
| &nbsp; | &nbsp; |

## <a name="58"></a>5.8

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120362)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.8.0
- Выпущено: 31 января 2020 г.

### <a name="whats-new-in-58"></a>Новые возможности в версии 5.8

| Изменения | Сведения |
| :------- | :------ |
| Добавлена поддержка PHP 7.4. | &nbsp; |
| Прекращена поддержка PHP 7.1. | &nbsp; |
| Добавлена поддержка драйвера Microsoft ODBC Driver 17.5 на всех платформах. | &nbsp; |
| Добавлена поддержка Debian 10 и Red Hat 8. | Для обоих требуется драйвер ODBC 17.4 или более поздней версии. |
| Добавлена поддержка macOS Catalina, Alpine Linux 3.11<sup>1</sup> и Ubuntu 19.10. | Для всех систем требуется драйвер ODBC 17.5 или более поздней версии. |
| Прекращена поддержка SQL Server 2008 R2, macOS Sierra, Ubuntu 18.10 и Ubuntu 19.04. | &nbsp; |
| Поддержка параметра языка при подключении к SQL Server. | &nbsp; |
| Поддержка типов расширенных строк PHP, представленных в PHP 7.2. | &nbsp; |
| Поддержка получения метаданных чувствительности классификации данных. | Требуются SQL Server 2019 и драйвер ODBC 17.4.2 или более поздней версии. |
| Поддержка функции Always Encrypted с безопасными анклавами. | Требуется драйвер ODBC 17.4 или более поздней версии. |
| Поддержка настраиваемых параметров языковых стандартов в Linux и macOS. |
| Повышение производительности за счет кэширования метаданных при выборке и пропуска избыточных вызовов. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Поддержка Alpine Linux является экспериментальной для версии 5.8.

## <a name="previous-releases"></a>Предыдущие выпуски

## <a name="561"></a>5.6.1

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120446)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.1)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.6.1
- Выпущено: 19 марта 2019 г.

### <a name="whats-new-in-561"></a>Новые возможности в версии 5.6.1

| Изменения | Сведения |
| :------- | :------ |
| Исправление ошибок | Исправлены предположения, связанные с вычислением метаданных полей или столбцов, которые могли привести к завершению работы приложения. |
| Исправление ошибок | Изменен файл конфигурации sqlsrv, который можно скомпилировать независимо от pdo_sqlsrv. |
| Исправление ошибок | PDOStatement::getColumnMeta() теперь возвращает значение false, если что-то пойдет не так. |
| &nbsp; | &nbsp; |

## <a name="56"></a>5.6

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120450)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.6.0
- Выпущено: 21 февраля 2019 г.

### <a name="whats-new-in-56"></a>Новые возможности в версии 5.6

| Изменения | Сведения |
| :------- | :------ |
| Поддержка PHP 7.3. | &nbsp; |
| Прекращена поддержка PHP 7.0. | &nbsp; |
| Поддержка драйвера Microsoft ODBC Driver 17.3 на всех платформах. | &nbsp; |
| Поддержка macOS Mojave. | Требуется драйвер ODBC 17.3 или более поздней версии. |
| Поддержка Ubuntu 18.10 и SUSE Linux 15. | Для обоих требуется драйвер ODBC 17.3 или более поздней версии. |
| Прекращена поддержка Linux Ubuntu 17.10 и macOS El Capitan. | &nbsp; |
| Поддержка маркера доступа AAD. | В Linux и macOS требуются драйвер ODBC 17.2+ и unixODBC 2.3.6+. |
| Поддержка проверки подлинности в AAD с помощью управляемого удостоверения для ресурсов Azure. | Требуется драйвер ODBC 17.3+. |
| Новые функции выборки | &bull; &nbsp; Добавлен новый флаг PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE для pdo_sqlsrv, позволяющий возвращать элементы даты и времени в качестве объектов.<br/><br/>&bull; &nbsp; Добавлен параметр ReturnDatesAsStrings на уровень инструкции для sqlsrv.<br/><br/>&bull; &nbsp; Добавлены новые параметры на уровнях подключения и инструкции для обоих драйверов, позволяющие форматировать десятичные значения в результатах выборки. |
| Поддержка статической компиляции драйверов, если пользователь выбирает сборку из источника. | &nbsp; |
| Повышение производительности за счет кэширования метаданных при выборке и повышения скорости преобразования строк Юникода. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="53"></a>5,3

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120447)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.3.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.3.0
- Выпущено: 20 июля 2018 г.

### <a name="whats-new-in-53"></a>Новые возможности в:версии 5.3

- Поддержка драйвера Microsoft ODBC Driver 17.2 на всех платформах.
- Поддержка macOS High Sierra (требуется драйвер ODBC 17 и более поздние версии).
- Поддержка Azure Key Vault для Always Encrypted для базовых функций CRUD, чтобы функция Always Encrypted была доступна для всех поддерживаемых платформ Windows, Linux или macOS. Дополнительные сведения см. в статье [Using Always Encrypted with the PHP Drivers for SQL Server](using-always-encrypted-php-drivers.md) (Использование функции Always Encrypted с драйверами PHP для SQL Server).
- Поддержка Ubuntu 18.04 LTS (требуется драйвер ODBC 17.2).
- Поддержка устойчивости подключений в Linux или macOS (требуется драйвер ODBC 17.2).

## <a name="52"></a>5,2

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120451)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v5.2.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 5.2.0
- Выпущено: 23 марта 2018 г.

### <a name="whats-new-in-52"></a>Новые возможности в версии 5.2

- Поддержка PHP 7.2.1 и более поздних версий в Windows, а также 7.2.0 и более поздних версий на других платформах.
- Поддержка драйвера Microsoft ODBC 17.
  - Версия 17 теперь используется по умолчанию на всех платформах.
- Поддержка Ubuntu 17.10, Debian 9 и SUSE Enterprise Linux 12.
- Прекращена поддержка Ubuntu 15.10.
- Поддержка Always Encrypted с функциями CRUD в Windows. См. подробнее об [использовании функции Always Encrypted с драйвером PHP для SQL Server](using-always-encrypted-php-drivers.md).
  - Поддержка хранилища сертификатов Windows.
  - Always Encrypted поддерживается только с драйвером Microsoft ODBC Driver 17 и более поздних версий.
- Поддержка языковых стандартов, отличных от UTF8, в Linux и macOS.
  - Языковые стандарты, отличные от UTF8, в Linux и macOS поддерживаются только с драйвером Microsoft ODBC Driver 17 и более поздней версии.
- Поддержка Azure Synapse Analytics
- Поддержка Управляемого экземпляра SQL Azure.

## <a name="43"></a>4.3

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120616)  
[Тег выпуска GitHub (пакеты Linux и macOS доступны здесь)](https://github.com/Microsoft/msphpsql/releases/tag/v4.3.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 4.3.0
- Выпущено: 6 июля 2017 г.

### <a name="whats-new-in-43"></a>Новые возможности в версии 4.3

- Поддержка PHP 7.1
- Поддержка macOS Sierra и macOS El Capitan.
- Поддержка Ubuntu 15.10 и Debian 8.
- Прекращена поддержка Ubuntu 15.04.
- Поддержка групп доступности Always On с помощью разрешения IP-адресов прозрачной сети. Дополнительные сведения см. в статье [Connection Options](connection-options.md).
- Добавлена поддержка типа данных sql_variant с ограничением.
- Поддержка устойчивости подключения в режиме ожидания в Windows. Дополнительные сведения см. в статье [Connection Options](connection-options.md).
- Поддержка организации пулов подключений для Linux и macOS. Дополнительные сведения см. в статье [Организация пулов соединений](connection-pooling-microsoft-drivers-for-php-for-sql-server.md).
- Поддержка проверки подлинности Azure Active Directory с помощью ActiveDirectoryPassword и SqlPassword. Дополнительные сведения см. в статье [Connection Options](connection-options.md).

## <a name="40"></a>4,0

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120448)  
[Тег выпуска GitHub](https://github.com/microsoft/msphpsql/releases/tag/v4.0-RTW)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 4,0
- Выпущено: 1 июля 2016 г.

### <a name="whats-new-in-40"></a>Новые возможности в версии 4.0

- Поддержка PHP 7.0  
- Полная поддержка 64-разрядных версий.
- Поддержка Ubuntu 15.04, Ubuntu 16.04 и RedHat 7.

## <a name="32"></a>3.2

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2120449)  
[Тег выпуска GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.2.0.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 3.2
- Выпущено: 9 марта 2015 г.

### <a name="whats-new-in-32"></a>Новые возможности в версии 3.2

- Поддержка PHP 5.6.  
- Содержит последние обновления для предыдущих версий PHP 5.5 и 5.4.  
- Требуется драйвер Microsoft ODBC Driver 11 для SQL Server  

## <a name="31"></a>3.1

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2143027)  
[Тег выпуска GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.1.0.0)

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 3.1
- Выпущено: 12 декабря 2014 г.

### <a name="whats-new-in-31"></a>Новые возможности в версии 3.1

- Поддержка PHP 5.5.  
- Требуется драйвер Microsoft ODBC Driver 11 для SQL Server. Предыдущие версии требуют наличия SQL Native Client.  

## <a name="30"></a>3.0

![Скачать](../../ssms/media/download-icon.png) [Скачать пакет Windows](https://go.microsoft.com/fwlink/?linkid=2143026)  

### <a name="whats-new-in-30"></a>Новые возможности в версии 3.0  

- Поддержка PHP 5.4.  PHP 5.2 не поддерживается в версии 3 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
- Добавлен параметр подключения AttachDBFileName. Дополнительные сведения см. в статье [Connection Options](connection-options.md).  
- Поддержка LocalDB, которая была добавлена в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Дополнительные сведения о поддержке LocalDB см. в [этой статье](php-driver-for-sql-server-support-for-localdb.md).
- Добавлен параметр подключения AttachDBFileName. Дополнительные сведения см. в статье [Connection Options](connection-options.md).  
- Поддержка функций высокой доступности и аварийного восстановления. См.подробнее о [поддержке высокой доступности и аварийного восстановления](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Поддержка клиентских курсоров (кэширования результирующего набора в памяти). Дополнительные сведения см. в статьях [Типы курсоров &#40;драйвер SQLSRV&#41;](cursor-types-sqlsrv-driver.md) и [Типы курсоров &#40;драйвер PDO_SQLSRV&#41;](cursor-types-pdo-sqlsrv-driver.md).
- Был добавлен атрибут PDO::ATTR_EMULATE_PREPARE. Подробнее см. в разделе [PDO::prepare](pdo-prepare.md).  

## <a name="20"></a>2.0

### <a name="whats-new-in-20"></a>Новые возможности версии 2.0

В версии 2.0 была добавлена поддержка драйвера PDO_SQLSRV. Дополнительные сведения см. в статье [Справочник по драйверу PDO_SQLSRV](pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>См. также:

[Обзор драйверов Майкрософт для PHP для SQL Server](overview-of-the-php-sql-driver.md)