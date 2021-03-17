---
title: Скачивание драйвера Microsoft OLE DB для SQL Server | Документация Майкрософт
description: Скачайте драйвер Microsoft ODBC Driver for SQL Server, чтобы разрабатывать Windows-приложения с подключением к SQL Server и базе данных SQL Azure.
ms.date: 12/01/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0e5e8a8b084c6762e732c0b866a0e4866e9d2707
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575332"
---
# <a name="download-microsoft-ole-db-driver-for-sql-server"></a>Скачать драйвер Microsoft OLE DB для SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Драйвер OLE DB для SQL Server — это изолированный прикладной программный интерфейс (API) для доступа к данным, используемый в OLE DB. Драйвер OLE DB для SQL Server есть в Windows и предоставляется в формате одиночной библиотеки динамической компоновки (DLL).

## <a name="download"></a>Скачивание

Распространяемый установщик Microsoft OLE DB Driver for SQL Server устанавливает клиентские компоненты, необходимые во время выполнения, чтобы воспользоваться преимуществами функциями нового SQL Server. Начиная с версии 18.3, установщик также включает и устанавливает библиотеку проверки подлинности Microsoft Active Directory (ADAL.dll).

Microsoft OLE DB Driver 18.5 for SQL Server — это последняя общедоступная версия. Если у вас установлена предыдущая версия, Microsoft OLE DB Driver 18 for SQL Server, то при установке версии 18.5 она обновляется до версии 18.5.

**[![Скачать](../../ssms/media/download-icon.png) Скачать драйвер Microsoft OLE DB для SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2135577)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать драйвер Microsoft OLE DB для SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2135722)**  

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 18.5.0
- Выпущено: 1 декабря 2020 г.

> [!Note]
> Если вы открываете локализованную версию этой страницы и хотите просмотреть актуальные материалы, посетите эту страницу на [версии сайта на языке US-English](https://aka.ms/downloadmsoledbsqlusenglish). С версии сайта US-English вы можете скачать SSMS на других [языках из числа доступных](#available-languages).

## <a name="available-languages"></a>Доступные языки

Этот выпуск драйвера Microsoft OLE DB для SQL Server можно установить на следующих языках:

Microsoft OLE DB Driver 18.5 for SQL Server (x64):  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)

Microsoft OLE DB Driver 18.5 for SQL Server (x86):  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)

## <a name="release-notes"></a>Заметки о выпуске

Дополнительные сведения об этом выпуске см. в [заметках о выпуске](release-notes-for-oledb-driver-for-sql-server.md).

## <a name="previous-releases"></a>Предыдущие выпуски

[Предыдущие выпуски драйвера OLE DB для SQL Server](release-notes-for-oledb-driver-for-sql-server.md#previous-releases)

## <a name="see-also"></a>См. также раздел

[Заметки о выпуске Microsoft OLE DB Driver for SQL Server](release-notes-for-oledb-driver-for-sql-server.md)  
[Требования к системе для драйвера OLE DB для SQL Server](system-requirements-for-oledb-driver-for-sql-server.md)  
[Политики поддержки для драйвера OLE DB для SQL Server](applications\support-policies-for-oledb-driver-for-sql-server.md)  
[Когда использовать драйвер OLE DB для SQL Server](when-to-use-oledb-driver-for-sql-server.md)  
[Установка драйвера OLE DB для SQL Server](applications/installing-oledb-driver-for-sql-server.md)
