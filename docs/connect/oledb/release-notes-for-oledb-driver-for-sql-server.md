---
title: Заметки о выпуске OLE DB Driver
description: В этих заметках о выпуске описываются изменения в каждом выпуске драйвера Microsoft OLE DB Driver for SQL Server.
ms.date: 12/01/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: genemi
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dfc66e7c1f1aa787ccc366c7dc50613741724a1f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837494"
---
# <a name="release-notes-for-the-microsoft-ole-db-driver-for-sql-server"></a>Заметки о выпуске Microsoft OLE DB Driver for SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта страница описывает, что было добавлено в каждой версии драйвера Microsoft OLE DB для SQL Server.

<!--
USE THE TABLE FORMAT!
Hello, from now on, please use the table-based format standard for all new Release Notes content.
See section "## 18.2.1" for a live example in this article.
Thank you. For questions, contact GeneMi. (2019/03/16)
-->

## <a name="1850"></a>18.5.0
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2135577)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2135722)  

Выпущено: 1 декабря 2020 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
    Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)  
    Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)  

### <a name="features-added"></a>Добавлены возможности

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Включено [обнаружение и классификация данных SQL](../../relational-databases/security/sql-data-discovery-and-classification.md) | [Использование классификации данных](features/using-data-classification.md) |
| Поддержка проверки подлинности субъекта-службы Azure Active Directory (`ActiveDirectoryServicePrincipal`) | [Использование Azure Active Directory](features/using-azure-active-directory.md) |

### <a name="bugs-fixed"></a>Исправлены ошибки

| Исправление ошибки | Сведения |
| :-------- | :------ |
| Исправлена проблема с внедренными символами NUL. | Исправлена ошибка, из-за которой драйвер возвращал неправильную длину строк с внедренными символами NUL. |
| Исправлена утечка памяти в интерфейсе [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md). | Исправлена утечка памяти в интерфейсе [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md) для операций с массовым копированием данных типа `sql_variant`. |
| Исправлены ошибки, из-за которых возвращаются неправильные значения для свойств `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD` и `SSPROP_MUTUALLYAUTHENTICATED`. | Предыдущие версии драйвера возвращали усеченные значения свойства `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD`. Кроме того, в случае проверки подлинности `ActiveDirectoryIntegrated`возвращаемое значение свойства `SSPROP_MUTUALLYAUTHENTICATED` было `VARIANT_FALSE`, даже если обе стороны выполняли взаимную проверку подлинности.|
| Исправлена ошибка при вставке удаленной таблицы связанного сервера. | Исправлена ошибка, которая приводила к сбою вставки удаленной таблицы связанного сервера, если был включен [параметр конфигурации сервера NOCOUNT](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md). |

## <a name="previous-releases"></a>Предыдущие выпуски

## <a name="1840"></a>18.4.0
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2129954)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2131003)  

Выпущено: Май 2020 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40a)  

### <a name="features-added"></a>Добавлены возможности

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка разрешения IP-адресов прозрачной сети (TNIR) |[Разрешение IP-адресов прозрачной сети (TNIR)](features/using-transparent-network-ip-resolution.md)|
| Поддержка кодировки UTF-8 в клиенте | [Поддержка UTF-8 в драйвере OLE DB для SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |

### <a name="bugs-fixed"></a>Исправлены ошибки

| Исправление ошибки | Сведения |
| :-------- | :------ |
| Исправлены различные ошибки в интерфейсе [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) | Некоторые ошибки, касающиеся многобайтовых кодовых страниц, приводили к тому, что интерфейс преждевременно сообщал о завершении потока во время операции чтения.|
| Исправлена утечка памяти в интерфейсе [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) | Исправлена утечка памяти в интерфейсе [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) при включении свойства `SSPROP_IRowsetFastLoad`. |
| Исправлена ошибка в сценариях с использованием типа данных `sql_variant` и строк, отличных от ASCII. | Некоторые сценарии с использованием типа данных `sql_variant` и строк, отличных от ASCII, могут приводить к повреждению данных. Подробная информация доступна в следующих статьях: [Известные проблемы](ole-db-data-types/ssvariant-structure.md#known-issues). |
| Устранены проблемы с кнопкой *Проверить подключение* в диалоговом окне [Настройка UDL](help-topics/data-link-pages.md). | Кнопка *Проверить соединение* в диалоговом окне [настройки UDL](help-topics/data-link-pages.md) теперь учитывает свойства инициализации, заданные на вкладке *Все*. |
| Исправлена проблема с обработкой значения по умолчанию для свойства `SSPROP_INIT_PACKETSIZE` | Устранена непредвиденная ошибка, возникавшая, когда свойству `SSPROP_INIT_PACKETSIZE` присваивалось значение по умолчанию `0`. Подробные сведения об этом свойстве см. в статье [Свойства инициализации и авторизации](ole-db-data-source-objects/initialization-and-authorization-properties.md). |
| Устранены проблемы с переполнением буфера в [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md) | Устранены проблемы переполнения буфера при использовании неверно сформированных файлов данных. |
| Устранены проблемы со специальными возможностями. | Устранены проблемы со специальными возможностями в пользовательском интерфейсе установщика и [диалоговом окне входа в SQL Server](help-topics/sql-server-login-dialog.md) (чтение содержимого, позиции табуляции). |

## <a name="1830"></a>18.3.0

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2117515)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2117517)  

Выпущено: Октябрь 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40a)  

### <a name="features-added"></a>Добавлены возможности

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка проверки подлинности Azure Active Directory (`ActiveDirectoryInteractive`, `ActiveDirectoryMSI`) | [Использование Azure Active Directory](features/using-azure-active-directory.md) |
| Включение библиотеки проверки подлинности Azure Active Directory (adal.dll) в установщик | Теперь Библиотека проверки подлинности Active Directory корпорации Майкрософт для SQL Server включена в базовую установку драйвера, а значит, существующие установки библиотеки будут автоматически обновлены и удалены установщиком OLE DB из списка установленных приложений в Windows. |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed"></a>Исправлены ошибки

| Исправление ошибки | Сведения |
| :-------- | :------ |
| Исправлена логика удаления индекса в [IIndexDefinition::DropIndex](/previous-versions/windows/desktop/ms722733(v=vs.85)). | Предыдущие версии драйвера OLE DB не умеют удалять индекс первичного ключа, если идентификатор схемы не совпадает с идентификатором владельца этого индекса. |
| &nbsp; | &nbsp; |

Чтобы скачать предыдущие версии драйвера OLE DB, щелкните заголовки со ссылками в приведенных ниже разделах.

## <a name="1823"></a>18.2.3

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2119554)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2119738)  

Выпущено: Июнь 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40a)  

### <a name="features-added-in-1823"></a>Возможности, добавленные в 18.2.3

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка обновления драйверов со съемного носителя SQL Server | Это обновление включает поддержку обновления драйверов непосредственно со съемного носителя SQL Server. |
| &nbsp; | &nbsp; |

## <a name="1822"></a>18.2.2

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2118512)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2118415)  

Выпущено: Май 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40a)  

### <a name="bugs-fixed-in-1822"></a>Ошибки, исправленные в 18.2.2

| Исправление ошибки | Сведения |
| :-------- | :------ |
| Исправлена неинтерактивная проверка подлинности Azure Active Directory в многопотоковом подразделении (MTA). | Драйвер OLE DB 18.2.1 некорректно пытается изменить модель параллелизма COM в подразделении, ранее инициализированном как многопотоковое (MTA). В результате, если приложение впоследствии совершает несколько вызовов [CoInitialize](/windows/win32/api/objbase/nf-objbase-coinitialize) или [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) до вызова интерфейса [IDBInitialize::Initialize](/previous-versions/windows/desktop/ms718026(v=vs.85)), драйверу не удается подключиться с использованием какой-либо модели проверки подлинности Azure Active Directory. |
| &nbsp; | &nbsp; |

## <a name="1821"></a>18.2.1

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2118511)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2118278)  

Выпущено: Февраль 2019 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40a)  

### <a name="features-added-in-1821"></a>Возможности, добавленные в 18.2.1

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка кодировки UTF-8 на сервере | [Поддержка UTF-8 в драйвере OLE DB для SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |
| Поддержка проверки подлинности Azure Active Directory | [Использование Azure Active Directory](features/using-azure-active-directory.md) |
| &nbsp; | &nbsp; |

## <a name="1810"></a>18.1.0

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2118506)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2118509)  

Выпущено: Июль 2018 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118509&2118509=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40a)  

### <a name="features-added-in-1810"></a>Возможности, добавленные в 18.1.0

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка ключевого слова строки подключения `UseFMTONLY`, а также свойства инициализации `SSPROP_INIT_USEFMTONLY` | `UseFMTONLY` определяет способ получения метаданных при подключении к [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более новым версиям.<br/><br/>Дополнительные сведения см. в разделе: [Использование ключевых слов строки подключения с OLE DB Driver for SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed-in-1810"></a>Ошибки, исправленные в 18.1.0

| Исправление ошибки | Сведения |
| :-------- | :------ |
| Исправлена неверная версия файла форматирования BCP. | Драйвер OLE DB 18.0 некорректно задает версию файла форматирования BCP как 18.0 вместо 11.0.<br/>Файлы форматирования, создаваемые драйвером OLE DB 18.0, не могут быть прочитаны драйвером OLE DB 18.1.<br/>Если файлы форматирования, созданные в предыдущей версии драйвера, требуется использовать в новом драйвере, вы можете отредактировать файлы и изменить версию на 11.0 вручную. |
| &nbsp; | &nbsp; |

## <a name="1802"></a>18.0.2

![скачать](../../ssms/media/download-icon.png) [Скачать установщик x64](https://go.microsoft.com/fwlink/?linkid=2118504)  
![скачать](../../ssms/media/download-icon.png) [Скачать установщик x86](https://go.microsoft.com/fwlink/?linkid=2118277)  

Выпущено: Март 2018 г.

Если необходимо загрузить установщик на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера x64: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40a)  
Для драйвера x86: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40a)  

### <a name="features-added-in-1802"></a>Возможности, добавленные в 18.0.2

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка ключевого слова `MultiSubnetFailover` для строки подключения, а также свойства инициализации `SSPROP_INIT_MULTISUBNETFAILOVER`. | Дополнительные сведения см. в разделе:<br/>&bull; &nbsp; [Поддержка высокого уровня доступности и аварийного восстановления в OLE DB Driver for SQL Server](features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).<br/>&bull;&nbsp;[Использование ключевых слов строки подключения с OLE DB Driver for SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>См. также раздел

[Драйвер Microsoft OLE DB для SQL Server](oledb-driver-for-sql-server.md)
