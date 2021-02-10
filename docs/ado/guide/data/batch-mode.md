---
description: Пакетный режим
title: Пакетный режим | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data updates [ADO], batch mode
- batch mode [ADO]
- updating data [ADO], batch mode
ms.assetid: 0cb548e0-fcb4-4c49-98c8-be287911f826
author: rothja
ms.author: jroth
ms.openlocfilehash: ee9e67527b109b36c7ded266086f9efea06e4381
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028029"
---
# <a name="batch-mode"></a>Пакетный режим
Пакетный режим действует, если свойство **LockType** имеет значение **адлоккбатчоптимистик** , а пакетное обновление поддерживается поставщиком. Некоторые параметры типа блокировки недоступны в зависимости от положения курсора. Например, тип пессимистической блокировки недоступен, если для **CursorLocation** задано значение **адусеклиент**. И наоборот, поставщик не может поддерживать оптимистичную блокировку пакетной службы, если расположение курсора находится на сервере. Пакетное обновление следует использовать только с курсором KEYSET или static.  
  
 Метод **UpdateBatch** используется для отправки изменений **набора записей** , хранящихся в буфере копирования, на сервер для обновления источника данных. В следующем разделе будет открыт **набор записей** в пакетном режиме, внесены изменения в буфер копирования, а затем отправлены изменения в источник данных с помощью вызова **UpdateBatch**.  
  
 В этом разделе рассматриваются следующие вопросы.  
  
-   [Отправка обновлений: метод UpdateBatch](./sending-the-updates-updatebatch-method.md)  
  
-   [Фильтрация обновленных записей](./filtering-for-updated-records.md)  
  
-   [Работа с ошибками обновлений](./dealing-with-failed-updates.md)  
  
-   [Обнаружение и разрешение конфликтов](./detecting-and-resolving-conflicts.md)  
  
-   [Отключение и повторное подключение набора записей](./disconnecting-and-reconnecting-the-recordset.md)  
  
-   [Обновление результатов объединения JOIN: уникальная таблица](./updating-joined-results-unique-table.md)