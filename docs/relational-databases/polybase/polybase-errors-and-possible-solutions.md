---
title: Ошибки в PolyBase и возможные решения
description: Справочник по ошибкам в PolyBase и способам их устранения.
ms.date: 03/22/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 46f56288382c1b1928e6ad3081a8eea41e23bb5f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833831"
---
# <a name="polybase-errors-and-possible-solutions"></a>Ошибки в PolyBase и возможные решения

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

В этой статье описаны распространенные сценарии ошибок в PolyBase и способы их устранения.

Дополнительные сведения о мониторинге и устранении неполадок в PolyBase см. в статье [Мониторинг PolyBase и устранение неполадок](polybase-troubleshooting.md).

Сведения о стандартном расположении файлов журналов PolyBase в Windows и Linux см. в статье [Мониторинг PolyBase и устранение неполадок](polybase-troubleshooting.md#log-file-locations).


## <a name="error-messages-and-possible-solutions"></a>Сообщения об ошибках и возможные решения

### <a name="error-100001failed-to-generate-query-plan"></a>Error: "100001;Failed to generate query plan"

Ошибка "Не удалось создать план запроса" может возникать, если ядро СУБД SQL Server было исправлен с применением по крайней мере накопительного пакета обновления 8 (15.0.4073), но компонент PolyBase не был обновлен до той же сборки. Это может произойти при добавлении компонента PolyBase в существующий экземпляр SQL Server. Дополнительные сведения см. в разделе, посвященном [ошибке PolyBase 100001 (не удалось создать план запроса)](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-error-100001-failed-to-generate-query-plan/ba-p/2174693).

Всегда следуйте инструкциям по установке компонента PolyBase, переведя новый компонент на тот же уровень версии. При необходимости установите пакеты обновления (SP), накопительные обновления (CU) и (или) выпуски для общего распространения (GDR). Сведения о том, как определить версию PolyBase, см. в статье [Определение уровня версий, выпусков и обновлений SQL Server и компонентов](/troubleshoot/sql/general/determine-version-edition-update-level#polybase).

### <a name="service-account-change"></a>Изменение учетной записи службы

Пример сообщения об ошибке:

> 107035: сбой авторизации DMS, так как [домен\пользователь] не входит в группу [PdwDataMovementAccess] <BR>
> 107017: недопустимый заголовок управления DMS:

Эта ошибка, скорее всего, вызвана изменением учетной записи службы PolyBase. Чтобы изменить учетные записи служб для PolyBase Engine и служб перемещения данных PolyBase, удалите и вновь установите компонент PolyBase.


### <a name="data-movement-service-permissions-errors"></a>Ошибки разрешений для службы перемещения данных

Дополнительные сведения об устранении неполадок и решении проблем, связанных с разрешениями для службы перемещения данных, см. в статье [Разрешения для учетной записи службы PolyBase и типичные ошибки, возникающие при их отсутствии](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="windows-authentication-failure"></a>Сбой аутентификации Windows

Дополнительные сведения об устранении неполадок и решении проблем, связанных со сбоем аутентификации Windows, см. в статье [Разрешения для учетной записи службы PolyBase и типичные ошибки, возникающие при их отсутствии](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="cannot-execute-the-query-remote-query"></a>Невозможно выполнить запрос "Remote Query" 

Пример сообщения об ошибке:

> Сообщение 7320, уровень 16, состояние 110, строка 14<BR>
> Не удалось выполнить запрос "Remote Query" при помощи поставщика OLE DB "SQLNCLI11" для связанного сервера "(NULL)". Запрос прерван — достигнуто максимальное пороговое значение отклонения (строк: 0) при чтении из внешнего источника. Отклонено строк из общего числа обработанных строк (1): 1.
(/nation/sensors.ldjson.txt) Порядковый номер столбца: 0, ожидаемый тип данных: INT, ошибочное значение: {"id":"S2740036465E2B","time":"2016-02-26T16:59:02.9300000Z","temp":23.3,"hum":0.77,"wind":17,"press":1032,"loc":[-76.90914996169623,38.8929314364726]} (Ошибка преобразования столбца), ошибка: ошибка преобразования типа данных NVARCHAR в INT.

Помните, что эта ошибка может иметь свои производные. Имя первого отклоненного файла отображается в SQL Server Management Studio (SSMS) с ошибочными типами данных или значениями.

**Возможная причина**  
Причина этой ошибки заключается в том, что у файлов разные схемы. При указании на каталог язык DDL внешней таблицы PolyBase рекурсивно считывает все файлы в этом каталоге. При несоответствии столбца или типа данных эта ошибка может отображаться в SSMS.

**Возможное решение**  
Если данные каждой таблицы содержатся в одном файле, в разделе LOCATION укажите имя файла с предваряющим его каталогом внешних файлов. Если у таблиц несколько файлов с данными, поместите каждый набор файлов в отдельный каталог в Хранилище BLOB-объектов Azure. Для LOCATION укажите имя каталога, а не определенного файла. Это рекомендуемое решение.

**Пример**.  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Поддержка Kerberos

SQL Server настроен для доступа к поддерживаемому кластеру Hadoop. Протокол Kerberos не применяется в кластере Hadoop.

При выборе из внешней таблицы возникает следующая ошибка:

> Сообщение 105019, уровень 16, состояние 1, строка 55<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect: ошибка [не удалось создать экземпляр LoginClass] при доступе к внешнему файлу".<BR>
> Сообщение 7320, уровень 16, состояние 110, строка 55<BR>
> Не удалось выполнить запрос "Remote Query" при помощи поставщика OLE DB "SQLNCLI11" для связанного сервера "(NULL)". Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect: ошибка [не удалось создать экземпляр LoginClass] при доступе к внешнему файлу".

При опросе журнала сервера DWEngine отображается следующая ошибка:

> [Поток:16432] [EngineInstrumentation:EngineQueryErrorEvent] (ошибка, высокий приоритет):<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect: ошибка [com.microsoft.polybase.client.KerberosSecureLogin] при доступе к внешнему файлу".
Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect: ошибка [com.microsoft.polybase.client.KerberosSecureLogin] при доступе к внешнему файлу". ---> Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException: исключение Java возникло при вызове HdfsBridge_Connect: ошибка [com.microsoft.polybase.client.KerberosSecureLogin] при доступе к внешнему файлу.

**Возможная причина**  
Протокол Kerberos не включен в кластере Hadoop, но включен в файлах core-site.xml, yarn-site.xml или hdfs-site.xml, которые по умолчанию находятся в папке Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf. В Linux эти файлы по умолчанию расположены в папке /var/opt/mssql/binn/polybase/hadoop/conf/.

**Возможное решение**  
Закомментируйте сведения о безопасности Kerberos из указанных выше файлов.

Дополнительные сведения об устранении неполадок с Kerberos в PolyBase см. в статье [Устранение неполадок с подключением PolyBase к Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="internal-query-processor-error"></a>Внутренняя ошибка обработчика запросов

При выполнении запросов к внешней таблице возникает следующая ошибка:

> Сообщение 8680, уровень 17, состояние 5, строка 118<BR>
> Внутренняя ошибка обработчика запросов: Обработчик запросов столкнулся с непредвиденной ошибкой на этапе обработки удаленного запроса.

Журнал сервера DWEngine содержит следующие сообщения:

> [Поток: 5216] [ControlNodeMessenger:ErrorEvent] (ошибка, высокий приоритет): ***** в системе DMS есть отключенные узлы:<BR>
> [Поток: 5216] [ControlNodeMessenger:ErrorEvent] (ошибка, высокий приоритет): ***** в системе DMS есть отключенные узлы:<BR>
> [Поток: 5216] [ControlNodeMessenger:ErrorEvent] (ошибка, высокий приоритет): ***** в системе DMS есть отключенные узлы:<BR>

**Возможная причина**  
Причиной этой ошибки может быть то, что SQL Server не был перезапущен после настройки PolyBase.

**Возможное решение**  
Перезапуск SQL Server. Проверьте журнал сервера DWEngine, чтобы убедиться в отсутствии отключений DMS после перезапуска.


### <a name="user-needed-for-hdfs-access"></a>Пользователь, необходимый для доступа к HDFS

**Сценарий.**  
SQL Server подключен к незащищенному кластеру Hadoop (протокол Kerberos не включен). В PolyBase настроена принудительная отправка вычислений в кластер Hadoop.

**Пример запроса**  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

Появляется сообщение об ошибке примерно следующего содержания: 

> Сообщение 105019, уровень 16, состояние 1, строка 1<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове JobSubmitter_PollJobStatus, ошибка [java.net.ConnectException: сбой вызова с big1506sql2016/172.16.1.4 на 0.0.0.0:10020 с исключением подключения, java.net ConnectException: в подключении отказано, дополнительные сведения недоступны; общие сведения см. на странице http://wiki.apache.org/hadoop/ConnectionRefused ] при доступе к внешнему файлу".<BR>
> Поставщик OLE DB "SQLNCLI11" для связанного сервера "(NULL)" вернул сообщение "Неопределенная ошибка".<BR>
> Сообщение 7421, уровень 16, состояние 2, строка 1<BR>
> Не удалось получить набор строк от поставщика OLE DB "SQLNCLI11" для связанного сервера "(NULL)". .<BR>

Ошибка в журнале Hadoop YARN:
> Ошибка в настройке задания: org.apache.hadoop.security.AccessControlException: отказ в разрешении: пользователь=pdw_user, доступ=WRITE, inode="/user":hdfs:hdfs:drwxr-xr-x в org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkFsPermission(FSPermissionChecker.java:265), в org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:251), в org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:232), в org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:176), в org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5525)

**Возможная причина**  
При отключенном протоколе Kerberos в PolyBase будет использоваться пользователь pdw_user для доступа к HDFS и отправки заданий MapReduce.

**Возможное решение**  
В Hadoop создайте пользователя pdw_user и предоставьте ему необходимые разрешения для каталогов, используемых при обработке MapReduce. Также убедитесь, что pdw_user является владельцем каталога HDFS /user/pdw_user.

Ниже приведен пример создания домашнего каталога и назначения разрешений для pdw_user.

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

После этого убедитесь, что у pdw_user есть разрешения на чтение, запись и выполнение для каталога /user/pdw_user. Убедитесь, что у каталога/tmp есть разрешения 777.

Дополнительные сведения об устранении неполадок с Kerberos в PolyBase см. в статье [Устранение неполадок с подключением PolyBase к Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="java-memory-error-due-to-utf-8"></a>Ошибка памяти Java из-за UTF-8

**Сценарий.**  
SQL Server с PolyBase настроен для работы с кластером Hadoop или Хранилищем BLOB-объектов Azure. Любой запрос Select завершается со следующей ошибкой:

> Сообщение 106000, уровень 16, состояние 1, строка 1<BR>
> Размер кучи Java<BR>

**Возможная причина**  
Недопустимые входные данные могут вызвать ошибку нехватки памяти Java. Возможно, файл имеет формат, отличный от UTF-8. Служба DMS пытается прочитать весь файл как одну строку. Так как она не может декодировать разделитель строк, это вызывает ошибку пространства кучи Java.

**Возможное решение**  
Преобразуйте файл в формат UTF-8, так как в настоящее время PolyBase требует использовать его для файлов с разделителями текста.

### <a name="hadoop-connectivity-configuration"></a>Настройка подключения Hadoop

При настройке подключения SQL Server PolyBase к Хранилищу BLOB-объектов Azure появляется следующее сообщение об ошибке в SQL Server:

> Сообщение 105019, уровень 16, состояние 1, строка 74<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect: ошибка [не найден параметр FileSystem для схемы wasbs] при доступе к внешнему файлу".<BR>

**Возможная причина**  
Для подключения Hadoop не задано значение конфигурации для доступа к Хранилищу BLOB-объектов Azure.

**Возможное решение**  
Задайте для подключения Hadoop значение (желательно 7), которое поддерживает Хранилище BLOB-объектов Azure, и перезапустите SQL Server. Список значений для подключения и поддерживаемых типов см. в статье [Конфигурация подключения к PolyBase (Transact-SQL)](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments).


### <a name="create-table-as-select-error"></a>Ошибка при выполнении инструкции Create Table As Select

**Сценарий.**  
При попытке экспорта данных в Хранилище BLOB-объектов Azure или в файловую систему Hadoop с помощью синтаксиса PolyBase CREATE EXTERNAL TABLE AS SELECT (CETAS) из SQL Server происходит сбой со следующим сообщением об ошибке:

> Сообщение 156, уровень 15, состояние 1, строка 177<BR>
> Неправильный синтаксис около ключевого слова "WITH".<BR>
> Сообщение 319, уровень 15, состояние 1, строка 177<BR>
> Неправильный синтаксис около ключевого слова "with". Если эта инструкция является обобщенным табличным выражением, предложением xmlnamespaces или предложением в контексте отслеживания изменений, предыдущую инструкцию необходимо завершить точкой с запятой.<BR>

**Возможная причина**  
При экспорте данных в Hadoop или хранилище BLOB-объектов Azure с помощью PolyBase передаются только данные без имен столбцов (метаданных), как определено в команде CREATE EXTERNAL TABLE.

**Возможное решение**  
Сначала создайте внешнюю таблицу, а затем используйте инструкцию INSERT INTO SELECT для экспорта во внешнее расположение. Пример кода см. в статье [Сценарии запросов PolyBase](polybase-queries.md#export-data).


### <a name="create-external-table-from-azure-blob-storage-fails"></a>Не удается создать внешнюю таблицу из Хранилища BLOB-объектов Azure

**Сценарий.**  
Хранилище данных SQL настроено для импорта данных из Хранилища BLOB-объектов Azure. Создание внешней таблицы завершается сбоем со следующим сообщением:

> Сообщение 105019, уровень 16, состояние 1, строка 34<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_IsDirExist. Сообщение об исключении Java: com.microsoft.azure.storage.StorageException, серверу не удалось аутентифицировать запрос. Убедитесь, что значение заголовка авторизации сформировано правильно, включая подпись. Ошибка [com.microsoft.azure.storage.StorageException: серверу не удалось аутентифицировать запрос. Убедитесь, что значение заголовка авторизации сформировано правильно, включая подпись.] при доступе к внешнему файлу".<BR>

**Возможная причина**  
При создании учетных данных для базы данных использовался неподходящий ключ Хранилища Azure.

**Возможное решение**  
Удалите все связанные объекты (т. е. источник данных, формат файла), а затем удалите и повторно создайте учетные данные для базы данных, используя подходящий ключ хранилища.


### <a name="kerberos-configuration-capitalization"></a>Использование прописных букв в конфигурации Kerberos

**Сценарий.**  
SQL Server настроен для работы с кластером Cloudera, в котором включен протокол Kerberos. После всех изменений конфигурации SQL Server был перезагружен. Ядро PolyBase и служба перемещения данных PolyBase работают после перезагрузки. Возвращается следующее сообщение об ошибке:

Источник данных настроен без указания того, где расположено средство регистрации заданий:  
> org.apache.hadoop.fs.FileSystem, не удалось создать экземпляр поставщика org.apache.hadoop.fs.viewfs.ViewFileSystem.

Источник данных настроен с указанием того, где расположено средство регистрации заданий:  
> Ошибка [не удается получить область Kerberos] при доступе к внешнему файлу.

**Возможная причина**  
Значение Kerberos неправильно задано для свойства "hadoop.security.authentication" в файле Coresite.xml.

**Возможное решение**  
В файле Coresite.xml для свойства "hadoop.security.authentication" должно быть задано значение KERBEROS (все символы в верхнем регистре). 

Дополнительные сведения об устранении неполадок с Kerberos в PolyBase см. в статье [Устранение неполадок с подключением PolyBase к Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="mapred-sitexml-missing-needed-values"></a>В файле mapred-site.xml отсутствуют необходимые значения

**Сценарий.**  
SQL Server или платформа APS настроены для работы с поддерживаемым кластером HDP. Запросы, для которых не требуется применять метод pushdown, завершаются сбоем, если включен параметр принудительного применения pushdown. При этом появляются следующие сообщения об ошибке:

> Сообщение 7320, уровень 16, состояние 110, строка 35<BR>
> Не удалось выполнить запрос "Remote Query" при помощи поставщика OLE DB "SQLNCLI11" для связанного сервера "(NULL)". Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове JobSubmitter_PollJobStatus: ошибка [org.apache.hadoop.ipc.RemoteException(java.lang.NullPointerException): java.lang.NullPointerException<BR>
> в org.apache.hadoop.mapreduce.v2.hs.HistoryClientService$HSClientProtocolHandler.getTaskAttemptCompletionEvents(HistoryClientService.java:277),<BR>
> в org.apache.hadoop.mapreduce.v2.api.impl.pb.service.MRClientProtocolPBServiceImpl.getTaskAttemptCompletionEvents(MRClientProtocolPBServiceImpl.java:173),<BR>
> в org.apache.hadoop.yarn.proto.MRClientProtocol$MRClientProtocolService$2.callBlockingMethod(MRClientProtocol.java:283),<BR>
> в org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:619),<BR>
> в org.apache.hadoop.ipc.RPC$Server.call(RPC.java:962),<BR>
> в org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2127),<BR>
> в org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2123),<BR>
> в java.security.AccessController.doPrivileged(собственный метод),<BR>
> в javax.security.auth.Subject.doAs(Subject.java:415),<BR>
> в org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1671),<BR>
> в org.apache.hadoop.ipc.Server$Handler.run(Server.java:2121)<BR>
> ] при доступе к внешнему файлу.<BR>

**Возможная причина**  
В файле mapred-site.xml отсутствуют некоторые необходимые значения, которые проверяют промежуточные и конечные результаты.

**Возможное решение**  
Добавьте следующие свойства и укажите для них правильные значения, как показано в Ambari в файле mapred-site.xml для SQL Server. 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>Настройка доступа по имени узла 

**Сценарий.**  
SQL Server настроен для доступа к поддерживаемому кластеру Hadoop. При создании внешней таблицы возникает одна из следующих ошибок:

> Не удалось выполнить запрос "Remote Query" при помощи поставщика OLE DB "SQLNCLI11" для связанного сервера "(NULL)". 110802: произошла внутренняя ошибка DMS, вызвавшая сбой операции. Сведения: Исключение: Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DmsSqlNativeException, Сообщение: SqlNativeBufferReader.Run, ошибка в OdbcExecuteQuery: SqlState: 42000, NativeError: 8680, "Ошибка вызова: SQLExecDirect(this->GetHstmt(), (SQLWCHAR *)statementText, SQL_NTS), код возврата SQL: -1 | Сведения об ошибке SQL: SrvrMsgState: 26, SrvrSeverity: 17, Ошибка <1>: ErrorMsg: [Microsoft][драйвер ODBC 13 для SQL Server][SQL Server] Внутренняя ошибка обработчика запросов: обработчик запросов обнаружил непредвиденную ошибку во время обработки удаленной фазы запроса. | Ошибка вызова: pReadConn->ExecuteQuery(statementText, bufferFormat) | состояние: FFFF, номер: 24, активных подключений: 8", Строка подключения: Driver={pdwodbc};APP=RCSmall-DmsNativeReader:WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433;Trusted_Connection=yes;AutoTranslate=no;Server=\\.\pipe\sql\query<BR>

> [Поток: 30544] [AbstractReaderWorker:ErrorEvent] (ошибка, высокий приоритет): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7:<BR>
> Не удалось получить блок: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Не удалось получить блок: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> в Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsBridgeReadAccess.Read(MemoryBuffer buffer, Boolean& isDone),<BR>
> в Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DataReader.ExternalMoveBufferReader.Read(),<BR>
> в Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.ReadAndSendData(),<BR>
> в Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execute(Object status)<BR>

**Возможная причина**  
Это сообщение об ошибке может отображаться, если конфигурация кластера Hadoop разрешает доступ к узлам данных за пределами кластера только при использовании имени узла, а не IP-адреса.

**Возможное решение**  
Добавьте следующие сведения в файл hdfs-site.xml на стороне клиента (SQL Server). При такой конфигурации узел имени будет возвращать URI для узлов данных с указанием имени узла, а не внутреннего IP-адреса.

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>Организация папок приводит к переполнению памяти

**Сценарий.**  
SQL Server выполняет запрос PolyBase к каталогу с большим количеством файлов (>30 000 файлов, рекурсивно содержащихся в нем по указанному пути). При этом появляется одно из следующих сообщений об ошибке:

> Сообщение 105019, уровень 16, состояние 1, строка 1<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_GetFileNameByIndex. Сообщение об исключении Java: переполнение памяти при сборке мусора: ошибка [переполнение памяти при сборке мусора] при доступе к внешнему файлу".<BR>

> Сообщение 105019, уровень 16, состояние 1, строка 1<BR>
> Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_GetDirectoryFiles. Сообщение об исключении Java: пространство кучи Java: ошибка [пространство кучи Java] при доступе к внешнему файлу".

**Возможная причина**  
При обработке пути PolyBase выполняет перечисление всех файлов по этому пути. При этом возникает переполнение фиксированной памяти сведениями о структуре данных, используемой для представления файлов. При большом количестве файлов переполнение становится заметным и может в конечном итоге исчерпать всю память, доступную для виртуальной машины Java.

**Возможное решение**  
Разместите данные в нескольких каталогах так, чтобы каждый каталог содержал подмножество файлов. Запрос разделите на несколько запросов, чтобы каждый из них по очереди работал с частью исходного пути, а таблицы материализуйте как таблицы SQL Server (перед их объединением).

Пример. Предположим, что данные внешней таблицы находятся в следующем расположении: Orders/file1.txt,..., file30K.txt.

Измените структуру расположения файлов, разместив их в обычной структуре файловых разделов вида Orders/*ГГГГ* / *ММ* / *ДД*/file1.txt.
Укажите для внешней таблицы путь к расположенному ниже уровню каталога, например "месяц" (ММ) или "день" (ДД), и импортируйте файлы в таблицы SQL Server по частям, а затем добавьте их как часть одной таблицы.
Даже если у вас уже есть подходящая структура каталогов, выполните шаг 2, чтобы иметь возможность работать с большим количеством файлов, избегая переполнения памяти виртуальной машины Java.


### <a name="unexpected-characters-in-configuration-files"></a>Недопустимые символы в файлах конфигурации

**Сценарий.**  
Настройка SQL Server или платформы APS для работы с кластером Hadoop предполагает внесение изменений в yarn-site.xml, hdfs-site.xml и другие файлы конфигурации. Отображается следующее сообщение об ошибке SQL Server:

> Сообщение 105019, уровень 16, состояние 1, строка 1<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Сбой доступа к EXTERNAL TABLE из-за внутренней ошибки: "Исключение Java возникло при вызове HdfsBridge_Connect. Сообщение об исключении Java: com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: недопустимых байтов (1) в последовательности UTF-8 из байтов (1). Ошибка [com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: недопустимых байтов (1) в последовательности UTF-8 из байтов (1)] при доступе к внешнему файлу". ---><BR>

**Возможная причина**  
Это может произойти при копировании текста на веб-сайте или в чате и его последующей вставке в файлы конфигурации. При этом в файлы конфигурации могут попасть лишние или непечатаемые символы.

**Возможное решение**  
Откройте файлы в другом текстовом редакторе (не в Блокноте), найдите такие символы и удалите их. Перезапустите нужные службы. 


## <a name="see-also"></a>См. также раздел

[Мониторинг PolyBase и устранение неполадок](polybase-troubleshooting.md)  
[Устранение неполадок с подключением PolyBase к Kerberos](polybase-troubleshoot-connectivity.md)  

