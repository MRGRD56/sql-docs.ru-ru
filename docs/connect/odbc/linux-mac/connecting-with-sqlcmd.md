---
title: Соединение с помощью sqlcmd
description: Узнайте, как использовать служебную программу sqlcmd с Microsoft ODBC Driver for SQL Server в Linux и macOS.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216f78615ca049d3e97134cb14831d9a5e6afd32
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837366"
---
# <a name="connecting-with-sqlcmd"></a>Соединение с помощью sqlcmd
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Служебная программа [sqlcmd](../../../tools/sqlcmd-utility.md) доступна с [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на Linux и macOS.
  
Следующие команды демонстрируют, как использовать проверку подлинности Windows (Kerberos) и проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] соответственно.
  
```console
sqlcmd -E -Sxxx.xxx.xxx.xxx  
sqlcmd -Sxxx.xxx.xxx.xxx -Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>Доступные параметры

В текущем выпуске доступны следующие параметры:  
  
- -? Отображение использования `sqlcmd`.  
  
- -а — запросить размер пакета.  
  
- -b — завершить пакетное задание в случае ошибки.  
  
- -c *batch_terminator* — указать признак конца пакета.  
  
- -C — доверять сертификату сервера.  

- -d *database_name* — выдает инструкцию `USE` *database_name* при запуске `sqlcmd`.  

- -D — интерпретировать значение, передаваемое параметру `sqlcmd` -S, как имя источника данных (DSN). Дополнительные сведения см. в разделе "Поддержка имени DSN в `sqlcmd` и `bcp`" в конце этой статьи.  
  
- -e — выводить входные скрипты на стандартное устройство вывода (stdout).

- -E — использовать доверительное соединение (встроенную проверку подлинности). Дополнительные сведения о создании доверительных соединений, использующих встроенную проверку подлинности, из клиента Linux или macOS см. в статье [Using Integrated Authentication](using-integrated-authentication.md) (Использование встроенной проверки подлинности).

- -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage] — задает входные и выходные кодовые страницы. Номер кодовой страницы — это числовое значение, которое определяет установленную кодовую страницу Linux.
Доступно с 17.5.1.1.

- -G — клиент использует этот параметр при подключении к Базе данных SQL или Azure Synapse Analytics, чтобы указать, что проверка подлинности пользователя выполняется с помощью Azure Active Directory. Этот параметр задает переменную скрипта SQLCMDUSEAAD = true программы sqlcmd . Для использования параметра -G требуется sqlcmd версии не ниже 17.6. Чтобы определить версию, выполните команду sqlcmd -?.

> [!IMPORTANT]
> Параметр `-G` применяется только для Базы данных SQL Azure и Azure Synapse Analytics.
> Интерактивная проверка подлинности AAD в Linux и macOS в настоящее время не поддерживается. Для встроенной проверки подлинности AAD требуется [драйвер Microsoft ODBC 17 for SQL Server](../download-odbc-driver-for-sql-server.md) версии 17.6.1 или более поздней, а также [правильно настроенная среда Kerberos](using-integrated-authentication.md#configure-kerberos).

- -h *число_строк* — задать число строк, печатаемое между заголовками столбцов.  
  
- -H — указать имя рабочей станции.  
  
- -i *файл_ввода*[,*файл_ввода*[,…]] — указать файл, содержащий пакет инструкций или хранимых процедур SQL.  
  
- -I — установить для параметра подключения `SET QUOTED_IDENTIFIER` на значение "Вкл".  
  
- -k — удалить или заменить символы управления.  
  
- **-K**_намерение\_приложения_  
Объявляет тип рабочей нагрузки приложения при соединении с сервером. Единственным поддерживаемым в данное время значением является **ReadOnly**. Если параметр **-K** не указан, `sqlcmd` не поддерживает подключение ко вторичной реплике в группе доступности AlwaysOn. См. подробнее об [обеспечении высокой доступности и аварийного восстановления с помощью драйвера ODBC для Linux и macOS](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> Параметр **-K** не поддерживается в CTP-версии для SUSE Linux. Однако вы можете указать ключевое слово **ApplicationIntent=ReadOnly** в файле DSN, передаваемом в `sqlcmd`. Дополнительные сведения см. в разделе "Поддержка имени DSN в `sqlcmd` и `bcp`" в конце этой статьи.  
  
- Параметр -l *timeout* задает время ожидания (в секундах) для входа в `sqlcmd` при попытке соединения с сервером.

- -m *уровень_ошибки* — выбрать сообщения об ошибках, отправляемые на stdout.  
  
- **-M** — _несколько подсетей\_отработки отказа_  
Всегда указывайте параметр **-M** при соединении с прослушивателем группы доступности [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] или экземпляром отказоустойчивого кластера [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **-M** позволяет ускорить обнаружение обработок отказа и подключение к активному (в данный момент) серверу. Если параметр **-M** не указан, значит **-M** отключен. См. подробнее об [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)] в руководстве по [обеспечению высокого уровня доступности и аварийного восстановления с помощью драйвера ODBC для Linux и macOS](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> Параметр **-M** не поддерживается в CTP-версии для SUSE Linux. Однако вы можете указать ключевое слово **MultiSubnetFailover=Yes** в файле DSN, передаваемом в `sqlcmd`. Дополнительные сведения см. в разделе "Поддержка имени DSN в `sqlcmd` и `bcp`" в конце этой статьи.  
  
- -N — шифровать соединение.  
  
- -o *файл_вывода* — указать файл, получающий выходные данные от `sqlcmd`.  
  
- -p — напечатать статистику производительности для каждого результирующего набора.  
  
- -P — указать пароль пользователя.  
  
- -q *commandline_query* — выполнение запроса при запуске программы `sqlcmd`, но выход из программы по завершении его выполнения не производится.  

- -Q *commandline_query* — выполнение запроса при запуске `sqlcmd`. `sqlcmd` завершает работу после выполнения запроса.  

- -r — перенаправлять сообщения об ошибках на stderr.

- -R — преобразовывать дату, время и валюту в символьные данные в соответствии с настройками региона клиента. В настоящее время используется только форматирование en_US (Английский (США)).
  
- -s *column_separator_char* — задать символ-разделитель столбцов.  

- -S [*протокол*:] *сервер*[ **,** _порт_]  
Задать экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], к которому требуется подключиться, или имя DSN при использовании с параметром -D. Драйвер ODBC для Linux и macOS требует -S. Обратите внимание, что единственным допустимым протоколом является **tcp**.  
  
- -t *время_ожидания_запроса* — указать время ожидания команды (или инструкции SQL) в секундах.  
  
- -u — указать, что <файл_вывода> хранится в формате Юникода вне зависимости от формата <файла_ввода>.  
  
- -U *login_id* — указать идентификатор входа пользователя.  
  
- -V *уровень_серьезности_ошибки* — задать уровень серьезности, используемый для переменной ERRORLEVEL.  
  
- -w *column_width* — задать ширину экрана для вывода.  
  
- -W — удалить в столбце конечные пробелы.  
  
- -x — отключить подстановку переменных.  
  
- -X — отключить команды, сценарий запуска и переменные сред.  
  
- -y *изменяемая_ширина_отображения* — задать переменную скрипта `sqlcmd` `SQLCMDMAXFIXEDTYPEWIDTH`.
  
- -Y *variable_length_type_display_width* — задать переменную скрипта `sqlcmd` `SQLCMDMAXVARTYPEWIDTH`.

- -z *password* — изменить пароль.  
  
- -Z *password* — изменить пароль и выйти.  


## <a name="available-commands"></a>Доступные команды

В текущем выпуске доступны следующие команды:  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*число*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>Недоступные параметры
В текущем выпуске следующие параметры недоступны:  

- -A — войти в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с выделенным административным соединением. Сведения об использовании выделенного административного соединения см. в разделе [Указания по программированию](programming-guidelines.md).  
  
- -L — вывести список серверных компьютеров, настроенных локально, и имена серверных компьютеров, передающих данные в сеть.  
  
- -v — создать переменную скрипта `sqlcmd`, которую можно использовать в сценарии `sqlcmd`.  
  
Можно использовать следующий альтернативный метод: Помещайте параметры в один файл, который затем можно добавить в другой файл. Это позволит использовать файл параметров для замены значений. Например, создайте файл с именем `a.sql` (файл параметров) со следующим содержимым:
  
```console
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
```
  
Затем создайте файл `b.sql` с параметрами для замены:  
  
```sql
    select $(ColumnName) from $(TableName)  
```

В командной строке совместите `a.sql` и `b.sql` в `c.sql` с помощью следующих команд:  
  
```console
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
```
  
Запустите `sqlcmd` и используйте `c.sql` в качестве входного файла:  
  
```console
    sqlcmd -S<...> -P<..> -U<..> -I c.sql  
```

## <a name="unavailable-commands"></a>Недоступные команды

В текущем выпуске следующие команды недоступны:  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>Поддержка имени DSN в sqlcmd и bcp

Вы можете указать имя источника данных (DSN) вместо имени сервера в **sqlcmd** или параметре **bcp** `-S` (или в команде **sqlcmd** :Connect), если зададите параметр `-D`. `-D` вызывает **sqlcmd** или **bcp** для подключения к серверу, указанному в имени DSN, с помощью параметра `-S`.  
  
Системные имена DSN хранятся в файле `odbc.ini` в каталоге ODBC SysConfigDir (`/etc/odbc.ini` в стандартных установках). Пользовательские имена DSN хранятся в `.odbc.ini` в домашнем каталоге пользователя (`~/.odbc.ini`).

В системах Windows системные и пользовательские имена DSN хранятся в реестре и управляются через файл odbcad32.exe. Файловые DSN не поддерживаются bcp и sqlcmd.

Список записей, поддерживаемых драйвером, см. в разделе [Ключевые слова и атрибуты строки подключения и имени DSN](../dsn-connection-string-attribute.md).

В имени DSN только запись DRIVER является обязательной, но, чтобы подключиться к удаленному серверу, `sqlcmd` или `bcp` требуется значение в элементе SERVER. Если элемент SERVER пуст или отсутствует в DSN, `sqlcmd` и `bcp` попытаются подключиться к экземпляру по умолчанию в локальной системе.

При использовании bcp в системах Windows [!INCLUDE [sssql17-md](../../../includes/sssql17-md.md)] и более ранних версий требуется драйвер SQL Native Client 11 (sqlncli11.dll), а для [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] и более поздних версий требуется драйвер Microsoft ODBC Driver 17 для драйвера SQL Server (msodbcsql17.dll).  

Если указать один и тот же параметр и в имени DSN, и в командной строке `sqlcmd` или `bcp`, значение в командной строке имеет приоритет перед значением DSN. Например, если имя DSN содержит запись DATABASE, а командная строка `sqlcmd` включает параметр **-d**, используется значение, передаваемое в **-d**. Если в имени DSN указано **Trusted_Connection=yes**, используется проверка подлинности Kerberos, а имя пользователя ( **–U**) и пароль ( **–P**), если они заданы, игнорируются.

Существующие сценарии, вызывающие `isql`, можно изменить для использования `sqlcmd`, определив следующий псевдоним: `alias isql="sqlcmd -D"`.  

## <a name="see-also"></a>См. также:  
[Подключение с помощью **bcp**](connecting-with-bcp.md)  
[Заметки о выпуске](release-notes-tools.md)
