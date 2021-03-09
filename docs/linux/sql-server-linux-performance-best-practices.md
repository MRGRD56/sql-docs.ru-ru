---
title: Рекомендации по обеспечению производительности SQL Server на Linux
description: В этой статье приводятся рекомендации по обеспечению производительности SQL Server на Linux.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 01/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 42638520dd0d7391a10217dc7f7fdd2ce708189f
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247496"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-on-linux"></a>Рекомендации по производительности и конфигурации для SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье приводятся рекомендации по повышению производительности приложений баз данных, подключающихся к SQL Server на Linux. Эти рекомендации относятся именно к платформе Linux. При этом действуют все стандартные рекомендации в отношении SQL Server, например касающиеся проектирования индекса.

Ниже приводятся рекомендации по настройке SQL Server и операционной системы (ОС) Linux.

## <a name="linux-os-configuration"></a>Конфигурация ОС Linux

Чтобы обеспечить максимальную производительность системы SQL Server, рекомендуется использовать приведенные ниже параметры конфигурации ОС Linux.

### <a name="storage-configuration-recommendation"></a>Рекомендации по конфигурации хранилища

#### <a name="use-storage-subsystem-with-appropriate-iops-throughput-and-redundancy"></a>Использование подсистемы хранения с соответствующей скоростью ввода-вывода, пропускной способностью и избыточностью

Подсистема хранения, в которой размещаются данные, журналы транзакций и другие связанные файлы (например, файлы контрольных точек для выполняющейся в памяти OLTP), должна легко справляться со средней и пиковой рабочей нагрузкой. Как правило, в локальных средах поставщик хранилища поддерживает соответствующую аппаратную конфигурацию RAID с чередованием нескольких дисков для обеспечения надлежащей скорости ввода-вывода, пропускной способности и избыточности. Однако эта поддержка зависит от конкретного поставщика хранилища и конкретной архитектуры системы хранения.

Для сервера SQL Server на Linux, развернутого на Виртуальных машинах Azure, рекомендуется использовать программную реализацию RAID, чтобы обеспечить выполнение требований к скорости ввода-вывода и пропускной способности. Сведения о различных аспектах, связанных с системой хранения при настройке SQL Server на виртуальных машинах Azure, см. в следующей статье: [Настройка хранилища для виртуальных машин SQL Server](/azure/azure-sql/virtual-machines/windows/storage-configuration)

Ниже приведен пример создания программного RAID-массива в Linux на Виртуальных машинах Azure. Однако вам следует использовать надлежащее количество дисков данных для достижения требуемой пропускной способности и скорости ввода-вывода с учетом объема данных, размера журнала транзакций и базы данных tempdb. В этом примере к виртуальной машине Azure подключены восемь дисков данных: 4 для размещения файлов данных, 2 — для журналов транзакций и 2 — для рабочей нагрузки tempdb.

```bash
# To locate the devices (for example /dev/sdc) for RAID creation, use the lsblk command
# For Data volume, using 4 devices, in RAID 5 configuration with 8KB stripes
mdadm --create --verbose /dev/md0 --level=raid5 --chunk=8K --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# For Log volume, using 2 devices in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md1 --level=raid10 --chunk=64K --raid-devices=2 /dev/sdg /dev/sdh

# For tempdb volume, using 2 devices in RAID 0 configuration with 64KB stripes
mdadm --create --verbose /dev/md2 --level=raid0 --chunk=64K --raid-devices=2 /dev/sdi /dev/sdj
```

#### <a name="disk-partitioning-and-configuration-recommendations"></a>Рекомендации по разбиению дисков на разделы и их настройке

Для SQL Server рекомендуется использовать конфигурации RAID. Размер полосы (sunit) и ширина полосы развернутой файловой системы должны соответствовать геометрии RAID. Ниже приведен пример для тома журнала в файловой системе XFS.

```bash
# Creating a log volume, using 6 devices, in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md3 --level=raid10 --chunk=64K --raid-devices=6 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf

mkfs.xfs /dev/sda1 -f -L log 
meta-data=/dev/sda1              isize=512    agcount=32, agsize=18287648 blks 
         =                       sectsz=4096  attr=2, projid32bit=1 
         =                       crc=1        finobt=1, sparse=1, rmapbt=0 
         =                       reflink=1 
data     =                       bsize=4096   blocks=585204384, imaxpct=5 
         =                       sunit=16     swidth=48 blks 
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1 
log      =internal log           bsize=4096   blocks=285744, version=2 
         =                       sectsz=4096  sunit=1 blks, lazy-count=1 
realtime =none                   extsz=4096   blocks=0, rtextents=0 
```

Для него используется массив RAID-10 с шестью дисками и размером полосы 64 КБ. Вы можете видеть следующее.

- Значение sunit=16 * bsize=4096 равно 64 КБ, что соответствует размеру полосы.
- Значение swidth = 48 blks, деленное на sunit, равно 3, что соответствует количеству дисков в массиве за вычетом дисков четности.

#### <a name="file-system-configuration-recommendation"></a>Рекомендации по конфигурации файловой системы

SQL Server поддерживает файловые системы EXT4 и XFS для размещения базы данных, журналов транзакций и дополнительных файлов, таких как файлы контрольных точек для выполняющейся в памяти OLTP. Корпорация Майкрософт рекомендует использовать файловую систему XFS для размещения файлов данных и журнала транзакций SQL Server.

```bash
# Formatting the volume with XFS filesystem
mkfs.xfs /dev/md0 -f -L datavolume
mkfs.xfs /dev/md1 -f -L logvolume
mkfs.xfs /dev/md2 -f -L tempdb
```

> [!NOTE]
> При создании и форматировании тома XFS можно настроить файловую систему XFS так, чтобы она не учитывала регистр. В экосистеме Linux такая конфигурация применяется нечасто, но может потребоваться в целях совместимости.
>
> Пример: mkfs.xfs /dev/md0 -f -n version=ci -L datavolume
>
> В этом примере параметры `-n version=ci` служат для настройки файловой системы XFS как не учитывающей регистр.

##### <a name="persistent-memory-filesystem-recommendation"></a>Рекомендация в отношении файловой системы PMEM

Для базовой файловой системы на устройствах PMEM следует настроить выделение блоков по 2 МБ. Дополнительные сведения на эту тему см. в разделе [Технические вопросы](sql-server-linux-configure-pmem.md#technical-considerations).

#### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>Отключение даты и времени последнего доступа к файлам данных и журналов SQL Server в файловых системах

Чтобы обеспечить автоматическое повторное подключение дисков, подключенных к системе, после перезагрузки, их необходимо добавить в файл `/etc/fstab`. Также для ссылки на диск настоятельно рекомендуется использовать в файле `/etc/fstab` идентификатор UUID (глобальный уникальный идентификатор), а не просто имя устройства (например, `/dev/sdc1`).

Настоятельно рекомендуется применять атрибут **noatime** с любой файловой системой, которая используется для хранения файлов данных и журналов SQL Server. Сведения о задании этого атрибута см. в документации по Linux. Ниже приведен пример включения параметра **noatime** для тома, подключенного к виртуальной машине Azure.

Запись точки подключения в ***/etc/fstab***

```bash
UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" /data1 xfs,rw,attr2,noatime 0 0
```

В приведенном выше примере UUID представляет устройство, которое можно найти с помощью команды ***blkid***.

#### <a name="sql-server-and-forced-unit-access-fua-io-subsystem-capability"></a>SQL Server и возможность принудительного доступа к модулям (FUA) в подсистеме ввода-вывода

В некоторых версиях поддерживаемых дистрибутивов Linux поддерживается возможность FUA подсистемы ввода-вывода, которая обеспечивает устойчивость данных. SQL Server использует возможность FUA для обеспечения эффективного и надежного ввода-вывода при выполнении рабочей нагрузки. Дополнительные сведения о поддержке возможности FUA в различных дистрибутивах Linux и ее влиянии на SQL Server см. в следующем блоге: [SQL Server в Linux: внутренние методы принудительного доступа к модулям (FUA)](https://bobsql.com/sql-server-on-linux-forced-unit-access-fua-internals/).

SUSE Linux Enterprise Server 12 с пакетом обновления 5 (SP5) и Red Hat Enterprise Linux 8.0 и более поздних версий поддерживают возможность FUA в подсистеме ввода-вывода. Если вы используете SQL Server 2017 с накопительным пакетом обновления 6 (CU6) или выше либо SQL Server 2019, следует использовать указанную ниже конфигурацию для высокопроизводительной, эффективной реализации ввода-вывода с функцией FUA.

Используйте приведенную ниже рекомендуемую конфигурацию, если выполняются указанные ниже условия.

- Используется SQL Server 2017 с накопительным пакетом обновления 6 (CU6) или выше либо SQL Server 2019.
- Используется дистрибутив и версия Linux, поддерживающие возможность FUA (Red Hat Enterprise Linux 8.0 или более поздней версии либо SUSE Linux Enterprise Server 12 с пакетом обновления 5 (SP5))
- Подсистема хранения и (или) оборудование поддерживают возможность FUA и настроены для ее использования.

Рекомендуемая конфигурация

1. Включите флаг трассировки 3979 в качестве параметра запуска.
2. Используйте **mssql-conf** для настройки `control.writethrough = 1` и `control.alternatewritethrough = 0`.

Для почти всех остальных конфигураций, которые не соответствуют указанным выше условиям, рекомендуется приведенная ниже конфигурация.

1. Включите флаг трассировки 3982 в качестве параметра запуска (используется по умолчанию для SQL Server в экосистеме Linux) и убедитесь в том, что флаг трассировки 3979 не включен в качестве параметра запуска.
2. Используйте **mssql-conf** для настройки `control.writethrough = 1` и `control.alternatewritethrough = 1`.

### <a name="kernel-and-cpu-settings-for-high-performance"></a>Параметры ядра и ЦП для высокой производительности

В этом разделе приводятся рекомендованные параметры ОС Linux для обеспечения высокой производительности и пропускной способности системы SQL Server. Сведения о настройке этих параметров см. в документации по ОС Linux. С помощью службы [***Tuned***](https://tuned-project.org) можно настроить многие из описанных ниже конфигураций ЦП и ядра.

#### <a name="using-tuned-to-configure-kernel-settings"></a>Настройка параметров ядра с помощью службы ***Tuned***

В Red Hat Enterprise Linux (RHEL) при использовании профиля пропускной способности [Tuned](https://tuned-project.org) некоторые параметры ядра и ЦП задаются автоматически (за исключением режима C-States). Начиная с RHEL 8.0 используется профиль ***Tuned** _ под названием _ *mssql**, разработанный совместно с Red Hat и предоставляющий возможность более детальной настройки производительности для рабочих нагрузок SQL Server. Этот профиль включает профиль пропускной способности RHEL. Определения этого профиля представлены ниже для сравнения с другими дистрибутивами Linux и выпусками RHEL, не содержащими этого профиля.

В SUSE Linux Enterprise Server 12 с пакетом обновления 5 (SP5), Ubuntu 18.04 и Red Hat Enterprise Linux 7.x пакет ***Tuned** _ можно установить вручную. Его можно использовать для создания и настройки профиля _ *mssql**, как описано ниже.

##### <a name="proposed-linux-settings-using-a-tuned-mssql-profile"></a>Рекомендуемые параметры Linux при использовании профиля Tuned mssql

```bash
#
# A Tuned configuration for SQL Server on Linux
#

[main]
summary=Optimize for Microsoft SQL Server
include=throughput-performance

[cpu]
force_latency=5

[sysctl]
vm.swappiness = 1
vm.dirty_background_ratio = 3
vm.dirty_ratio = 80
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.transparent_hugepages=always
# For multi-instance SQL deployments, use
# vm.transparent_hugepages=madvise
vm.max_map_count=1600000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.numa_balancing=0
kernel.sched_latency_ns = 60000000
kernel.sched_migration_cost_ns = 500000
kernel.sched_min_granularity_ns = 15000000
kernel.sched_wakeup_granularity_ns = 2000000
```

Чтобы включить этот профиль Tuned, сохраните определения в файле **tuned.conf** в папке `/usr/lib/tuned/mssql` и включите профиль, выполнив следующие команды:

```bash
chmod +x /usr/lib/tuned/mssql/tuned.conf
tuned-adm profile mssql
```

Проверьте, включен ли профиль, с помощью следующей команды:

```bash
tuned-adm active
```

или

```bash
tuned-adm list
```

#### <a name="cpu-settings-recommendation"></a>Рекомендации в отношении параметров ЦП

В приведенной ниже таблице представлены рекомендации по параметрам ЦП.

| Параметр | Значение | Дополнительные сведения |
|---|---|---|
| Регулятор частоты ЦП | производительность | См. описание команды **cpupower** |
| ENERGY_PERF_BIAS | производительность | См. описание команды **x86_energy_perf_policy** |
| min_perf_pct | 100 | См. документацию по режиму Intel p-state |
| C-States | Только C1 | Сведения о том, как включить только режим C1 C-States, см. в документации по Linux или вашей системе |

При использовании службы ***Tuned** _ описанным выше образом параметры регулятора частоты ЦП, ENERGY_PERF_BIAS и min_perf_pct настраиваются автоматически, так как профиль пропускной способности используется как основа для профиля _ *mssql**. Параметр C-States необходимо настроить вручную в соответствии с документацией, предоставляемой сообществом Linux или распространителем системы.

#### <a name="disk-settings-recommendations"></a>Рекомендации в отношении параметров дисков

В приведенной ниже таблице представлены рекомендации по параметрам дисков.

| Параметр | Значение | Дополнительные сведения |
|---|---|---|
| disk `readahead` | 4096 | См. описание команды `blockdev` |
| Параметры sysctl | kernel.sched_min_granularity_ns = 15000000<br/>kernel.sched_wakeup_granularity_ns = 2000000<br/>vm.dirty_ratio = 80<br/>vm.dirty_background_ratio = 3<br/>vm.swappiness = 1 | См. описание команды **sysctl** |

**Описание.**

- **vm.swappiness**: Этот параметр управляет относительным весовым коэффициентом, присвоенным выгрузке из памяти процесса среды выполнения по сравнению с кэшем файловой системы. Значение этого параметра по умолчанию — 60, то есть страницы выгружаются из памяти процесса среды выполнения и удаляются из кэша файловой системы в соотношении 60:140. Значение 1 указывает на то, что в физической памяти предпочтительнее сохранять процесс среды выполнения, а не кэш файловой системы. Так как SQL Server использует буферный пул в качестве кэша страниц данных и преимущественно выполняет запись в физическую память в обход кэша файловой системы для надежного восстановления, конфигурация с агрессивной подкачкой может быть выгодна для высокопроизводительных и выделенных серверов SQL Server.
Дополнительные сведения см. в [документации по параметру /proc/sys/vm/ — #swappiness](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/vm.html#swappiness).

- **vm.dirty_\*** : операции доступа SQL Server для записи файлов не кэшируются для выполнения требований к целостности данных. Эти параметры обеспечивают высокую производительность асинхронной записи и снижают влияние операций записи с кэшированием в Linux на подсистему ввода-вывода хранилища, разрешая кэширование достаточного объема содержимого, при этом ограничивая запись на диск.

- **kernel.sched_\*** : эти значения параметров представляют текущую рекомендацию по настройке алгоритма абсолютно справедливого планирования (CFS) в ядре Linux для повышения пропускной способности сети и подсистемы ввода-вывода хранилища с точки зрения межпроцессного вытеснения и возобновления потоков.

При использовании профиля **mssql** **_Tuned_ *_ настраиваются параметры _* vm.swappiness**, **vm.dirty_\* *_ и _* kernel.sched_\*** . Настройка `readahead` для диска с помощью команды `blockdev` производится на уровне устройства и должна выполняться вручную.

#### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>Автоматическая балансировка между узлами NUMA в системах с несколькими узлами NUMA

Если сервер SQL Server устанавливается в системе с несколькими узлами **NUMA**, указанный ниже параметр ядра **kernel.numa_balancing** включается по умолчанию. Чтобы обеспечить максимально эффективную работу SQL Server в системе **NUMA**, отключите автоматическую балансировку NUMA в системе с несколькими узлами NUMA.

```bash
sysctl -w kernel.numa_balancing=0
```

При использовании профиля **_Tuned_ *_ **mssql** настраивается параметр _* kernel.numa_balancing**.

#### <a name="kernel-settings-for-virtual-address-space"></a>Параметры ядра для виртуального адресного пространства

Значение параметра **vm.max_map_count** по умолчанию (65 536) может быть недостаточно большим для системы SQL Server. Поэтому измените значение **vm.max_map_count** минимум на 262144 для развертывания SQL Server и ознакомьтесь с разделом [Рекомендуемые параметры Linux при использовании профиля Tuned mssql](#proposed-linux-settings-using-a-tuned-mssql-profile) для дальнейшей настройки этих параметров ядра. Максимальное значение для vm.max_map_count равно 2147483647.

```bash
sysctl -w vm.max_map_count=1600000
```

При использовании профиля **_Tuned_ *_ **mssql** настраивается параметр _* vm.max_map_count**.

#### <a name="leave-transparent-huge-pages-thp-enabled"></a>Сохранение параметра Transparent Huge Pages (THP) во включенном состоянии

В большинстве систем Linux этот параметр конфигурации включен по умолчанию. Чтобы обеспечить максимально единообразную производительность, мы рекомендуем не отключать его. Однако в случае высокой активности страничной памяти в развертываниях SQL Server с несколькими экземплярами или при выполнении SQL Server с другими ресурсоемкими приложениями на сервере рекомендуется проверить производительность приложений после выполнения следующей команды:

```bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

Либо изменить профиль **mssql** **_Tuned_**, используя следующую строку:

```bash
vm.transparent_hugepages=madvise
```

После изменения следует активировать профиль **mssql**:

```bash
tuned-adm off
tuned-adm profile mssql
```

При использовании профиля **_Tuned_ *_ **mssql** настраивается параметр _* transparent_hugepage**.

#### <a name="network-setting-recommendations"></a>Рекомендации по настройке сети

Как и в случае с хранилищем и ЦП, существуют рекомендации в отношении сети. Они приведены ниже для справки. Указанные ниже параметры доступны не для всех сетевых карт. За указаниями обратитесь к поставщику конкретной сетевой карты. Протестируйте эти настройки в среде разработки перед их применением в рабочей среде. Параметры сопровождаются примерами, а используемые команды относятся к конкретному типу и поставщику сетевой карты.

1. Настройка размера буфера для сетевого порта. В приведенном ниже примере сетевая карта называется eth0. Это сетевая карта Intel. Для таких сетевых карт рекомендуется размер буфера 4 КБ (4096). Проверьте предварительно установленные максимальные значения и выполните настройку с помощью приведенных ниже примеров команд.

   ```bash
            #To check the pre-set maximums please run the command, example NIC name used here is:"eth0"
            ethtool -g eth0
            #command to set both the rx(recieve) and tx (transmit) buffer size to 4 KB. 
            ethtool -G eth0 rx 4096 tx 4096
            #command to check the value is properly configured is:
            ethtool -g eth0
   ```

2. Включение jumbo-кадров. Перед включением jumbo-кадров убедитесь в том, что они поддерживаются всеми сетевыми коммутаторами, маршрутизаторами и другими важными компонентами на пути передачи пакетов между клиентами и сервером SQL Server. Только в этом случае включение jumbo-кадров может повысить производительность. После включения jumbo-кадров подключитесь к SQL Server и измените размер сетевого пакета на 8060 с помощью `sp_configure`, как показано ниже.

   ```bash
            #command to set jumbo frame to 9014 for a Intel NIC named eth0 is
            ifconfig eth0 mtu 9014
            #verify the setting using the command:
            ip addr | grep 9014
   ```

   ```sql
            sp_configure 'network packet size' , '8060'
            go
            reconfigure with override
            go
   ```

3. По умолчанию рекомендуется задать порт для объединения запросов на прерывание RX/TX. В результате доставка прерываний будет корректироваться в направлении оптимизации задержки при низкой скорости передачи пакетов или повышения пропускной способности при высокой скорости их передачи. Имейте в виду, что этот параметр может быть доступен не во всех сетевых инфраструктурах. Необходимо убедиться в том, что он поддерживается. В приведенном ниже примере сетевая карта называется eth0. Это сетевая карта Intel.

   ```bash
            #command to set the port for adaptive RX/TX IRQ coalescing
            echtool -C eth0 adaptive-rx on
            echtool -C eth0 adaptive-tx on
            #confirm the setting using the command:
            ethtool -c eth0
   ```

   > [!NOTE]
   > Для предсказуемого поведения в высокопроизводительных средах, таких как среды для тестирования производительности, отключите адаптивное объединение запросов на прерывание RX/TX, а затем задайте его вручную. Для заданий значений воспользуйтесь примерами команд для отключения объединения запросов на прерывание RX/TX.

   ```bash
            #commands to disable adaptive RX/TX IRQ coalescing
            echtool -C eth0 adaptive-rx off
            echtool -C eth0 adaptive-tx off
            #confirm the setting using the command:
            ethtool -c eth0
            #Let us set the rx-usecs parameter which specify how many microseconds after at least 1 packet is received before generating an interrupt, and the [irq] parameters are the corresponding delays in updating the #status when the interrupt is disabled. For Intel bases NICs below are good values to start with:
            ethtool -C eth0 rx-usecs 100 tx-frames-irq 512
            #confirm the setting using the command:
            ethtool -c eth0
   ```

4. Кроме того, рекомендуется включить RSS (масштабирование на стороне приема) и по умолчанию объединить очереди RSS на стороне приема и передачи. В некоторых случаях обращений в службу поддержки Майкрософт отключение RSS также приводило к повышению производительности. Протестируйте этот параметр в тестовой среде перед его применением в рабочих средах. Ниже приведен пример команды для сетевых карт Intel.

   ```bash
            #command to get pre-set maximums
            ethtool -l eth0 
            #note the pre-set "Combined" maximum value. let's consider for this example, it is 8.
            #command to combine the queues with the value reported in the pre-set "Combined" maximum value:
            ethtool -L eth0 combined 8
            #you can verify the setting using the command below
            ethtool -l eth0
   ```

5. Сходство запросов на прерывание для портов сетевой карты Чтобы добиться ожидаемой производительности путем настройки сходства запросов на прерывание, можно воспользоваться рядом важных параметров, в том числе относящихся к серверной топологии в Linux и стеку драйверов сетевой карты, а также параметрами по умолчанию и параметром irqbalance. Оптимизация параметров сходства запросов на прерывание для портов сетевой карты выполняется с учетом топологии сервера, предусматривает отключение параметра irqbalance и использование параметров конкретного поставщика сетевой карты. Ниже приведен пример для сетевой инфраструктуры Mellanox. Обратите внимание, что команды зависят от среды. За дополнительными инструкциями обратитесь к поставщику сетевой карты.

   ```bash
            #disable irqbalance or get a snapshot of the IRQ settings and force the daemon to exit
            systemctl disable irqbalance.service
            #or
            irqbalance --oneshot

            #download the Mellanox mlnx_tuning_scripts tarball, https://www.mellanox.com/sites/default/files/downloads/tools/mlnx_tuning_scripts.tar.gz and extract it
            tar -xvf mlnx_tuning_scripts.tar.gz
            # be sure, common_irq_affinity.sh is executable. if not, 
            # chmod +x common_irq_affinity.sh       

            #display IRQ affinity for Mellanox NIC port; e.g eth0
            ./show_irq_affinity.sh eth0

            #optimize for best throughput performance
            ./mlnx_tune -p HIGH_THROUGHPUT

            #set hardware affinity to the NUMA node hosting physically the NIC and its port
            ./set_irq_affinity_bynode.sh `\cat /sys/class/net/eth0/device/numa_node` eth0

            #verify IRQ affinity
            ./show_irq_affinity.sh eth0

            #add IRQ coalescing optimizations
            ethtool -C eth0 adaptive-rx off
            ethtool -C eth0 adaptive-tx off
            ethtool -C eth0  rx-usecs 750 tx-frames-irq 2048

            #verify the settings
            ethtool -c eth0
   ```

6. После внесения указанных выше изменений проверьте скорость работы сетевой карты с помощью следующей команды:

   ```bash
            ethtool eth0 | grep -i Speed
   ```

#### <a name="additional-advanced-kernelos-configuration"></a>Дополнительная настройка ядра и ОС

- Для обеспечения максимальной производительности ввода-вывода хранилища в Linux рекомендуется использовать планирование на основе нескольких очередей для блочных устройств. Это позволяет эффективно масштабировать производительность на блочном уровне при использовании быстрых твердотельных накопителей (SSD) и многоядерных систем. Чтобы узнать, включена ли эта функция по умолчанию в вашем дистрибутиве Linux, обратитесь к документации. В большинстве случаев включить ее можно с помощью параметра **scsi_mod.use_blk_mq=y** при загрузке ядра, хотя в документации к дистрибутиву Linux могут быть дополнительные указания. Это согласуется с основным ядром Linux.

- Так как для развертываний SQL Server часто используется многомаршрутная подсистема ввода-вывода, многомаршрутный целевой объект модуля отображения устройств (DM) необходимо также настроить для использования инфраструктуры `blk-mq`. Для этого следует включить параметр загрузки ядра **dm_mod.use_blk_mq=y**. Значение по умолчанию — `n` (отключено). Когда базовые устройства SCSI используют `blk-mq`, этот параметр сокращает затраты на блокировку на уровне DM. Дополнительные рекомендации по настройке см. в документации по используемому дистрибутиву Linux.

#### <a name="configure-swapfile"></a>Настройка файла подкачки

Во избежание проблем с нехваткой памяти правильно настройте файл подкачки. Сведения о создании файла подкачки и его правильном размере см. в документации по вашей системе Linux.

#### <a name="virtual-machines-and-dynamic-memory"></a>Виртуальные машины и динамическая память

Если SQL Server на Linux выполняется в виртуальной машине, настройте фиксированный размер памяти, резервируемой для виртуальной машины. Не используйте такие функции, как динамическая память Hyper-V.

## <a name="sql-server-configuration"></a>Конфигурация SQL Server

Чтобы обеспечить максимальную производительность, рекомендуется выполнить описанные ниже задачи по настройке после установки SQL Server на Linux.

### <a name="best-practices"></a>Рекомендации

- **Использование PROCESS AFFINITY для узла и ЦП**

   Рекомендуется использовать инструкцию `ALTER SERVER CONFIGURATION`, чтобы задать `PROCESS AFFINITY` для всех узлов **NUMANODE** или ЦП, используемых для SQL Server в ОС Linux (то есть обычно для всех узлов и ЦП). Соответствие процессоров помогает эффективно планировать работу Linux и SQL. Использование параметра **NUMANODE** — простейший метод. **PROCESS AFFINITY** следует использовать, даже если на вашем компьютере всего один узел NUMA. Дополнительные сведения о задании **PROCESS AFFINITY** см. в статье об [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

- **Настройка нескольких файлов данных tempdb**

   Так как установленный экземпляр SQL Server на Linux не дает возможности настроить несколько файлов tempdb, мы рекомендуем создать несколько файлов данных tempdb после установки. Дополнительные сведения см. в статье [Рекомендации по сокращению состязания за выделяемые ресурсы в базе данных tempdb SQL Server](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d).

### <a name="advanced-configuration"></a>Расширенная настройка

Ниже приведены рекомендации по дополнительным параметрам конфигурации, которые можно настроить после установки SQL Server на Linux. Их выбор зависит от требований рабочей нагрузки и конфигурации ОС Linux.

- **Настройка предельного объема памяти с помощью mssql-conf**

   Чтобы обеспечить достаточный объем свободной физической памяти для ОС Linux, процесс SQL Server по умолчанию использует только 80 % физического ОЗУ. В некоторых системах с большим размером ОЗУ 20 % может быть очень много. Например, в системе с 1 ТБ ОЗУ при настройке по умолчанию неиспользуемыми остаются приблизительно 200 ГБ ОЗУ. В таком случае может быть желательно установить более высокий предел памяти. См. документацию по средству **mssql-conf** и параметру [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit), который определяет объем памяти, доступный SQL Server (в МБ).

   При изменении этого параметра будьте осторожны. Не задавайте слишком большое значение. Если памяти будет недостаточно, могут возникнуть проблемы в работе ОС Linux и других приложений Linux.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о функциях SQL Server, повышающих производительность, см. в статье о [начале работы с функциями производительности](sql-server-linux-performance-get-started.md).

Дополнительные сведения об SQL Server на Linux см. в статье [Обзор SQL Server на Linux](sql-server-linux-overview.md).
