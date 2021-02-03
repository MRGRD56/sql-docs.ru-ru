---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 0b9956dd515098189983b68f8639a22ab43da3a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185594"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Сведения

|attribute|Значение|
|---|---|
|Название продукта|SQL Server|
|Идентификатор события|4214|
|Источник события|MSSQLSERVER|
|Компонент|SQLEngine|
|Символическое имя|DMPX_NODBBACKUP|
|Текст сообщения|Инструкцию BACKUP LOG невозможно выполнить, так как не существует резервной копии текущей базы данных.|
||

## <a name="explanation"></a>Пояснение

Эта ошибка возникает при попытке выполнить резервное копирование журнала транзакций перед полным резервным копированием базы данных. Перед тем как выполнять резервное копирование журнала транзакций для базы данных, необходимо произвести ее полное резервное копирование. Кроме того, в журнале ошибок появится следующее сообщение:

> Резервное копирование \<Datetime>    Ошибка: 3041, серьезность: 16, состояние: 1.  
Резервное копирование \<Datetime>     BACKUP не выполнила команду BACKUP LOG \<db_name>. Проверьте дополнительные сообщения в журнале приложения резервного копирования.

## <a name="user-action"></a>Рекомендуемые действия

Перед тем как выполнять резервное копирование журнала транзакций для базы данных, произведите ее полное резервное копирование.

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения см. в следующей статье: [Резервное копирование и восстановление баз данных SQL Server](../backup-restore/back-up-and-restore-of-sql-server-databases.md).