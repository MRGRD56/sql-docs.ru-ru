---
title: Создание оценки миграции служб SSIS с помощью Помощник по миграции данных
description: Узнайте, как использовать Помощник по миграции данных для оценки локальной службы интеграции SQL Server (SSIS) перед миграцией в базу данных SQL Azure или Azure SQL Управляемый экземпляр
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: e32a7334ecef7531311ead57332f9b7e4d7f6322
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247318"
---
# <a name="perform-a-sql-server-integration-service-migration-assessment-with-data-migration-assistant"></a>Выполнение оценки миграции службы интеграции SQL Server с Помощник по миграции данных

## <a name="prerequisites"></a>Предварительные требования

Чтобы SQL Server оценить пакеты служб Integration Services (SSIS), необходимо установить следующие компоненты с помощью Помощник по миграции данных:

- Служба интеграции SQL Server с той же версией, что и пакеты служб SSIS для оценки.
- Пакет дополнительных компонентов Azure или другие сторонние компоненты, если пакеты служб SSIS для оценки имеют эти компоненты.  

DMA необходимо запустить с **правами администратора** для оценки пакетов служб SSIS в хранилище пакетов.

## <a name="performance-assessments"></a>Оценка производительности

Следующие пошаговые инструкции помогут вам выполнить первую оценку SQL Server миграции пакетов служб Integration Services (SSIS) в базу данных SQL Azure или Azure SQL Управляемый экземпляр с помощью Помощник по миграции данных.

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Создание оценки

1. Выберите **Новый** значок (+), а затем выберите тип проекта **оценки** в качестве **службы интеграции**.

1. Укажите тип исходного и целевого серверов.

    Выберите источник как **SQL Server** и задайте тип целевого сервера в качестве **базы данных SQL Azure** или **Azure SQL управляемый экземпляр**.

1. Нажмите кнопку **Создать**.

    ![создать оценку](media/dma-assess-ssis/dma-assess-ssis-create.png)

## <a name="connect-to-a-server"></a>Подключение к серверу

1. Используйте параметр по умолчанию и нажмите кнопку **Далее** , чтобы **выбрать источники**.
1. Введите имя экземпляра SQL Server, выберите тип проверки подлинности и задайте правильные свойства соединения.
1. Используемых Введите путь к папке, содержащей пакеты служб SSIS.
1. Используемых Введите пароль шифрования пакета, если это применимо.
1. Щелкните **Подключиться** к ИСХОДНОМу экземпляру SQL Server.
  ![Снимок экрана, показывающий панель "подключение к серверу" с параметром введите путь к папке, содержащей пакеты служб SSIS, и введите пароль шифрования пакета, если применимый параметр — out.](media/dma-assess-ssis/dma-assess-ssis-addsource.png)

## <a name="add-sources-to-assess"></a>Добавление источников для оценки

1. Выберите типы хранилища пакетов служб SSIS для оценки, а затем нажмите кнопку **Добавить**.
![Снимок экрана, показывающий панель добавления источников.](media/dma-assess-ssis/dma-assess-ssis-addsource-type.png)
1. Выберите **Добавить источники** , чтобы открыть всплывающее меню подключения, если требуется оценка нескольких папок.
1. Нажмите кнопку **Start Assessment** (Запустить оценку).
  ![Начать оценку](media/dma-assess-ssis/dma-assess-ssis-assess.png)

## <a name="view-results"></a>Просмотр результатов

Категория проблемы совместимости предоставляет частично поддерживаемые или неподдерживаемые функции, которые блокируют миграцию локальных пакетов служб SSIS в Azure-SSIS Integration Runtime. Затем он предоставляет рекомендации по устранению этих проблем.

![Просмотр результатов](media/dma-assess-ssis/dma-assess-ssis-result.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Общие сведения о переносе локальных рабочих нагрузок служб SSIS в службы SSIS в ADF](/azure/data-factory/scenario-ssis-migration-overview)
- [Перенос пакетов SQL Server Integration Services в Управляемый экземпляр SQL Azure](/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Повторное развертывание пакетов SQL Server Integration Services в базе данных SQL Azure](/azure/dms/how-to-migrate-ssis-packages)