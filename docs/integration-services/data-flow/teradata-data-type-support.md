---
description: Поддержка типов данных Teradata
title: Поддержка типов данных Teradata | Документация Майкрософт
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346527"
---
# <a name="data-type-support"></a>Поддержка типов данных

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Компоненты служб SSIS используют API Teradata Parallel Transporter (API TPT) для загрузки и передачи данных из базы данных Teradata и обратно, поэтому в службах SSIS можно использовать только тип данных, поддерживаемый API TPT.

> [!NOTE]
>
> Типы данных TIME, TIMESTAMP и INTERVAL в Teradata обрабатываются API TPT как символьные строки фиксированного размера. Компонентами служб SSIS для Teradata они обрабатываются как строки.

## <a name="data-type-mapping"></a>Сопоставление типов данных

В таблице ниже приведены типы данных базы данных Teradata и их стандартное сопоставление с типами данных служб SSIS. В ней также приведены неподдерживаемые типы данных Teradata.

|Тип данных SQL|Тип данных служб SSIS|
|:-|:-|
|DECIMAL, NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|ЦЕЛОЕ ЧИСЛО|DT_I4|
|FLOAT, REAL, DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TIME<br>TIME(n)|DT_STR|
|timestamp<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR (DT_WSTR для кодировки Юникод)<br>**Примечания**<br> Максимальная поддерживаемая длина VARCHAR составляет 32 000. <br> Максимальная допустимая длина DT_STR составляет 8000 символов, а DT_WSTR — 4000 символов. При превышении максимальной длины данные усекаются.|
|LONG VARCHAR|Не поддерживается|
|CLOB|Не поддерживается|
|BYTE|DT_BYTES<br>**Примечание.** Максимально допустимая длина составляет 8000 байтов. При превышении максимальной длины данные усекаются.|
|VARBYTE|DT_BYTES<br>**Примечание.** Максимально допустимая длина составляет 8000 байтов. При превышении максимальной длины данные усекаются.|
|BLOB|Не поддерживается|

## <a name="next-steps"></a>Дальнейшие действия

- Настройка [диспетчера подключений Teradata](teradata-connection-manager.md)
- Настройка [источника Teradata](teradata-source.md)
- Настройка [назначения Teradata](teradata-destination.md)
- Если у вас возникнут вопросы, посетите страницу [технического сообщества](https://aka.ms/AA6iwdw).
