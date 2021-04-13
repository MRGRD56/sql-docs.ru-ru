---
title: SQL Server с поддержкой Azure Arc — заметки о выпуске
description: Последние заметки о выпуске
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 96239b337ed917b74effc58ac4e55b67b568a9e4
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557537"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>Заметки о выпуске — SQL Server с поддержкой Azure Arc (предварительная версия)

> [!NOTE]
> В отношении технологии (как предварительной версии функции), описанной в этой статье, действуют [дополнительные условия использования предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="april-2021"></a>Апрель 2021 г.

### <a name="breaking-change"></a>Критическое изменение

Критических изменений нет.

### <a name="other-changes"></a>Другие изменения

В тип ресурса **SQL Server - Azure Arc** добавлено новое свойство *LicenseType*. Оно указывает, требуется ли лицензия для экземпляра SQL Server. Это свойство может принимать одно из следующих значений:

| **Значение** | **Описание** |
|:--|:--|
|Платный|Указывает, что экземпляр SQL использует выпуск Enterprise, Standard или Web сервера SQL Server.|
|Бесплатный|Указывает, что экземпляр SQL использует выпуск Express или Developer сервера SQL Server.|
|HADR|Указывает, что экземпляр SQL является репликой в группе доступности. Если действует программа Software Assurance, то, возможно, лицензия не потребуется. Дополнительные сведения см. в [Условиях коммерческого лицензирования SQL Server](https://www.microsoft.com/licensing/terms/productoffering/SQLServer/EAEAS).

> [!NOTE]
> Для существующих ресурсов **SQL Server - Azure Arc** это свойство будет иметь значение *NULL*. Значение автоматически поменяется на правильное после того, как служба SQL Server с поддержкой Azure Arc станет общедоступной.

## <a name="december-2020"></a>Декабрь 2020 г.

### <a name="breaking-change"></a>Критическое изменение

В этом выпуске появился обновленный [поставщик ресурсов](/azure/azure-resource-manager/management/azure-services-resource-providers) с названием `Microsoft.AzureArcData`. Прежде чем продолжить использование SQL Server с поддержкой Azure Arc, необходимо зарегистрировать этот поставщик ресурсов. Инструкции по регистрации поставщика ресурсов приводятся в разделе [Предварительные требования](connect.md#prerequisites).

Если у вас уже есть существующие ресурсы "SQL Server — Azure Arc", выполните следующие действия, чтобы перенести их в пространство имен Microsoft.AzureArcData.

1. Запустите [Cloud Shell](https://shell.azure.com/). См. [дополнительные сведения о PowerShell в Cloud Shell](/azure/cloud-shell/quickstart-powershell).

2. Отправьте скрипт в оболочку с помощью следующей команды:

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. Выполните скрипт.  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - Чтобы вставить команды в оболочку, используйте клавиши `Ctrl-Shift-V` в Windows или `Cmd-v` в MacOS.
> - Команда `curl` скопирует скрипт непосредственно в домашнюю папку, связанную с вашим сеансом Cloud Shell.
> - Скрипт запрашивает имя группы ресурсов и выводит соответствующее сообщение после завершения миграции.

### <a name="other-changes"></a>Другие изменения

* Свойство *TCPPorts* для типа ресурса **SQL Server — Azure Arc** переименовано в *TCPStaticPorts*
* Ограничены необходимые разрешения. Дополнительные сведения см. в разделе [Требуемое разрешение](overview.md#required-permissions).

### <a name="known-issues"></a>Известные проблемы

* Свойство *CreateTime* не будет добавляться к вновь созданным ресурсам в пространстве имен AzureArcData, включая ресурсы **SQL Server — Azure Arc**.

## <a name="october-2020"></a>Октябрь 2020 г.

Обновление за октябрь содержит следующие улучшения.

* Колонка «Регистрация SQL Server с поддержкой Azure Arc» теперь содержит вкладку **Теги**. Теги включены в скрипт регистрации и отражаются в ресурсе(ах) **SQL Server — Azure Arc**. Подробности см. в [Подключение SQL Server к Azure Arc](connect.md).

* Запись **Работоспособность среды** теперь поддерживает активацию **Оценка SQL** с портала путем развертывания *CustomScriptExtension*. Дополнительные сведения см. в статье о [настройке Оценки SQL](assess.md#run-on-demand-sql-assessment).

### <a name="known-issues"></a>Известные проблемы

В отношении октябрьского выпуска действуют следующие ограничения.

* Для подключения экземпляров SQL Server к Azure Arc требуется учетная запись с широким набором разрешений. Подробности см. в [Необходимые разрешения](overview.md#required-permissions).

## <a name="september-2020"></a>Сентябрь 2020 г.

SQL Server с поддержкой Azure Arc выпущен как общедоступная предварительная версия. SQL Server с поддержкой Azure Arc распространяет службы Azure на экземпляры SQL Server, размещенные за пределами Azure в центре обработки данных клиента, в пограничной зоне или в среде с несколькими облаками.

Дополнительные сведения см. в [Обзор SQL Server с поддержкой Azure Arc](overview.md).

### <a name="known-issues"></a>Известные проблемы

В отношении сентябрьского выпуска действуют следующие ограничения.

* Колонка **Регистрация SQL Server с поддержкой Azure Arc** не поддерживает настройку пользовательских тегов. Чтобы добавить пользовательские теги, откройте ресурс **SQL Server — Azure Arc** после регистрации и измените теги на странице **Обзор**.

* Для подключения экземпляров SQL Server к Azure Arc требуется учетная запись с широким набором разрешений. Подробности см. в [Необходимые разрешения](overview.md#required-permissions).

## <a name="next-steps"></a>Следующие шаги

**Хотите попробовать?**  Быстро приступайте к работе с помощью [Руководства по началу работы с SQL Server с поддержкой Azure Arc](https://aka.ms/AzureArcSqlServerJumpstart).