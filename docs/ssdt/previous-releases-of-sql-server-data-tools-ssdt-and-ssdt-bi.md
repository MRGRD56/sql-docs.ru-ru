---
title: Предыдущие выпуски SQL Server Data Tools (SSDT)
description: Узнайте, какие версии SSDT и SSDT-BI работают с какими версиями Visual Studio. Узнайте, как установить различные версии SSDT и SSDT-BI.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 3e2a3e607b561ede827652f61032b90fc7efa336
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353992"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>Предыдущие выпуски SQL Server Data Tools (SSDT и SSDT-BI)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) предоставляет шаблоны проектов и рабочих областей конструирования для создания различных типов содержимого SQL Server: реляционных баз данных, моделей Analysis Services, отчетов Reporting Services и пакетов Integration Services.

SSDT обладает обратной совместимостью. Это означает, что вы всегда можете использовать [новейшую версию SSDT](download-sql-server-data-tools-ssdt.md) для разработки и развертывания баз данных, моделей, отчетов и пакетов, которые будут работать и в более ранних версиях SQL Server.

Исторически оболочка Visual Studio, которая используется для создания типов содержимого SQL Server, выпускалась под разными именами, в том числе **SQL Server Data Tools**, **SQL Server Data Tools — Business Intelligence** и **Business Intelligence Development Studio**. Предыдущие версии поставляются с отдельными наборами шаблонов проектов. Чтобы получить все шаблоны проектов в одном SSDT, необходима [новейшая версия](download-sql-server-data-tools-ssdt.md). В противном случае вам, возможно, потребуется установить несколько предыдущих версий, чтобы получить все шаблоны, использованные в SQL Server. На каждую версию Visual Studio устанавливается только одна оболочка, при установке второго SSDT просто добавляются отсутствующие шаблоны.

## <a name="previous-ssdt-releases"></a>Предыдущие выпуски SSDT

Чтобы скачать предыдущую версию SSDT, щелкните ссылку для скачивания в соответствующем разделе.

| Версия SSDT | Версия Visual Studio |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT для Visual Studio (VS) 2017

**[Скачайте SSDT для Visual Studio 2017 (15.8)](https://go.microsoft.com/fwlink/?linkid=2124319)**

Этот выпуск **SSDT для Visual Studio 2017** можно установить на следующих языках:

[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT для Visual Studio (VS) 2015

Чтобы установить эту версию SSDT, необходимо скачать ISO-образ. ISO-образ представляет собой автономный файл, который содержит все компоненты, необходимые для SSDT, и его можно скачать с помощью диспетчера возобновляемой загрузки, который требуется при ограниченной или менее надежной пропускной способности сети. После скачивания ISO-образ можно подключить как диск.

Шаги по установке:

1. **[Скачайте SSDT для Visual Studio 2015 (17.4)](https://go.microsoft.com/fwlink/?linkid=2132817)** .

2. Откройте ISO-образ.

3. Запустите файл *SSDTSetup.exe*.

    ![ISO-образ](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Этот выпуск **SSDT для Visual Studio 2015** можно установить на следующих языках:

| Язык | Хэш SHA256 |
|----------|-------------|
| [Китайский (упрощенный)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Китайский (традиционный)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Французский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Немецкий](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Итальянский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Японский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Корейский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Русский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Испанский](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT для Visual Studio (VS) 2013

Чтобы установить эту версию SSDT, необходимо скачать ISO-образ. ISO-образ представляет собой автономный файл, который содержит все компоненты, необходимые для SSDT, и его можно скачать с помощью диспетчера возобновляемой загрузки, который требуется при ограниченной или менее надежной пропускной способности сети. После скачивания ISO-образ можно подключить как диск.

Шаги по установке:

1. **[Скачайте SSDT для Visual Studio 2013 (16.5)](https://go.microsoft.com/fwlink/?linkid=832312)**

2. Откройте ISO-образ.

3. Запустите файл *SSDTSetup.exe*.

    ![ISO-образ](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Этот выпуск **SSDT для Visual Studio 2013** можно установить на следующих языках:

| Язык | Хэш SHA256 |
|----------|-------------|
| [Китайский (упрощенный)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Китайский (традиционный)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Английский (США)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Французский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Немецкий](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Итальянский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Японский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Корейский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Русский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Испанский](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT для Visual Studio (VS) 2012

Чтобы установить эту версию SSDT, необходимо скачать ISO-образ. ISO-образ представляет собой автономный файл, который содержит все компоненты, необходимые для SSDT, и его можно скачать с помощью диспетчера возобновляемой загрузки, который требуется при ограниченной или менее надежной пропускной способности сети. После скачивания ISO-образ можно подключить как диск.

Шаги по установке:

1. **[Скачайте SSDT для Visual Studio 2012 (11.1.50727.1)](https://go.microsoft.com/fwlink/?linkid=518814)**

2. Откройте ISO-образ.

3. Запустите файл *SSDTSetup.exe*.

    ![ISO-образ](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Этот выпуск **SSDT для Visual Studio 2012** можно установить на следующих языках:

| Язык | Хэш SHA256 |
|----------|-------------|
| [Китайский (упрощенный)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [Китайский (традиционный)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [Английский (США)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [Французский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [Немецкий](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [Итальянский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [Японский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [Корейский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [Русский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [Испанский](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT поддерживает две последние версии Visual Studio. После выпуска Visual Studio 2019 версии SSDT для Visual Studio 2015 и более ранних версий больше не обновляются. SSDT для Visual Studio 2010 больше недоступен. Дополнительные сведения см. в разделе *Часто задаваемые вопросы* в [этой записи блога группы разработчиков SSDT](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017).

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI: службы Analysis Services, Reporting Services или Integration Services

Шаблоны бизнес-аналитики используются для создания моделей SSAS, отчетов SSRS и пакетов SSIS. Конструкторы бизнес-аналитики связаны с определенными выпусками SQL Server. Чтобы использовать более новые возможности бизнес-аналитики, установите конструктор бизнес-аналитики актуальной версии.

## <a name="bi-designers"></a>Конструкторы бизнес-аналитики

[Скачать SSDT-BI для Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313): SQL Server 2014, SQL Server 2012, SQL Server 2008 и 2008 R2

[Скачать SSDT-BI для Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843): SQL Server 2014, SQL Server 2012, SQL Server 2008 и 2008 R2

Business Intelligence Development Studio (BIDS) устанавливается с помощью программы установки SQL Server. Установочные файлы SQL Server 2008 и 2008 R2 невозможно скачать через Интернет.

Для SQL Server 2012 или 2014 вы можете использовать **SSDT-BI для Visual Studio 2012** или **SSDT-BI fили Visual Studio 2013**. Единственное различие между ними — версия Visual Studio.

## <a name="next-steps"></a>Дальнейшие действия

- [Скачать SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Заметки о выпуске SQL Server Data Tools (SSDT)](release-notes-ssdt.md)
- [Скачивание SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Скачать Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Средства и программы SQL](../tools/overview-sql-tools.md)