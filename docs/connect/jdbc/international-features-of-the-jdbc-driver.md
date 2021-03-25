---
description: Сведения о функциях интернационализации драйвера JDBC, а также о том, как создать среду локализованного приложения.
title: Функции поддержки разных языков JDBC Driver
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: bbb74a1d-9278-401f-9530-7b5f45aa79de
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 159c1e35761876d786ad0a36c78eaa8ce8f3e847
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673428"
---
# <a name="international-features-of-the-jdbc-driver"></a>Функции поддержки разных языков JDBC Driver

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Функции [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] включают в себя следующее:
  
- Поддержка полной локализации на тех же языках, что и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- Поддержка преобразований языка Java для данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], зависящих от языкового стандарта.  
- Поддержка международных языков независимо от операционной системы.  
- Поддержка международных доменных имен (начиная с Microsoft JDBC Driver 6.0 для SQL Server).  
  
## <a name="handling-of-character-data"></a>Обработка символьных данных

Символьные данные на Java по умолчанию рассматриваются как данные в формате Юникода. Объект Java **String** представляет символьные данные в формате Юникода. Единственное исключение из этого правила в драйвере JDBC — методы считывания и задания ASCII-потока, но это особый случай, поскольку в этих методах при обработке байтовых потоков неявно предполагается использование конкретной известной кодовой страницы (ASCII).  
  
Кроме того, драйвер JDBC поддерживает свойство **sendStringParametersAsUnicode** для строки подключения. С помощью этого свойства можно указывать, что подготовленные параметры для символьных данных были отправлены в формате ASCII или MBCS, а не в Юникоде. Дополнительные сведения о свойстве подключения **sendStringParametersAsUnicode** см. в статье [Настройка свойств подключения](setting-the-connection-properties.md).  
  
### <a name="driver-incoming-conversions"></a>Входящие преобразования драйвера

Текстовые данные в Юникоде, приходящие с сервера, преобразовывать не нужно. Они передаются непосредственно в Юникоде. Данные в других форматах, приходящие с сервера, преобразовываются в Юникод из кодировки для данных на уровне базы данных или столбца. Драйвер JDBC использует для выполнения этих преобразований подпрограммы Java Virtual Machine (JVM). Этим преобразованиям подвергаются все методы получения потоков строчных и символьных данных.  
  
Если база данных не поддерживает нужные кодировки для JVM, то драйвер JDBC вызовет исключение "Кодовая страница ХХХ не поддерживается средой Java". Чтобы решить эту проблему, следует установить полную поддержку международных символов, необходимую для данной виртуальной машины JVM.
  
### <a name="driver-outgoing-conversions"></a>Исходящие преобразования драйвера

Символьные данные, исходящие из драйвера на сервер, могут иметь форматы ASCII или Юникод. Например, новые методы национальных символов в JDBC 4.0, такие как методы setNString, setNCharacterStream и setNClob классов [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) и [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md), всегда выполняют отправку на сервер своих значений параметров в формате Юникода.  
  
C другой стороны, методы для API ненациональных символов, такие как setString, setCharacterStream и setClob классов [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md) и [SQLServerCallableStatement](reference/sqlservercallablestatement-class.md), отправляют на сервер значения своих параметров в формате Юникода, только если свойство **sendStringParametersAsUnicode** имеет значение True (это значение по умолчанию).  
  
## <a name="non-unicode-parameters"></a>Параметры для кодировок, отличных от Юникода

Для оптимальной работы типов **CHAR**, **VARCHAR** и **LONGVARCHAR** (в параметрах, отличных от Юникода), задайте для свойства строки подключения **sendStringParametersAsUnicode** значение false и воспользуйтесь методами ненациональных символов.  
  
## <a name="formatting-issues"></a>Проблемы с форматированием

Все форматирование локализованных данных для дат, времени и валют выполняется на уровне языка Java с помощью объекта Locale и различных методов форматирования для типов данных **Date**, **Calendar** и **Number**. В редких случаях, когда драйвер JDBC должен передать зависящие от локали конфиденциальные данные в локализованном формате, нужный инструмент форматирования используется с локалем JVM, заданным по умолчанию.  
  
## <a name="collation-support"></a>Поддержка параметров сортировки

В версии JDBC Driver 3.0 поддерживаются все параметры сортировки, поддерживаемые [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], а также новые параметры сортировки и новые версии имен параметров сортировки Windows, реализованные в [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)].  
  
Дополнительные сведения о параметрах сортировки см. в статьях [Поддержка параметров сортировки и Юникода](/previous-versions/sql/sql-server-2008-r2/ms143503(v=sql.105)) и [Имя параметров сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).  
  
## <a name="using-international-domain-names-idn"></a>Использование международных доменных имен (IDN)

JDBC Driver 6.0 для SQL Server поддерживает использование международных доменных имен (IDN), которые при необходимости могут преобразовать имя сервера в Юникоде в кодировку, совместимую с ASCII (Punycode), во время подключения.  Если международные доменные имена хранятся в системе доменных имен (DNS) в виде строк ASCII в формате Punycode (как указано в RFC 3490), включите преобразование имени сервера в Юникоде, задав для свойства serverNameAsACE значение true.  В противном случае, если служба DNS настроена на использование символов Юникода, задайте для свойства serverNameAsACE значение false (по умолчанию).  Для более старых версий JDBC Driver можно также преобразовать имя сервера в Punycode с помощью методов [IDN.toASCII Java](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html), прежде чем устанавливать это свойство для подключения.  
  
> [!NOTE]  
> Большинство программ сопоставителя, написанных для платформ, отличных от Windows, основано на интернет-стандартах DSN, поэтому для международных доменных имен чаще всего используется формат Punycode, хотя DNS-сервер под управлением Windows в частной сети можно в индивидуальном порядке настроить на использование символов UTF-8.  Дополнительные сведения см. в статье о [поддержке символов Юникода](/previous-versions/windows/it-pro/windows-server-2003/cc738403(v=ws.10)).  
  
## <a name="see-also"></a>См. также раздел

[Общие сведения о JDBC Driver](overview-of-the-jdbc-driver.md)  
