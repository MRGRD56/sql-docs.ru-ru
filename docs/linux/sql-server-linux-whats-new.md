---
title: Новые возможности SQL Server 2017 на Linux
description: В этой статье описываются основные функции и службы, доступные для SQL Server 2017 на Linux.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c863cb129f1ba40c2bb73fff51adb859a8a7bade
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464696"
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Новые возможности SQL Server 2017 на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье описываются основные функции и службы, доступные для SQL Server 2017 на Linux.

> [!NOTE]
> Помимо описанных ниже возможностей, следует отметить регулярно выпускаемые накопительные обновления. Эти накопительные обновления содержат много усовершенствований и исправлений. Дополнительные сведения о последнем выпуске накопительного обновления см. здесь: [https://aka.ms/sql2017cu](https://aka.ms/sql2017cu). Дополнительные сведения и известные проблемы см. в статье с [заметками о выпуске](sql-server-linux-release-notes.md).

## <a name="ubuntu-1804-supported"></a>Поддержка Ubuntu 18.04

Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается Ubuntu 18.04. Ознакомьтесь с нашим кратким руководством по [установке SQL Server и созданию базы данных в Ubuntu](quickstart-install-connect-ubuntu.md).

## <a name="rhel-8-supported"></a>Поддержка RHEL 8

Начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20), поддерживается RHEL 8. Ознакомьтесь с нашим кратким руководством по [установке SQL Server и созданию базы данных в Red Hat](quickstart-install-connect-red-hat.md).

## <a name="sql-server-database-engine"></a>Компонент SQL Server Database Engine

- Включены основные возможности ядра СУБД SQL Server.
- Поддержка собственных путей Linux.
- Поддержка IPV6.
- Поддержка файлов баз данных на NFS.
- Поддержка шифрования [Transport Layer Security](sql-server-linux-encrypted-connections.md) (TLS).
- Поддерживается [проверка подлинности Azure Active Directory](sql-server-linux-active-directory-authentication.md).
- [Функции групп доступности](sql-server-linux-availability-group-overview.md) для обеспечения высокой доступности.
- [Поддержка полнотекстового поиска](sql-server-linux-setup-full-text-search.md).

## <a name="sql-server-agent"></a>Агент SQL Server

- Включена поддержка [агента SQL Server](sql-server-linux-setup-sql-agent.md) для следующих задач:
  - [Задания Transact-SQL](sql-server-linux-run-sql-server-agent-job.md)
  - [Почта базы данных](sql-server-linux-db-mail-sql-agent.md)
  - [Доставка журналов](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>Службы SQL Server Integration Services (SSIS)

- Возможность выполнять пакеты SSIS на Linux. Дополнительные сведения см. в разделе [Настройка SQL Server Integration Services в Linux с помощью ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="other-improvements"></a>Другие усовершенствования

- Средство настройки командной строки [mssql-conf](sql-server-linux-configure-mssql-conf.md).
- [Переменные среды](sql-server-linux-configure-environment-variables.md) для автоматической установки.
- Кроссплатформенное [расширение Visual Studio Code mssql-server](../tools/visual-studio-code/sql-server-develop-use-vscode.md).
- Кроссплатформенный генератор сценариев, [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md).
- Кроссплатформенный монитор динамического административного представления (DMV), [средство DBFS](https://github.com/Microsoft/dbfs).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы установить SQL Server на Linux, используйте один из следующих учебников:

- [Установка в Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Установка в SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Установка в Ubuntu](quickstart-install-connect-ubuntu.md)
- [Запуск в Docker](quickstart-install-connect-docker.md)
- [Подготовка виртуальной машины SQL в Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.yml). Дополнительные сведения об улучшениях, появившихся в SQL Server 2017, см. в разделе [Новые возможности SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
