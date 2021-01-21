---
title: Что представляет собой расширение языка Java?
titleSuffix: SQL Server Language Extensions
description: Расширение языка Java — это компонент SQL Server, используемый для выполнения внешнего кода Java. Реляционные данные могут использоваться во внешнем коде Java с помощью платформы расширяемости.
author: dphansen
ms.author: davidph
ms.date: 11/10/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4df5f2e96ff8795ac592c6aa61f7c79e03145c31
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596299"
---
# <a name="what-is-java-language-extension"></a>Что представляет собой расширение языка Java?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Расширение языка Java — это компонент SQL Server, используемый для выполнения внешнего кода Java. Реляционные данные могут использоваться во внешнем коде Java с помощью [платформы расширяемости](concepts/extensibility-framework.md). Расширение языка Java входит в состав пакета [расширений языков для SQL Server](language-extensions-overview.md).

Среда выполнения Java по умолчанию — Zulu Open JRE. Можно также использовать другую среду Java JRE или пакет SDK.

## <a name="what-you-can-do-with-the-java-language-extension"></a>Возможности расширения языка Java

Расширение языка Java использует платформу расширяемости для выполнения внешнего кода Java. Выполнение кода изолировано от процессов ядра, но полностью интегрировано с выполнением запросов SQL Server. Вы можете выполнять код Java в источнике данных, чтобы не передавать данные по сети.

Внешний код Java определяется с помощью инструкции [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). Системная хранимая процедура [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) используется в качестве интерфейса для выполнения кода Java.

## <a name="get-started-with-java-language-extension"></a>Начало работы с расширением языка Java

1. [Установите расширение языка Java для SQL Server в Windows](install/windows-java.md) или [в Linux](../linux/sql-server-linux-setup-language-extensions-java.md).

1. Настройте средства разработки.

    + Используйте свою привычную интегрированную среду разработки для написания кода Java.
    + Установите [пакет Microsoft Extensibility SDK для Java](how-to/extensibility-sdk-java-sql-server.md) для выполнения кода Java на SQL Server.
    + Используйте [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) для выполнения внешнего кода на SQL Server.
    + Используйте системную хранимую процедуру [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения кода Java на SQL Server.

1. Напишите свой первый код Java.

    + [Руководство. Регулярные выражения с Java](tutorials/search-for-string-using-regular-expressions-in-java.md)

## <a name="limitations"></a>Ограничения

Количество значений в буферах ввода и вывода не может превышать значение `MAX_INT (2^31-1)`, так как это максимальное количество элементов, которое может быть выделено для массива в Java.

## <a name="next-steps"></a>Дальнейшие действия

+ Установка [расширения языка Java для SQL Server в Windows](install/windows-java.md) или [Linux](../linux/sql-server-linux-setup-language-extensions-java.md)
+ Установка [Пакета Microsoft Extensibility SDK для Java](how-to/extensibility-sdk-java-sql-server.md)