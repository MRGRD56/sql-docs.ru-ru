---
title: Заметки о выпуске ODBC Driver for SQL Server для Windows
description: В этих заметках о выпуске описываются изменения в каждом выпуске драйвера Microsoft ODBC Driver for SQL Server в Windows.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
ms.reviewer: v-daenge
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 2c7ea508a6f0f79a000de1ae42f5e7eb3de09f5a
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464772"
---
# <a name="release-notes-for-microsoft-odbc-driver-for-sql-server-on-windows"></a>Заметки о выпуске Microsoft ODBC Driver for SQL Server для Windows

В этих заметках о выпуске описываются новые возможности драйвера Microsoft ODBC для SQL Server в Windows.

<!--
PLEASE USE THE STANDARD 2-COLUMN TABLE FORMAT!

For all our Release Notes articles (What's New too?), we are standardizing on the 2-column format that you see here for version "## 17.3".

Going forward, all new additions to this article must use the 2-column format.

Also, use the shorter ## H2 title format, which eliminates all the redundant constants, and appends the date-added.
One beneift of shortness is the avoidance of the annoying wrapping of unnecessarily long H2 titles in the rightNav.
- OLD H2:  ## What's New in the [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] on Windows
- NEW H2:  ## 17.3, February 2019

By the way, in GitHub, the file name is changing today 2019/03/30:
- FROM:  docs/connect/odbc/windows/release-notes.md
- TO  :  docs/connect/odbc/windows/release-notes-odbc-sql-server-windows.md

Thank you.
GeneMi (and CraigG).  2019/03/30.
-->

## <a name="177"></a>17,7

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2153471)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2153469)  

Номер версии: 17.7.1.1  
Выпущено: 29 января 2021 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

| Добавленная возможность | Сведения |
| :------- | :------ |
| Интерактивная проверка подлинности Azure Key Vault | Подробные сведения см. в статье [Использование Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Проверка подлинности на основе субъекта-службы | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Возможность вставки в зашифрованные столбцы money и smallmoney | Подробные сведения см. в статье [Использование Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>Предыдущие выпуски

## <a name="176"></a>17.6

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2137027)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2137028)  

Номер версии: 17.6.1.1  
Выпущено: 31 июля 2020 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40a)

| Добавленная возможность | Сведения |
| :------- | :------ |
| Кэширование метаданных для подготовленных инструкций | Подробные сведения см. в статье [Использование Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Атрибут подключения SQL_COPT_SS_AUTOBEGINTXN, определяющий, выполняется ли автоматически инструкция BEGIN TRANSACTION после ROLLBACK или COMMIT. | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1752"></a>17.5.2

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120137)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120140)  

Номер версии: 17.5.2.1  
Выпущено: 6 марта 2020 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40a)  

### <a name="features-added-in-1752"></a>Возможности, добавленные в 17.5.2

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка проверки подлинности с помощью управляемого удостоверения для Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка дополнительных конечных точек Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

Чтобы скачать предыдущие версии драйвера ODBC, щелкните заголовки со ссылками в приведенных ниже разделах.

## <a name="175"></a>17.5

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120248)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120353)  

Номер версии: 17.5.1.1  
Выпущено: 31 января 2020 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40a)  

### <a name="features-added-in-175"></a>Возможности, добавленные в 17.5

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Атрибут подключения SQL_COPT_SS_SPID для получения SPID без обращения к серверу | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1742"></a>17.4.2

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120354)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120249)  

Номер версии: 17.4.2.1  
Выпущено: Октябрь 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40a)  

### <a name="features-added-in-1742"></a>Возможности, добавленные в 17.4.2

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка дополнительных конечных точек Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка настройки версии классификации данных | См. статью [Классификация данных](../data-classification.md#bkmk-version). |
| Включение библиотеки проверки подлинности Azure Active Directory (adal.dll) в установщик | Теперь Библиотека проверки подлинности Active Directory корпорации Майкрософт для SQL Server включена в базовую установку драйвера, а значит, существующие установки библиотеки будут автоматически обновлены и удалены установщиком ODBC из списка установленных приложений в Windows. |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="174"></a>17.4

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120149)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120150)  

Номер версии: 17.4.1.1  
Выпущено: Июль 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40a)  

### <a name="features-added-in-174"></a>Возможности, добавленные в 17.4

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Always Encrypted с безопасными анклавами. | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Настраиваемые параметры проверки активности TCP. | См. сведения о [подключении к SQL Server](../linux-mac/connection-string-keywords-and-data-source-names-dsns.md). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="173"></a>17.3

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120355)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120356)  

Номер версии: 17.3.1.1  
Выпущено: Февраль 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40a)  

### <a name="features-added-in-173"></a>Возможности, добавленные в 17.3

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Режим проверки подлинности Управляемого удостоверения Azure Active Directory (назначаемого системой и пользователем). | См. статью [Использование Azure Active Directory с драйвером ODBC](../using-azure-active-directory.md). |
| Возможность передавать входные параметры в потоковом режиме для столбцов Always Encrypted. | См. раздел [Ограничения драйвера ODBC при использовании Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted). |
| Распределенные транзакции XA. | [Использование транзакций XA](../use-xa-with-dtc.md). |
| Обновленный распространяемый компонент Visual C++ | Зависимость среды выполнения обновлена до распространяемого пакета Visual C++ 2017 ([загрузка для x64](https://aka.ms/vs/15/release/vc_redist.x64.exe) [загрузка для x86](https://aka.ms/vs/15/release/vc_redist.x86.exe)) |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="172"></a>17.2

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120250)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120357)  

Номер версии: 17.2.0.1  
Выпущено: Июль 2018 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40a)  

### <a name="features-added-in-172"></a>Возможности, добавленные в 17.2

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Классификация данных для Базы данных SQL Azure и SQL Server. | См. статью [Классификация данных](../data-classification.md). |
| Поддержка кодировки UTF-8 на сервере. | &nbsp; |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="171"></a>17.1

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120151)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120443)  

Номер версии: 17.1.0.1  
Выпущено: Март 2018 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40a)  

### <a name="features-added-in-171"></a>Возможности, добавленные в 17.1

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка атрибутов подключения `SQL_COPT_SS_CEKCACHETTL` и `SQL_COPT_SS_TRUSTEDCMKPATHS`. | &bull; &nbsp; `SQL_COPT_SS_CEKCACHETTL`<br/>Позволяет управлять временем существования локального кэша для ключей шифрования столбцов, а также освобождать его.<br/><br/>&bull; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS`<br/>Позволяет приложению ограничивать главные ключи столбцов, используемые для операций Always Encrypted, определенным списком.<br/><br/> Дополнительные сведения см. в статье [Использование функции Always Encrypted с драйвером ODBC для SQL Server](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка интерактивной проверки подлинности Azure Active Directory | &nbsp; |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="170"></a>17.0

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2120444)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120152)  

Номер версии: 17.0.1.1  
Выпущено: Февраль 2018 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40a)  

### <a name="features-added-in-170"></a>Возможности, добавленные в 17.0

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка Always Encrypted для API BCP. | &nbsp; |
| Новый атрибут строки подключения `UseFMTOnly`. | Предписывает драйверу использовать старые метаданные в особых случаях, в которых требуются временные таблицы. |
| Поддержка Управляемого экземпляра SQL Azure. | См. список [отличий при использовании Управляемого экземпляра (ODBC версии 17)](#diffs-managed-instance-17). |
| &nbsp; | &nbsp; |

| Изменение зависимости | Сведения |
| :------------ | :------ |
| Удален помощник по входу в Microsoft Online Services. | Зависимость удалена. |
| &nbsp; | &nbsp; |

### <a name="differences-when-using-managed-instance-odbc-version-17"></a><a name="diffs-managed-instance-17"></a> Отличия при использовании Управляемого экземпляра (ODBC версии 17)

В этой версии ODBC имеется поддержка Управляемого экземпляра базы данных SQL Azure. Ниже представлен список отличий при использовании Управляемого экземпляра.

> [!NOTE]
> При использовании Управляемого экземпляра есть ряд особенностей.
>
> - FILESTREAM не поддерживается.
> - Доступ к локальной файловой системе не поддерживается, однако требуется, например, для файлов трассировки.
> - Создание пользовательских типов по локальным путям не поддерживается.
> - Встроенная проверка подлинности Windows не поддерживается.
> - DTC не поддерживается.
> - Учетная запись `sa` отсутствует (учетная запись по умолчанию называется `cloudSA`).
> - В ошибке токена TDS (0xAA) возвращается неправильное имя сервера.
> - Специальные символы в имени базы данных не поддерживаются.
> - Инструкция ALTER DATABASE [имя_БД1] MODIFY NAME = [имя_БД2] не поддерживается.
> - Сообщения об ошибках всегда выводятся на английском языке независимо от выбранного языка (так же как в Azure).

## <a name="131"></a>Версия 13.1

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2121020)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120923)  

Номер версии: Версия 13.1  

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40a)  

[Скачать Microsoft Command Line Utilities 13.1 для SQL Server](https://www.microsoft.com/download/details.aspx?id=53591)

### <a name="features-added-in-131"></a>Возможности, добавленные в 13.1

| Добавленная возможность | Сведения |
| :------------ | :------ |
| В драйвере ODBC 13.1 для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] добавлена поддержка [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) и [Azure Active Directory](../using-azure-active-directory.md). | Поддержка этих функций доступна при подключении к Microsoft SQL Server 2016 или более поздней версии. |
| Появились ключевые слова и атрибуты для пулов соединений, соответствующие поддержке Always Encrypted и Azure Active Directory. | Эти ключевые слова и атрибуты описываются в статье [Организация пулов соединений с учетом драйвера в ODBC Driver for SQL Server](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="13"></a>13

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2121118)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2120924)  

Номер версии: 13  

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40a)  

[Скачать Microsoft Command Line Utilities 13 для SQL Server](https://www.microsoft.com/download/details.aspx?id=52680)

### <a name="features-added-in-13"></a>Возможности, добавленные в 13

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Добавлена поддержка Microsoft SQL Server 2016. | Сохраняются функциональные возможности драйвера ODBC версии 11. |
| &nbsp; | &nbsp; |

## <a name="11"></a>11

![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2121206)  
![скачать](../../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2121021)  

Номер версии: 11  

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40a)  

[Скачать Microsoft Command Line Utilities 11 для SQL Server](https://www.microsoft.com/download/details.aspx?id=36433)  

### <a name="features-added-in-11"></a>Возможности, добавленные в 11

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Появились новые возможности. | См. статью [Функции Microsoft ODBC Driver for SQL Server в Windows](features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md). |
| Включены все возможности, имевшиеся в ODBC для SQL Server 2012 Native Client. | &nbsp; |
| &nbsp; | &nbsp; |
