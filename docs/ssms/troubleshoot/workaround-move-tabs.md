---
description: Обходное решение для перемещения вкладок
title: Возможность перемещения вкладок без сбоя среды SSMS
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654639"
---
# <a name="workaround-to-move-tabs"></a>Обходное решение для перемещения вкладок

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Для перемещения вкладок в окне редактора запросов или закрепления ранее удаленной вкладки может потребоваться обходное решение.  Если вы применили все доступные обновления Windows, но при операциях с вкладками редактора запросов все равно происходит аварийное завершение работы, воспользуйтесь описанным ниже [обходным решением](#workaround).

## <a name="applicable-environments"></a>Применимые среды
В обновлениях Windows для платформы .NET Framework есть известная ошибка, которая приводит к аварийному завершению работы SQL Server Management Studio (SSMS) при закреплении вкладок или разделении окна.  Последние сведения см. на странице [отзывов пользователей об SQL Server](https://feedback.azure.com/forums/908035/suggestions/42651556).

## <a name="workaround"></a>Обходной путь

Если аварийное завершение по-прежнему происходит после применения всех доступных обновлений Windows, выполните указанные ниже шаги, чтобы устранить проблему.

1. Закройте все экземпляры SQL Server Management Studio (SSMS).

2. Найдите файл приложения SSMS (EXE).  Обычно он находится в папке `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE`.

3. Откройте файл `Ssms.exe.config` в Блокноте от имени администратора.

4. Найдите узел `AppContextSwitchOverrides` и добавьте к его значению следующие два свойства.
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![Изменение файла ssms.exe.config](../media/troubleshoot/execonfig-edit.png)

5. Сохраните файл конфигурации и повторно откройте среду SSMS.
