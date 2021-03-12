---
title: Заметки о выпуске для SQL Server 2017 на Linux
description: Эта статья содержит заметки о выпуске и поддерживаемые функции для SQL Server 2017 на Linux. Приведены заметки о выпуске для последнего и нескольких предыдущих выпусков.
author: VanMSFT
ms.author: vanto
ms.date: 02/24/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: 1494c406ca4889e64e2016c36d48d5c7d54ee84a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837943"
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Заметки о выпуске для SQL Server 2017 на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Следующие заметки о выпуске применимы к [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], работающему в Linux. Эта статья разбита на разделы для каждого выпуска. В общедоступном (GA) выпуске содержатся подробные сведения о поддержке и известных проблемах. Каждый накопительный пакет обновления (CU) или выпуск для общего распространения (GDR) содержит ссылку на статью поддержки, где описаны изменения накопительного пакета обновления, а также ссылки на скачиваемые файлы пакета Linux.

> [!TIP]
> Эти заметки о выпуске предназначены специально для выпусков [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Дополнительные сведения о новом [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] см. в статье [Заметки о выпуске для предварительной версии SQL Server 2019 в Linux](sql-server-linux-release-notes-2019.md?view=sql-server-ver15&preserve-view=true).

## <a name="supported-platforms"></a>Поддерживаемые платформы

| Платформа | Файловая система | Руководство по установке |
|-----|-----|-----|
| Red Hat Enterprise Linux 7.3–7.8 или 8.0–8.3 Server | XFS или EXT4 | [Руководство по установке](quickstart-install-connect-red-hat.md) | 
| SUSE Enterprise Linux Server версии 12 с пакетом обновления 2–5 (SP2–SP5) | XFS или EXT4 | [Руководство по установке](quickstart-install-connect-suse.md) |
| Ubuntu 16.04 LTS, 18.04 LTS | XFS или EXT4 | [Руководство по установке](quickstart-install-connect-ubuntu.md) | 
| Подсистема Docker Engine 1.8+ на базе Windows, Mac или Linux | Недоступно | [Руководство по установке](quickstart-install-connect-docker.md) | 

> [!TIP]
> Дополнительные сведения см. в [требованиях к системе](sql-server-linux-setup.md#system) для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на базе Linux. Актуальную политику поддержки для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], см. в статье [Политика технической поддержки для Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="tools"></a>Инструменты

Большинство существующих клиентских средств, предназначенных для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], могут без проблем работать с [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], выполняющимся в Linux. Некоторые средства для нормальной работы с Linux могут иметь определенные требования к версиям. Полный список средств [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] см. в статье [Средства и служебные программы SQL для SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>История выпусков

В следующей таблице указана история выпусков для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].

| Release               | Версия       | Дата выпуска |
|-----------------------|---------------|--------------|
| [CU23](#CU23)         | 14.0.3381.3  | 24.02.2020   |
| [CU22-GDR](#CU22)         | 14.0.3370.1  | 12.01.2021 |
| [CU22](#CU22)         | 14.0.3356.20  | 10.09.2020   |
| [CU21](#CU21)         | 14.0.3335.7   | 01.07.2020   |
| [CU20](#CU20)         | 14.0.3294.2   | 10.04.2020   |
| [CU19](#CU19)         | 14.0.3281.6   | 2020-02-05   |
| [CU18](#CU18)         | 14.0.3257.3   | 2019-12-09   |
| [CU17](#CU17)         | 14.0.3238.1   | 2019-10-08   |
| [CU16](#CU16)         | 14.0.3223.3   | 01.08.2019   |
| [CU15](#CU15)         | 14.0.3162.1   | 2019-05-23   |
| [CU14](#CU14)         | 14.0.3076.1   | 2019-03-25   |
| [CU13](#CU13)         | 14.0.3048.4   | 2018-12-18   |
| [CU12](#CU12)         | 14.0.3045.24  | 2018-10-24   |
| [CU11](#CU11)         | 14.0.3038.14  | 2018-09-20   |
| [CU10](#CU10)         | 14.0.3037.1   | 2018-08-27   |
| [CU9-GDR2](#CU9-GDR2) | 14.0.3035.2   | 2018-08-18   |
| [GDR2](#GDR2)         | 14.0.2002.14  | 2018-08-18   |
| [CU9](#CU9)           | 14.0.3030.27  | 2018-07-18   |
| [CU8](#CU8)           | 14.0.3029.16  | 2018-06-21   |
| [CU7](#CU7)           | 14.0.3026.27  | 2018-05-24   |
| [CU6](#CU6)           | 14.0.3025.34  | 2018-04-19   |
| [CU5](#CU5)           | 14.0.3023.8   | 2018-03-20   |
| [CU4](#CU4)           | 14.0.3022.28  | 2018-02-20   |
| [CU3](#CU3)           | 14.0.3015.40  | 2018-01-03   |
| [GDR1](#GDR1)         | 14.0.2000.63  | 2018-01-03   |
| [CU2](#CU2)           | 14.0.3008.27  | 2017-11-28   |
| [CU1](#CU1)           | 14.0.3006.16  | 2017-10-24   |
| [GA](#GA)             | 14.0.1000.169 | 2017-10-02   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Установка обновлений

Если вы настроили репозиторий CU (**mssql-server-2017**), то получите последнюю версию накопительного пакета обновления для пакетов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] при выполнении новых установок. Этот репозиторий CU используется по умолчанию для всех статей установки пакетов для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux. Если вы настроили репозиторий GDR (**mssql-server-2017-gdr**), то получите только критические обновления для системы безопасности, выпущенные с момента выхода общедоступной версии. Если вам требуются обновления CU или GDR для контейнеров Docker, см. официальные образы для [Microsoft SQL Server на Linux для подсистемы Docker](https://hub.docker.com/r/microsoft/mssql-server). Дополнительные сведения о настройке репозиториев см. в статье [Настройка репозиториев для установки и обновления SQL Server на Linux.](sql-server-linux-change-repo.md)

При обновлении существующих пакетов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] выполните соответствующую команду обновления для каждого пакета, чтобы получить последний накопительный пакет обновления. Конкретные инструкции по обновлению для каждого пакета см. в следующих руководствах по установке.

- [Установка пакета SQL Server](sql-server-linux-setup.md#upgrade)
- [Установка пакета полнотекстового поиска](sql-server-linux-setup-full-text-search.md)
- [Установка служб SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Включение агента SQL Server](sql-server-linux-setup-sql-agent.md)

## <a name="cu23-february-2021"></a><a id="CU23"></a> CU23 (февраль 2021 г.)

Это выпуск накопительного пакета обновления 23 (CU23) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3381.3. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/5000685>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается **Ubuntu 18.04** и **RHEL 8**.
>
> Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04, за исключением пакета служб SSIS (который недоступен для Ubuntu 18.04). Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Ссылки для установки автономного пакета для Red Hat указывают на пакеты RHEL 8, за исключением пакета служб SSIS (который недоступен для RHEL 8). Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3381.3-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3381.3-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3381.3-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3381.3-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3381.3-2.x86_64.rpm) | 
| Пакет Debian Ubuntu 18.04 | 14.0.3381.3-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3381.3-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3381.3-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3381.3-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-gdr-january-2021"></a><a id="CU22-GDR"></a> CU22-GDR (январь 2021 г.)

Это выпуск накопительного пакета обновления 22-GDR (CU22-GDR) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3370.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается **Ubuntu 18.04** и **RHEL 8**.
>
> Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04, за исключением пакета служб SSIS (который недоступен для Ubuntu 18.04). Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Ссылки для установки автономного пакета для Red Hat указывают на пакеты RHEL 8, за исключением пакета служб SSIS (который недоступен для RHEL 8). Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3370.1-23-18 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| Пакет SLES RPM | 14.0.3370.1-18 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm) | 
| Пакет Debian Ubuntu 18.04 | 14.0.3370.1-18 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3370.1-18_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3370.1-18_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3370.1-18_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-september-2020"></a><a id="CU22"></a> CU22 (сентябрь 2020 г.)

Это выпуск накопительного пакета обновления 22 (CU22) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3356.20. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4577467>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается **Ubuntu 18.04** и **RHEL 8**.
>
> Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04, за исключением пакета служб SSIS (который недоступен для Ubuntu 18.04). Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Ссылки для установки автономного пакета для Red Hat указывают на пакеты RHEL 8, за исключением пакета служб SSIS (который недоступен для RHEL 8). Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3356.20-23 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3356.20-23 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm) | 
| Пакет Debian Ubuntu 18.04 | 14.0.3356.20-23 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3356.20-23_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3356.20-23_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3356.20-23_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu21-july-2020"></a><a id="CU21"></a> CU21 (Июль 2020)

Это выпуск накопительного пакета обновления 21 (CU21) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3335.7. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4557397>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается **Ubuntu 18.04** и **RHEL 8**.
>
> Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04, за исключением пакета служб SSIS (который недоступен для Ubuntu 18.04). Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Ссылки для установки автономного пакета для Red Hat указывают на пакеты RHEL 8, за исключением пакета служб SSIS (который недоступен для RHEL 8). Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3335.7-17 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3335.7-17 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm) | 
| Пакет Debian Ubuntu 18.04 | 14.0.3335.7-17 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3335.7-17_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3335.7-17_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3335.7-17_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu20-april-2020"></a><a id="CU20"></a> CU20 (апрель 2020)

Это выпуск накопительного пакета обновления 20 (CU20) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3294.2. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу <https://support.microsoft.com/help/4541283>.

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается **Ubuntu 18.04** и **RHEL 8**.
>
> Ссылки для установки автономного пакета для Ubuntu указывают на пакеты Ubuntu 18.04, за исключением пакета служб SSIS (который недоступен для Ubuntu 18.04). Если вы ищете пакеты Ubuntu 16.04, см. путь скачивания <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>.
>
> Ссылки для установки автономного пакета для Red Hat указывают на пакеты RHEL 8, за исключением пакета служб SSIS (который недоступен для RHEL 8). Если вы ищете пакеты RHEL 7, см. путь скачивания <https://packages.microsoft.com/rhel/7/mssql-server-2017/>.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3294.2-27 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3294.2-27 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm) | 
| Пакет Debian Ubuntu 18.04 | 14.0.3294.2-27 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3294.2-27_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3294.2-27_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3294.2-27_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu19-february-2020"></a><a id="CU19"></a> CU19 (февраль 2020 г.)

Это выпуск накопительного пакета обновления 19 (CU19) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3281.6. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4535007](https://support.microsoft.com/help/4535007).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3281.6-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3281.6-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3281.6-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3281.6-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3281.6-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3281.6-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu18-december-2019"></a><a id="CU18"></a> CU18 (декабрь 2019 г.)

Это выпуск накопительного пакета обновления 18 (CU18) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3257.3. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4527377](https://support.microsoft.com/help/4527377).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3257.3-13 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3257.3-13 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3257.3-13 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3257.3-13_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3257.3-13_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3257.3-13_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

### <a name="added-support"></a>Добавлена поддержка

- Система отслеживания измененных данных (CDC) поддерживается с SQL Server 2017 в Linux, начиная с CU18.
- Репликация транзакций поддерживается с SQL Server 2017 в Linux, начиная с CU18.

### <a name="remarks"></a>Remarks

Контейнеры SQL Server 2017 теперь имеют новый шаблон тегов, как описано ниже с примерами.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-<update>-<Linux Distribution>-<Linux Distribution Version>`

  Извлекает образ контейнера с помощью сочетания, описанного в теге.

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-latest`

    Извлекает последнюю версию SQL Server в последней поддерживаемой версии Ubuntu.

**Примеры:**

`mcr.microsoft.com/mssql/server:2017-CU18-ubuntu-16.04`

Извлекает SQL Server 2017 CU18 на основе контейнера Ubuntu 16.04.

`mcr.microsoft.com/mssql/server:2017-latest`

Извлекает последнюю версию SQL Server 2017 (CU18 на момент написания статьи) на основе контейнера Ubuntu 16.04.

> [!NOTE]
> Мы больше не будем публиковать контейнеры с другими шаблонами тегов для контейнеров SQL Server 2017 в будущем.


## <a name="cu17-october-2019"></a><a id="CU17"></a> CU17 (октябрь 2019 г.)

Это выпуск накопительного пакета обновления 17 (CU17) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3238.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4515579](https://support.microsoft.com/help/4515579).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3238.1-19 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3238.1-19 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3238.1-19 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3238.1-19_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3238.1-19_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3238.1-19_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu16-august-2019"></a><a id="CU16"></a> CU16 (август 2019 г.)

Это выпуск накопительного пакета обновления 16 (CU16) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3223.3. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4508218](https://support.microsoft.com/help/4508218).

### <a name="whats-new"></a>What's New

|Новые функции или обновления | Сведения |
|:---|:---|
| Поддержка координатора распределенных транзакций | Поддержка координатора распределенных транзакций Майкрософт (MSDTC) для SQL Server 2017. Дополнительные сведения см. в статье [Сведения о настройке координатора распределенных транзакций (Майкрософт) (MSDTC) в Linux](sql-server-linux-configure-msdtc.md). |

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3223.3-15 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3223.3-15 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3223.3-15 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3223.3-15_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3223.3-15_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3223.3-15_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu15-may-2019"></a><a id="CU15"></a> CU15 (май 2019 г.)

Это выпуск накопительного пакета обновления 15 (CU15) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3162.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4498951](https://support.microsoft.com/help/4498951).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3162.1-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3162.1-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3162.1-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3162.1-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3162.1-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3162.1-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu14-mar-2019"></a><a id="CU14"></a> CU14 (март 2019 г.)

Это выпуск накопительного пакета обновления 14 (CU14) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3076.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4484710](https://support.microsoft.com/help/4484710).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3076.1-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3076.1-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3076.1-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3076.1-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3076.1-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3076.1-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu13-dec-2018"></a><a id="CU13"></a> CU13 (декабрь 2018 г.)

Это выпуск накопительного пакета обновления 13 (CU13) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3048.4. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4466404](https://support.microsoft.com/help/4466404).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3048.4-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3048.4-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3048.4-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3048.4-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3048.4-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3048.4-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu12-oct-2018"></a><a id="CU12"></a> CU12 (октябрь 2018 г.)

Это выпуск накопительного пакета обновления 12 (CU12) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3045.24. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4464082](https://support.microsoft.com/help/4464082).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3045.24-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3045.24-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3045.24-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3045.24-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3045.24-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3045.24-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu11-sept-2018"></a><a id="CU11"></a> CU11 (сентябрь 2018 г.)

Это выпуск накопительного пакета обновления 11 (CU11) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3038.14. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4462262](https://support.microsoft.com/help/4462262).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3038.14-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3038.14-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3038.14-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3038.14-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3038.14-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3038.14-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu10-aug-2018"></a><a id="CU10"></a> CU10 (август 2018 г.)

Это выпуск накопительного пакета обновления 10 (CU10) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3037.1. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4342123](https://support.microsoft.com/help/4342123).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3037.1-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3037.1-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3037.1-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3037.1-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3037.1-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3037.1-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu9-gdr2-aug-2018"></a><a id="CU9-GDR2"></a> CU9-GDR2 (август 2018 г.)

Это обновление для системы безопасности, которое также включает ранее выпущенный накопительный пакет обновления (CU9) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3035.2. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4293805](https://support.microsoft.com/help/4293805).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3035.2-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm)| 
| Пакет SLES RPM | 14.0.3035.2-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3035.2-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3035.2-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3035.2-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3035.2-1_amd64.deb)<br/> |

## <a name="gdr2-aug-2018"></a><a id="GDR2"></a> GDR2 (август 2018 г.)

Это обновление для системы безопасности, которое включает только исправления безопасности GDR2 (и GDR1) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)].  Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.2002.14. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4293803](https://support.microsoft.com/help/4293803).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.2002.14-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.2002.14-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.2002.14-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2002.14-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2002.14-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2002.14-1_amd64.deb) |

## <a name="cu9-jul-2018"></a><a id="CU9"></a> CU9 (июль 2018 г.)

Это выпуск накопительного пакета обновления 9 (CU9) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3030.27. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4341265](https://support.microsoft.com/help/4341265).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3030.27-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3030.27-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3030.27-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3030.27-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3030.27-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3030.27-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu8-jun-2018"></a><a id="CU8"></a> CU8 (июнь 2018 г.)

Это выпуск накопительного пакета обновления 8 (CU8) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3029.16. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4338363](https://support.microsoft.com/help/4338363).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3029.16-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3029.16-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3029.16-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3029.16-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3029.16-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu7-may-2018"></a><a id="CU7"></a> CU7 (май 2018 г.)

Это выпуск накопительного пакета обновления 7 (CU7) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3026.27. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4229789](https://support.microsoft.com/help/4229789).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3026.27-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3026.27-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3026.27-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu6-apr-2018"></a><a id="CU6"></a> CU6 (апрель 2018 г.)

Это выпуск накопительного пакета обновления 6 (CU6) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3025.34. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4101464](https://support.microsoft.com/help/4101464).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3025.34-3 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3025.34-3 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3025.34-3 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu5-mar-2018"></a><a id="CU5"></a> CU5 (март 2018 г.)

Это выпуск накопительного пакета обновления 5 (CU5) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3023.8. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4092643](https://support.microsoft.com/help/4092643).

### <a name="known-upgrade-issue"></a>Известные проблемы при обновлении

При обновлении предыдущего выпуска до накопительного пакета обновления CU5 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] может не запуститься со следующей ошибкой.

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

Чтобы устранить эту ошибку, включите агент SQL Server и перезапустите [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с помощью следующих команд.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3023.8-5 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3023.8-5 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3023.8-5 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu4-feb-2018"></a><a id="CU4"></a> CU4 (февраль 2018 г.)

Это выпуск накопительного пакета обновления 4 (CU4) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3022.28. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4056498](https://support.microsoft.com/help/4056498).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

> [!NOTE]
> Начиная с накопительного пакета обновления CU4, агент SQL Server больше не устанавливается в виде отдельного пакета. Он устанавливается вместе с пакетом подсистемы и должен быть включен для использования.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3022.28-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3022.28-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3022.28-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="gdr1-jan-2018"></a><a id="GDR1"></a> GDR1 (январь 2018 г.)

Это обновление для системы безопасности, которое включает только исправления безопасности GDR1 для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.2000.63. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4057122](https://support.microsoft.com/help/4057122).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.2000.63-3 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.2000.63-3 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.2000.63-3 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2000.63-3_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2000.63-3_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2000.63-3_amd64.deb) |

## <a name="cu3-jan-2018"></a><a id="CU3"></a> CU3 (январь 2018 г.)

Это выпуск накопительного пакета обновления 3 (CU3) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3015.40. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4052987](https://support.microsoft.com/help/4052987).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3015.40-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3015.40-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3015.40-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[Пакет Debian агента SQL Server](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu2-nov-2017"></a><a id="CU2"></a> CU2 (ноябрь 2017 г.)

Это выпуск накопительного пакета обновления 2 (CU2) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3008.27. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/4052574](https://support.microsoft.com/help/4052574).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3008.27-1 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3008.27-1 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3008.27-1 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[Пакет Debian агента SQL Server](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu1-oct-2017"></a><a id="CU1"></a> CU1 (октябрь 2017 г.)

Это выпуск накопительного пакета обновления 1 (CU1) для [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.3006.16. Сведения об исправлениях и улучшениях в этом выпуске см. по адресу [https://support.microsoft.com/help/KB4053439](https://support.microsoft.com/help/4038634).

### <a name="package-details"></a>Сведения о пакете

Для ручной или автономной установки пакета можно скачать пакеты RPM и Debian, используя сведения из следующей таблицы.

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.3006.16-3 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.3006.16-3 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.3006.16-3 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[Пакет Debian агента SQL Server](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="ga-oct-2017"></a><a id="GA"></a> GA (октябрь 2017 г.)

Это выпуск общедоступной версии (GA) [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Версия [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] для этого выпуска — 14.0.1000.169.

### <a name="package-details"></a>Сведения о пакете

Сведения о пакете и расположениях для скачивания для пакетов RPM и Debian приведены в приведенной ниже таблице. Эти пакеты не требуется скачивать напрямую, если вы следуете инструкциям, описанным в следующих руководствах по установке:

- [Установка пакета SQL Server](sql-server-linux-setup.md)
- [Установка пакета полнотекстового поиска](sql-server-linux-setup-full-text-search.md)
- [Установка пакета агента SQL Server](sql-server-linux-setup-sql-agent.md)
- [Установка служб SQL Server Integration Services](sql-server-linux-setup-ssis.md)

| Пакет | Версия пакета | Файлы для загрузки |
|-----|-----|-----|
| Пакет Red Hat RPM | 14.0.1000.169-2 | [Пакет RPM подсистемы](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[Пакет SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Пакет SLES RPM | 14.0.1000.169-2 | [Пакет RPM подсистемы mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM высокой доступности](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM полнотекстового поиска](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Пакет RPM агента SQL Server](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Пакет Ubuntu 16.04 Debian | 14.0.1000.169-2 | [Пакет подсистемы Debian](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[Пакет Debian высокой доступности](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[Пакет Debian полнотекстового поиска](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[Пакет Debian агента SQL Server](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[Пакет SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="known-issues"></a>Известные проблемы

В следующих разделах описаны известные проблемы с общедоступным (GA) выпуском [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] в Linux.

#### <a name="general"></a>Общие сведения

- Длина имени узла, где устанавливается [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], не должна превышать 15 символов. 

    - **Решение**. Измените имя в /etc/hostname, чтобы оно содержало не более 15 символов.

- Ручной перевод системного времени назад приведет к тому, что [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] прекратит обновлять внутреннее системное время в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Решение**. Перезапустите [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Поддерживаются только установки с одним экземпляром.

    - **Решение**. Если на заданном узле требуется несколько экземпляров, рекомендуется использовать виртуальные машины или контейнеры Docker. 

- Диспетчеру конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не удается подключиться к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux.

- По умолчанию для имени входа **sa** используется английский язык.

    - **Решение**. Измените язык для имени входа **sa** с помощью инструкции **ALTER LOGIN**.

#### <a name="databases"></a>Базы данных

- Базу данных master нельзя переместить с помощью служебной программы mssql-conf. Другие базы данных с помощью mssql-conf переместить можно.

- При восстановлении базы данных, для которой была создана резервная копия в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на базе Windows, нужно использовать предложение **WITH MOVE** в инструкции Transact-SQL.

- Некоторые алгоритмы (наборы шифров) для протокола TLS не работают должным образом с [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на Linux. Это приводит к сбоям подключения при попытке подключиться к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], а также к проблемам при установке соединений между репликами в группах с высокой доступностью.

   - **Решение**. Измените скрипт конфигурации **mssql.conf** для [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux, чтобы отключить проблемные комплекты шифров, выполнив следующие действия.

      1. Добавьте следующий код в /var/opt/mssql/mssql.conf.

         ```
         [network]
         tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
         ```

         > [!NOTE]
         > В приведенном выше коде `!` выполняет отрицание для выражения. Это означает, что OpenSSL не будет использовать следующий комплект шифров.  

      1. Перезапустите [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с помощью следующей команды.

         ```bash
         sudo systemctl restart mssql-server
         ```

- Базы данных [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] в Windows, использующие встроенный OLTP, не могут быть восстановлены в [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] на Linux. Чтобы восстановить базу данных [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], использующую выполняющуюся в памяти OLTP, сначала обновите базы до [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] или [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] в Windows, прежде чем перемещать их в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на базе Linux с помощью операции резервного копирования/восстановления или подключения/отключения.

- Сейчас разрешение пользователя **ADMINISTER BULK OPERATIONS** в Linux не поддерживается.

#### <a name="networking"></a>Сеть

Функции, затрагивающие исходящие подключения TCP из процесса sqlservr, такие как связанные серверы или группы доступности, могут не работать, если выполняются оба следующих условия.

1. Целевой сервер указан в виде имени узла, а не IP-адреса.

1. В ядре для экземпляра источника отключен протокол IPv6. Чтобы проверить, включена ли в ядре поддержка протокола IPv6, необходимо успешно выполнить все следующие проверки.

   - `cat /proc/cmdline` выводит на печать командную строку загрузки текущего ядра. Выходные данные не должны содержать `ipv6.disable=1`.
   - Каталог /proc/sys/net/ipv6/ должен существовать.
   - Программа на языке C, которая вызывает `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)`, должна выполняться успешно — системный вызов должен возвращать fd != -1, а не завершиться с ошибкой EAFNOSUPPORT.

Конкретная ошибка зависит от функции. Для связанных серверов это проявляется в виде ошибки времени ожидания при входе. Для групп доступности команда DDL `ALTER AVAILABILITY GROUP JOIN` на вторичной реплике завершится сбоем через 5 минут с ошибкой времени ожидания конфигурации скачивания.

Для обхода этой проблемы выполните одно из следующих действий.

1. Используйте IP-адреса вместо имен узлов, чтобы указать целевой объект подключения TCP.

1. Включите протокол IPv6 в ядре, удалив `ipv6.disable=1` из командной строки загрузки. Используемая для этого процедура зависит от дистрибутива Linux и загрузчика, такого как grub. Если вы хотите отключить протокол IPv6, это можно сделать, задав `net.ipv6.conf.all.disable_ipv6 = 1` в конфигурации `sysctl` (например, `/etc/sysctl.conf`). Это по-прежнему помешает сетевому адаптеру системы получить IPv6-адрес, но обеспечит работу функций sqlservr.

#### <a name="network-file-system-nfs"></a>Файловая система NFS
При использовании удаленных общих папок **NFS** в рабочей среде необходимо обратить внимание на следующие требования к поддержке.

- Версия NFS должна быть **4.2 или более поздняя**. Более старые версии NFS не поддерживают необходимые функции, такие как использование команды fallocate и создание разреженных файлов, общие для современных файловых систем.
- При подключении NFS следует указать только каталоги **/var/opt/mssql**. Другие файлы, например системные двоичные файлы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], не поддерживаются.
- При подключении удаленной общей папки клиенты NFS должны использовать параметр nolock.

#### <a name="localization"></a>Локализация

- Если ваш языковой стандарт отличается от английского (en_us) во время установки, в сеансе или терминале bash нужно использовать кодировку UTF-8. Если использовать кодировку ASCII, появится примерно следующее сообщение об ошибке.

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Если вы не можете использовать кодировку UTF-8, запустите программу установки с помощью переменной среды MSSQL_LCID, чтобы указать требуемый язык.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- При выполнении программы установки mssql-conf и выполнении установки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на языке, отличном от английского, после локализованного текста "Настройка SQL Server..." отображаются неверные символы национального алфавита. А для установок на языках, отличных от романских, это предложение может отсутствовать полностью. В отсутствующем предложении должна отображаться следующая локализованная строка: "Идентификатор процесса лицензирования обработан. Новый выпуск: [выпуск \<Name\>]". Эта строка выводится только для информационных целей, а в следующем накопительном пакете обновления [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] эта проблема будет устранена для всех языков. Это никак не влияет на успешное выполнение установки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

#### <a name="full-text-search"></a>Компонент Full-text Search

- В этом выпуске доступны не все фильтры, включая фильтры для документов Office. Список поддерживаемых фильтров см. в статье [Установка полнотекстового поиска SQL Server в Linux](sql-server-linux-setup-full-text-search.md#filters).

#### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> Службы SQL Server Integration Services

- В этом выпуске пакет **mssql-server-is** не поддерживается в SUSE. Сейчас он поддерживается в Ubuntu и Red Hat Enterprise Linux (RHEL).

- С помощью [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] в Linux CTP 2.1 и более поздних версиях пакеты [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] могут использовать подключения ODBC в Linux. Эта функция была протестирована с использованием [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и драйверов ODBC для MySQL, но она также должна работать с любым драйвером ODBC для Юникода, который поддерживает спецификацию ODBC. Во время разработки можно указать либо имя DSN, либо строку подключения для подключения к данным ODBC. Кроме того, можно использовать проверку подлинности Windows. Дополнительные сведения см. в [записи блога с объявлением поддержки ODBC в Linux.](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

- Следующие функции не поддерживаются в этом выпуске при запуске пакетов служб SSIS в Linux:
  - База данных каталогов [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Выполнение пакета агентом SQL по расписанию
  - Проверка подлинности Windows
  - Сторонние компоненты
  - Система отслеживания измененных данных (CDC)
  - Горизонтальное увеличение масштаба [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]
  - Пакет дополнительных компонентов Azure для SSIS
  - Поддержка Hadoop и HDFS
  - Соединитель Microsoft Connector для SAP BW

Список встроенных компонентов SSIS, которые сейчас не поддерживаются или поддерживаются с ограничениями, см. в статье [Ограничения и известные проблемы для служб SSIS в Linux](sql-server-linux-ssis-known-issues.md#components).

Дополнительные сведения о службах SSIS в Linux см. в следующих статьях.
-   [Запись блога с объявлением о поддержке SSIS для Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [Установка служб SQL Server Integration Services (SSIS) в Linux](sql-server-linux-setup-ssis.md)
-   [Извлечение, преобразование и загрузка данных в Linux с помощью служб SSIS](sql-server-linux-migrate-ssis.md)

#### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

На [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] в Windows, подключенный к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в Linux, распространяются следующие ограничения.

- Планы обслуживания не поддерживаются.

- Хранилище данных управления (MDW) и сборщик данных в [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] не поддерживаются. 

- Компоненты пользовательского интерфейса [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], использующие параметры проверки подлинности Windows или журнала событий Windows, не работают с Linux. Эти функции по-прежнему можно использовать с другими параметрами, такими как имена входа SQL. 

- Число сохраняемых файлов журнала не подлежит изменению.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы приступить к работе, ознакомьтесь со следующими краткими руководствами.

- [Установка в Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Установка в SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Установка в Ubuntu](quickstart-install-connect-ubuntu.md)
- [Запуск в Docker](quickstart-install-connect-docker.md)
- [Подготовка виртуальной машины SQL в Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Запуск и подключение — облако](quickstart-install-connect-clouds.md)

Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.md).
