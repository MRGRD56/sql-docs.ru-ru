---
title: Устранение неполадок в SQL Server на Linux
description: Сведения об устранении неполадок в Microsoft SQL Server, работающем в Linux или контейнере Docker. Также из этой статьи вы узнаете, где найти сведения о поддерживаемых функциях и известных ограничениях.
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 1f2df7f7cb0660cb879334a1099f6b7ce639d211
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464642"
---
# <a name="troubleshoot-sql-server-on-linux"></a>Устранение неполадок в SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Этот документ описывает устранение неполадок в Microsoft SQL Server, работающих в Linux или контейнере Docker. При устранении неполадок SQL Server на Linux не забудьте ознакомиться с поддерживаемыми функциями и известными ограничениями в [заметках о выпуске SQL Server на Linux](sql-server-linux-release-notes.md).

> [!TIP]
> Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.yml).

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> Устранение неполадок при сбоях подключения
Если у вас возникли трудности при подключении к SQL Server на базе Linux, нужно проверить следующее.

- Если вы не можете подключиться локально с помощью **localhost**, попробуйте использовать вместо этого IP-адрес 127.0.0.1. Возможно, **localhost** не сопоставлен должным образом с этим адресом.

- Убедитесь, что IP-адрес или имя сервера доступны с клиентского компьютера.

   > [!TIP]
   > Чтобы найти IP-адрес своего компьютера Ubuntu, можно выполнить команду ifconfig, как показано в следующем примере.
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Для Red Hat можно использовать ip addr, как показано в следующем примере.
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Исключение из этой методики связано с виртуальными машинами Azure. Для виртуальных машин Azure [найдите общедоступный IP-адрес для виртуальной машины на портале Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Если это применимо, убедитесь, что открыт порт SQL Server (по умолчанию 1433) в брандмауэре.

- Для виртуальных машин Azure убедитесь в наличии [правила группы безопасности сети для порта SQL Server по умолчанию](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Убедитесь, что имя пользователя и пароль не содержат опечатки, лишние пробелы или неверный регистр.

- Попробуйте явно задать протокол и номер порта с именем сервера, как показано в следующем примере: **tcp:имя_сервера,1433**.

- Проблемы с сетевым подключением также могут вызвать ошибки подключения и истечение времени ожидания. Проверив сведения о подключении и возможность сетевого подключения, повторите попытку подключения.

## <a name="manage-the-sql-server-service"></a>Управление службой SQL Server

В следующих разделах показано, как запустить, остановить, перезапустить службу SQL Server и проверить ее состояние.

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Управление службой mssql-server в Red Hat Enterprise Linux (RHEL) и Ubuntu 

Проверьте состояние службы SQL Server с помощью следующей команды.

   ```bash
   sudo systemctl status mssql-server
   ```

Вы можете останавливать, запускать или перезапускать службу SQL Server по мере необходимости, используя следующие команды.

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Управление выполнением контейнера Docker mssql

Вы можете получить состояние и идентификатор контейнера для последнего созданного контейнера Docker SQL Server, выполнив следующую команду (идентификатор находится в столбце **CONTAINER ID**).

   ```bash
   sudo docker ps -l
   ```
   
Вы можете останавливать или перезапускать службу SQL Server по мере необходимости, используя следующие команды.
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Дополнительные советы по устранению неполадок в Docker см. в статье [Устранение неполадок с контейнерами Docker в SQL Server](./sql-server-linux-docker-container-troubleshooting.md).

## <a name="access-the-log-files"></a>Доступ к файлам журнала
   
Подсистема SQL Server записывает данные журнала в файл /var/opt/mssql/log/errorlog в установках Linux и Docker. Для просмотра этого каталога вы должны находиться в режиме суперпользователя.

Установщик записывает данные журнала сюда: /var/opt/mssql/setup-< метка времени, отражающая время установки>. Вы можете просмотреть файлы журнала ошибок с помощью любого совместимого с UTF-16 средства, например vim или cat. 

   ```bash
   sudo cat errorlog
   ```

При желании можно также преобразовать файлы в UTF-8, чтобы прочесть их с помощью more или less, используя следующую команду.
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Расширенные события

Расширенные события можно запрашивать с помощью команды SQL.  Дополнительные сведения о расширенных событиях см. [здесь](../relational-databases/extended-events/extended-events.md).

## <a name="crash-dumps"></a>Аварийные дампы 

Дампы находятся в каталоге журналов в Linux. В каталоге /var/opt/mssql/log находятся дампы Linux Core (с расширением TAR.GZ2) или минидампы SQL (с расширением MDMP).

Для дампов Core 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Для дампов SQL 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Запуск SQL Server с минимальной конфигурацией или в однопользовательском режиме

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Запуск SQL Server с минимальной конфигурацией
Эта функция полезна в случае, если установленные значения конфигурации (например, слишком большой объем выделяемой памяти) не позволяют выполнить запуск сервера.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Запуск SQL Server в однопользовательском режиме
При определенных обстоятельствах экземпляр SQL Server нужно запустить в однопользовательском режиме, используя параметр запуска -m. Например, может понадобиться изменить параметры конфигурации сервера, восстановить поврежденную базу данных master или другую системную базу данных. Например, может понадобиться изменить параметры конфигурации сервера, восстановить поврежденную базу данных master или другую системную базу данных.   

Запуск SQL Server в однопользовательском режиме
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Запуск SQL Server в однопользовательском режиме с использованием SQLCMD
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Чтобы избежать проблем с запуском в дальнейшем, SQL Server в Linux следует запускать с указанием пользователя "mssql". Пример: "sudo -u mssql /opt/mssql/bin/sqlservr [параметры запуска]" 

Если вы случайно запустили SQL Server с другим пользователем, нужно изменить владельца файлов базы данных SQL Server обратно на пользователя mssql перед запуском SQL Server с использованием systemd. Например, чтобы изменить владельца всех файлов базы данных в /var/opt/mssql на пользователя mssql, выполните следующую команду.

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Перестроение системных баз данных
В качестве крайней меры можно выбрать перестроение базы данных master и шаблона базы данных до версий по умолчанию.

> [!WARNING]
> Эти действия приведут к **удалению всех системных данных SQL Server**, которые вы настроили. Сюда входят сведения о пользовательских базах данных (но не сами пользовательские базы данных). Кроме того, будут удалены другие сведения, хранящиеся в системных базах данных, включая следующие: сведения о главном ключе, все сертификаты, загруженные в базу данных master, пароль для входа пользователя SA, сведения о заданиях из msdb, сведения о компоненте Database Mail из msdb и параметры sp_configure. Используйте эти возможности, только если осознаете последствия.

1. Остановите SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Запустите **sqlservr** с параметром **force-setup**. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > См. предыдущее предупреждение. Кроме того, необходимо запустить его от имени пользователя **mssql**, как указано здесь.

1. После появления сообщения "Восстановление завершено" нажмите клавиши CTRL+C. Это приведет к завершению работы SQL Server.

1. Перенастройте пользователя SA.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Запустите SQL Server и перенастройте сервер. Сюда входит восстановление или повторное подключение любых пользовательских баз данных.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>Повышение производительности

Существует множество факторов, влияющих на производительность, в том числе структура базы данных, оборудование и требования к рабочим нагрузкам. Если вы хотите повысить производительность, начните с изучения рекомендаций, описанных в статье [Рекомендации по производительности и рекомендации по конфигурации для SQL Server в Linux](sql-server-linux-performance-best-practices.md). Затем изучите некоторые из доступных средств для устранения проблем с производительностью.

- [Хранилище запросов](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Динамические административные представления системы](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Панель мониторинга производительности в SQL Server Management Studio](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)

## <a name="common-issues"></a>Распространенные проблемы

1. Невозможно подключиться к удаленному экземпляру SQL Server.

   См. раздел об устранении неполадок в статье [Подключение к SQL Server в Linux](#connection).

2. ОШИБКА: длина имени узла не должна превышать 15 символов.

   Это известная проблема, которая возникает каждый раз, когда имя компьютера, пытающегося установить пакет Debian с SQL Server, имеет длину более 15 символов. Сейчас способы обхода этой проблемы ограничены изменением имени компьютера. Один из способов добиться этого заключается в изменении файла имени узла и перезагрузке компьютера. Это подробно описано в следующем [руководстве](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/).

3. Сброс пароля системного администрирования (SA).

   Если вы забыли пароль системного администратора или по какой-либо причине хотите сбросить его, выполните следующие действия.

   > [!NOTE]
   > Следующие действия временно приостанавливают работу службы SQL Server.

   Войдите в терминал узла, выполните следующие команды и следуйте инструкциям на экране, чтобы сбросить пароль SA.

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Использование специальных знаков в пароле.

   Если в пароле для входа в SQL Server используются специальные знаки, может потребоваться экранировать их с помощью обратной косой черты при их использовании в команде Linux в терминале. Например, нужно всегда экранировать знак доллара ($) при его использовании в команде терминала/скрипте оболочки.

   Не работает:

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Работает:

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Ресурсы: [Специальные знаки](https://tldp.org/LDP/abs/html/special-chars.html)
   [Экранирование](https://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]