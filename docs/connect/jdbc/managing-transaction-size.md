---
description: Узнайте, как управлять размером транзакций, чтобы не добавлять в приложение блокировки, которые будут блокировать других пользователей.
title: Управление размером транзакций
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 82900342-bc80-445f-98a4-468a303aae1e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2289e35116fc9ae39210db1b2af7dcb6e7dfad7a
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793036"
---
# <a name="managing-transaction-size"></a>Управление размером транзакций

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

При работе с транзакциями важно, чтобы транзакции были как можно более короткими. Применяемый по умолчанию режим автофиксации, который можно включить или отключить с использованием метода [setAutoCommit](reference/setautocommit-method-sqlserverconnection.md), будет фиксировать каждое действие. Это самый простой режим работы для большинства разработчиков.

При использовании ручных транзакций убедитесь, что код фиксирует транзакцию как можно скорее. Будучи открытой, транзакция блокирует доступ к данным для остальных пользователей. Например, хорошим приемом программирования считается помещение вызова rollback в блок catch, а вызова commit — в блок `finally`. Но это зависит от конструкции вашего приложения.

Чтобы повысить степень параллелизма, необходимо уменьшить размер транзакций. Например, если вы запускаете транзакцию вручную и изменяете 10 000 строк в таблице с 20 000 строками, половина таблицы будет заблокирована для других пользователей, даже если они будут только читать данные. Сокращение же модификаций до 2 000 строк оставит таблицу доступной на 90 процентов.

Кроме того, обязательно используйте параметр истечения времени блокировки, если в приложении ожидаются проблемы с блокировками. Для этого можно использовать метод [setLockTimeout](reference/setlocktimeout-method-sqlserverdatasource.md). Установленное по умолчанию значение истечения времени блокировки равно –1. Это означает блокировку на неопределенно долгое время в ожидании блокировки. Можно установить время ожидания блокировки в 30 секунд, чтобы настроить истечение времени ожидания заблокированного соединения через 30 секунд в случае блокировки другим подключением.

## <a name="see-also"></a>См. также раздел

[Повышение производительности и надежности с помощью JDBC Driver](improving-performance-and-reliability-with-the-jdbc-driver.md)
