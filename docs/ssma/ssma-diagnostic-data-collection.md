---
title: Сбор данных об использовании и диагностике SSMA
description: Сведения об использовании и сборе диагностических данных в Помощник по миграции SQL Server.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 04/02/2021
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.author: alexiva
ms.openlocfilehash: d306193accdf97ab04b45724cc8a275c890bec80
ms.sourcegitcommit: f1a571b6ce02a39c385ad32508ceff23475ed9f5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387390"
---
# <a name="ssma-usage-and-diagnostic-data-collection"></a>Сбор данных об использовании и диагностике SSMA

Помощник по миграции SQL Server (SSMA) содержит компоненты с поддержкой Интернета, которые могут выполнять сбор и отправку в корпорацию Майкрософт анонимных данных об использовании и диагностике компонентов.

## <a name="collected-data"></a>Собранные данные

SSMA может получать стандартные сведения о компьютере и сведения об использовании и производительности, которые могут передаваться в корпорацию Майкрософт и анализироваться в целях повышения качества, безопасности и надежности SSMA. Сбор таких сведений, как имена, адреса и другие сведения личного характера, не осуществляется. Дополнительные сведения см. в [заявлении о конфиденциальности Майкрософт](https://privacy.microsoft.com/privacystatement) и [приложении к заявлению о конфиденциальности SQL Server](../sql-server/sql-server-privacy.md).
## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssma"></a>Включение и отключение сбора данных об использовании и диагностике в SSMA

Следующая запись реестра позволяет принять или отказаться от сбора данных:

Имя RegEntry — `DisableTelemetry`.  
Тип записи `STRING` : `True` отказ от участия; отсутствует `False` или отсутствует согласие

Кроме того, можно отключить автоматические проверки для более новых версий средств во время запуска, добавив следующую запись:

Имя RegEntry — `DisableAutoUpdate`.  
Тип записи `STRING` : `True` отказ от участия; отсутствует `False` или отсутствует согласие

В зависимости от источника миграции запись должна быть помещена в один из следующих разделов реестра:

- Для Помощник по миграции SQL Server доступа:

  Подраздел = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Access`  
  Подраздел (32-разрядное приложение, установленное в 64-разрядной ОС) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Access`  

- Для Помощник по миграции SQL Server для DB2:

  Подраздел = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for DB2`  
  Подраздел (32-разрядное приложение, установленное в 64-разрядной ОС) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for DB2`  

- Для Помощник по миграции SQL Server для MySQL:

  Подраздел = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for MySQL`  
  Подраздел (32-разрядное приложение, установленное в 64-разрядной ОС) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for MySQL`  

- Для Помощник по миграции SQL Server для Oracle:

  Подраздел = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Oracle`  
  Подраздел (32-разрядное приложение, установленное в 64-разрядной ОС) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Oracle`  

- Для Помощник по миграции SQL Server для Sybase:

  Подраздел = `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server Migration Assistant for Sybase`  
  Подраздел (32-разрядное приложение, установленное в 64-разрядной ОС) = `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server Migration Assistant for Sybase`  
