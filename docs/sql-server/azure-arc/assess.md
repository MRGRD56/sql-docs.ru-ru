---
title: Настройка оценки SQL по запросу для экземпляра SQL Server с поддержкой Azure Arc
description: Настройка оценки SQL по запросу для экземпляра SQL Server с поддержкой Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: f1fcf571b491a4efc5759c10c4243f3b6b5ed135
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556417"
---
# <a name="configure-sql-assessment-on-an-azure-arc-enabled-sql-server-instance"></a>Настройка оценки SQL для экземпляра SQL Server с поддержкой Azure Arc

Оценка SQL предоставляет механизм для вычисления конфигурации SQL Server. В этой статье приводятся инструкции по использованию оценки SQL в экземпляре SQL Server с поддержкой Azure Arc.

## <a name="prerequisites"></a>Предварительные требования

* Ваш экземпляр SQL Server на базе Windows должен быть подключен к Azure Arc. Выполните инструкции по [подключению экземпляра SQL Server к SQL Server с поддержкой Arc](connect.md).
   
   > [!NOTE]
   > В настоящее время Оценка SQL по запросу поддерживает только SQL Server на компьютерах Windows.

* На компьютере должно быть установлено и настроено расширение Microsoft Monitoring Agent (MMA). Инструкции см. в статье [Установка MMA](configure-advanced-data-security.md#install-microsoft-monitoring-agent-mma). Кроме того, дополнительные сведения см. в статье [Log Analytics Agent](/azure/azure-monitor/platform/log-analytics-agent).

* В экземпляре SQL Server должен быть [включен протокол TCP/IP](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).

* При работе с именованным экземпляром SQL Server должна быть запущена [служба обозревателя SQL Server](../../tools/configuration-manager/sql-server-browser-service.md).

* Убедитесь, что вы проверили документ по SQL Server в разделе [Предварительные требования для оценок по запросу Центра служб](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

## <a name="run-on-demand-sql-assessment"></a>Выполнение оценки SQL по требованию

1. Откройте SQL Server — ресурс Azure Arc и выберите **Работоспособность среды** на панели слева.

   > [!div class="mx-imgBorder"]
   > [ ![Снимок экрана: экран "Работоспособность среды" в SQL Server. Ресурса Azure Arc.](media/assess/sql-assessment-heading-sql-server-arc.png) ](media/assess/sql-assessment-heading-sql-server-arc.png#lightbox)

> [!IMPORTANT]
> Если расширение MMA не установлено, вы не сможете инициировать Оценку SQL по запросу.

2. Выберите тип учетной записи. Если у вас есть управляемая учетная запись службы, она позволит запускать Оценку SQL непосредственно с портала. Укажите имя учетной записи.

> [!NOTE]
> Указание *Управляемая учетная запись службы* активирует кнопку **Настроить Оценку SQL** , чтобы можно было инициировать оценку с портала, развернув *CustomScriptExtension*. Так как одновременно можно развернуть только одно *CustomScriptExtension*, расширение скрипта для Оценки SQL будет автоматически удалено после выполнения. Если у вас уже есть другое *CustomScriptExtension*, развернутое на размещающем компьютере, кнопка **Настройка Оценки SQL** не будет активирована.

3. Укажите рабочую папку на компьютере для сбора данных, если вы хотите изменить вариант по умолчанию. По умолчанию используется `C:\sql_assessment\work_dir`. Во время сбора и анализа данные временно хранятся в этой папке. Если папка не существует, она будет создана автоматически.

4. Если вы инициируете Оценку SQL с портала, нажимая кнопку **Настроить Оценку SQL**, отображается стандартное всплывающее меню развертывания.

> [!div class="mx-imgBorder"]
   > [ ![Снимок экрана, на котором показано развертывание CustomScriptExtension.](media/assess/sql-assessment-custom-script-deployment.png) ](media/assess/sql-assessment-custom-script-deployment.png#lightbox)

5. Если вы предпочитаете инициировать Оценку SQL с целевого компьютере, щелкните **Загрузить скрипт конфигурации**, скопируйте загруженный скрипт на целевой компьютер и выполните один из следующих блоков кода в экземпляре **powershell.exe** администратора:

   * _Учетная запись домена_:  Вам будет предложено ввести учетную запись пользователя и пароль.

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1'
      ```

   * _Управляемая учетная запись службы (MSA)_

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1' -ManagedServiceAccountName <MSA account name>
      ```

> [!NOTE]
> Скрипт планирует задачу с именем *SQLAssessment*, которая запускает сбор данных. Эта задача выполняется в течение часа после выполнения скрипта. Затем она повторяется каждые семь дней.

> [!TIP]
> Вы можете изменить эту задачу, чтобы она выполнялась в другой день и время, или даже принудительно запустить ее немедленно. В библиотеке планировщика заданий найдите **Microsoft** > **Operations Management Suite** > **AOI\*\*\***  > **Assessments** > **SQLAssessment**.

## <a name="view-sql-assessment-results"></a>Просмотр результатов оценки SQL

* На панели _Работоспособность среды_ выберите **Представление результатов оценки SQL**.

   > [!NOTE]
   > Кнопка **Представление результатов оценки SQL** остается неактивной до тех пор, пока результаты не будут готовы в журнале аналитики. Этот процесс может занять до двух часов после обработки файлов данных на целевом компьютере.

   > [!div class="mx-imgBorder"]
   > [ ![Снимок экрана: результаты оценки SQL.](media/assess/sql-assessment-results.png) ](media/assess/sql-assessment-results.png#lightbox)

* Узнать состояние обработки данных на компьютере для сбора можно, просмотрев файлы в рабочей папке. После завершения запланированной задачи вы увидите несколько файлов с префиксом _new._ Префикс в рабочей папке.

   > [!div class="mx-imgBorder"]
   > [ ![Снимок экрана: окно диспетчера файлов, в котором отображаются новые файлы данных в рабочей папке.](media/assess/sql-assessment-data-files-ready.png) ](media/assess/sql-assessment-data-files-ready.png#lightbox)

* Microsoft Monitoring Agent сканирует рабочую папку каждые 15 минут. Он ищет _new.*_ и отправляет данные в рабочую область Log Analytics. После того как MMA отправит файл, префикс меняется с _new._ на _processed._

   > [!div class="mx-imgBorder"]
   > ![Снимок экрана: окно диспетчера файлов, отображающее обработанные файлы данных.](media/assess/sql-assessment-data-files-processed.png)

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения можно получить, просмотрев предварительные документы в разделе [Предварительные требования для оценок по запросу Центра служб](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

* Для получения комплексной поддержки функции оценки SQL по запросу требуется подписка уровня Premier или Unified. Дополнительные сведения см. в статье [Поддержка Azure Premier](https://azure.microsoft.com/support/plans/premier).
