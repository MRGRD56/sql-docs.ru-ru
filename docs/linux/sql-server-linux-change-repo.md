---
title: Настройка репозиториев Linux для SQL Server 2017 и 2019
description: Проверьте и настройте репозитории исходного кода для SQL Server 2019 и SQL Server 2017 в Linux. Репозиторий исходного кода влияет на версию SQL Server, используемую во время установки и обновления.
author: VanMSFT
ms.author: vanto
ms.date: 04/28/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
zone_pivot_groups: ld2-linux-distribution
ms.openlocfilehash: d32bd886cdea6d64cee893ca76d83c4a5f74a7a2
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003890"
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Настройка репозиториев для установки и обновления SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: zone pivot="ld2-rhel"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **Red Hat (RHEL)** .
::: zone-end

::: zone pivot="ld2-sles"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **SUSE (SLES)** .
::: zone-end

::: zone pivot="ld2-ubuntu"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **Ubuntu**.
::: zone-end

> [!TIP]
> SQL Server 2019 уже доступен! Чтобы опробовать его, настройте новый репозиторий **mssql-server-2019**, как описано в этой статье. После этого выполните инструкции в [руководстве по установке](sql-server-linux-setup.md).

## <a name="repositories"></a><a id="repositories"></a> Репозитории

При установке SQL Server на Linux необходимо настроить репозиторий Майкрософт. Он используется для получения пакета ядра СУБД (**mssql-server**) и связанных с ним пакетов SQL Server. В настоящее время существует пять основных репозиториев:

| Хранилище | Имя | Описание |
|---|---|---|
| **2019** | **mssql-server-2019** | Репозиторий для SQL Server 2019 с накопительным пакетом обновления. |
| **2019 GDR** | **mssql-server-2019-gdr** | Репозиторий выпуска SQL Server 2019 для общего распространения, предназначенный только для критических обновлений. |
| **2019, предварительная версия** | **mssql-server-preview** | Репозиторий для предварительной версии и версии релиз-кандидата SQL Server 2019. |
| **2017** | **mssql-server-2017** | Репозиторий для SQL Server 2017 с накопительным пакетом обновления. |
| **2017 GDR** | **mssql-server-2017-gdr** | Репозиторий выпуска SQL Server 2017 для общего распространения, предназначенный только для критических обновлений. |

## <a name="cumulative-update-versus-gdr"></a><a id="cuversusgdr"></a> Накопительный пакет обновления и GDR

Важно отметить, что для каждого дистрибутива имеются два основных типа репозиториев.

- **Накопительный пакет обновления**. Репозиторий накопительного пакета обновления содержит пакеты для основного выпуска SQL Server, а также все исправления ошибок и улучшения, добавленные с момента этого выпуска. Накопительные пакеты обновления связаны с определенной версий, например SQL Server 2019. Они выпускаются регулярно.

- **Выпуск для общего распространения (GDR)** . Репозиторий GDR содержит пакеты для основного выпуска SQL Server и только критические исправления и обновления для системы безопасности, добавленные с момента этого выпуска. Эти обновления также добавляются в следующий накопительный пакет обновления.

Каждый выпуск накопительного пакета обновления и GDR содержит полный пакет SQL Server и все предыдущие обновления для этого репозитория. Выполнить обновление с выпуска GDR на выпуск накопительного пакета обновления можно путем изменения настроенного репозитория для SQL Server. Можно также [перейти на использование любого более раннего выпуска](sql-server-linux-setup.md#rollback) в рамках основной версии (например, 2017).

> [!NOTE]
> Выполнить обновление с выпуска GDR на выпуск накопительного пакета обновления можно в любой момент путем изменения репозитория. Обновление с выпуска накопительного пакета обновления на выпуск GDR не поддерживается.

## <a name="configure-repositories"></a>Настройка репозиториев

::: zone pivot="ld2-rhel"
Чтобы настроить репозитории в Red Hat Enterprise Server (RHEL), выполните инструкции, приведенные в следующих разделах.
::: zone-end

::: zone pivot="ld2-sles"
Чтобы настроить репозитории в SUSE Linux Enterprise Server (SLES), выполните инструкции, приведенные в следующих разделах.
::: zone-end

::: zone pivot="ld2-ubuntu"
Чтобы настроить репозитории в Ubuntu, выполните инструкции, приведенные в следующих разделах.
::: zone-end

## <a name="check-for-previously-configured-repositories"></a>Проверка ранее настроенных репозиториев

<!--RHEL-->
::: zone pivot="ld2-rhel"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Просмотрите файлы в каталоге **/etc/yum.repos.d** с помощью следующей команды:

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Найдите файл, который служит для настройки каталога SQL Server, например **mssql-server.repo**.

3. Выведите содержимое файла.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. Настроенный репозиторий указан в свойстве **name**. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Используйте команду **zypper info**, чтобы получить сведения о ранее настроенном репозитории.

   ```bash
   sudo zypper info mssql-server
   ```

2. Настроенный репозиторий указан в свойстве **Repository**. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Просмотрите содержимое файла **/etc/apt/sources.list**.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Найдите URL-адрес пакета mssql-server. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

## <a name="remove-old-repository"></a>Удаление старого репозитория

<!--RHEL-->
::: zone pivot="ld2-rhel"
При необходимости удалите старый репозиторий с помощью приведенной ниже команды.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Предполагается, что в предыдущем разделе был определен файл **mssql-server.repo**.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
При необходимости удалите старый репозиторий. В зависимости от типа ранее настроенного репозитория выполните одну из приведенных ниже команд.

| Хранилище | Команда для удаления |
|---|---|
| **Предварительная версия (2019)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-preview'` |
| **2019 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019'` |
| **2019 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019-gdr'`|
| **2017 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **2017 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
При необходимости удалите старый репозиторий. В зависимости от типа ранее настроенного репозитория выполните одну из приведенных ниже команд.

> [!NOTE]
>
> - Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU10), поддерживается Ubuntu 20.04.
> - Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3) и SQL Server 2017 с накопительным пакетом обновления 20 (CU20), теперь поддерживается Ubuntu 18.04.
> - Если вы используете Ubuntu 16,04, измените приведенный ниже путь на `/ubuntu/16.04` вместо `/ubuntu/18.04` и используйте правильное [имя кода распространения](https://releases.ubuntu.com/).

| Хранилище | Команда для удаления |
|---|---|
| **Предварительная версия (2019)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview xenial main'` |
| **2019 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019 bionic main'` | 
| **2019 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019-gdr bionic main'` |
| **2017 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **2017 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

::: zone-end

## <a name="configure-new-repository"></a>Настройка нового репозитория

<!--RHEL-->
::: zone pivot="ld2-rhel"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

> [!NOTE]
> Следующие команды для SQL Server 2019 ссылаются на репозиторий RHEL 8. RHEL 8 не входит в состав установки python2, которая требуется для SQL Server. Дополнительные сведения см. в статье по установке компонента python2 и настройке его как интерпретатора по умолчанию в следующем блоге: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>
> Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается RHEL 8.
>
> Если вы используете RHEL 7 или RHEL 8, убедитесь, что пути соответствуют `/rhel/7` или `/rhel/8`. Наши пакеты не зависят от дополнительных версий RHEL. Это означает, что, если вы используете RHEL 7.6, для настройки репозитория необходим путь `/rhel/7`.

| Хранилище | Версия | Get-Help |
|---|---|---|
| **2019 CU** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017-gdr.repo` |

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

| Хранилище | Версия | Get-Help |
|---|---|---|
| **2019 CU** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server.

> [!NOTE]
>
> - Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU10), теперь поддерживается Ubuntu 20.04.
> - Начиная с SQL Server 2019 с накопительным пакетом обновления 3 (CU3) и SQL Server 2017 с накопительным пакетом обновления 20 (CU20), теперь поддерживается Ubuntu 18.04.
> - Следующие команды ссылаются на репозиторий Ubuntu 18.04. Если вы используете Ubuntu 16.04, в приведенном ниже пути замените `/ubuntu/18.04` на `/ubuntu/16.04`.

1. Импортируйте открытые ключи GPG из репозитория.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

   | Хранилище | Версия | Get-Help |
   |---|---|---|
   | **2019 CU** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"` |
   | **2019 GDR** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019-gdr.list)"` |
   | **2017 CU** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"` |
   | **2017 GDR** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017-gdr.list)"` |

3. Выполните команду **apt-get update**.

   ```bash
   sudo apt-get update
   ```

::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

Настроив нужный репозиторий, можно перейти к [установке](sql-server-linux-setup.md#platforms) или [обновлению](sql-server-linux-setup.md#upgrade) SQL Server и всех связанных пакетов из него.

::: zone pivot="ld2-rhel"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для RHEL](quickstart-install-connect-red-hat.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

::: zone pivot="ld2-sles"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для SLES](quickstart-install-connect-suse.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

::: zone pivot="ld2-ubuntu"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для Ubuntu](quickstart-install-connect-ubuntu.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

Дополнительные сведения об установке SQL Server 2017 на Linux см. в статье [Руководство по установке SQL Server на Linux](sql-server-linux-setup.md).
