---
title: Скачивание CAB-файлов обновлений для автономной установки
description: Скачивание CAB-файлов для Служб машинного обучения SQL Server (Python и R) Эти CAB-файлы содержат обновления компонента "Службы машинного обучения" (Python и R) и используются при установке SQL Server на сервере без доступа к Интернету.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/25/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6268ceaae37016bae72a488170408312d25bf536
ms.sourcegitcommit: 35cc65808d2a1769dea65c6ce7f9c2f69640968b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105568930"
---
# <a name="cab-downloads-for-offline-installation-of-cumulative-updates-for-sql-server-machine-learning-services"></a>Скачивание CAB-файлов с накопительными обновлениями Служб машинного обучения SQL Server для автономной установки

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

::: moniker range=">=sql-server-2017"
Скачивание CAB-файлов для Служб машинного обучения SQL Server (Python и R) Эти CAB-файлы содержат обновления компонента "Службы машинного обучения" (Python и R) и используются при установке SQL Server на сервере без доступа к Интернету.
::: moniker-end

::: moniker range="=sql-server-2016"
Скачайте CAB-файлы для SQL Server 2016 R Services (Python и R). Эти CAB-файлы содержат обновления для компонента R Services и используются при установке SQL Server на сервере без доступа к Интернету.
::: moniker-end

Ниже приведены ссылки для скачивания CAB-файлов для каждого накопительного обновления. Дополнительные сведения об автономной установке см. в разделе [Установка компонентов машинного обучения SQL Server без доступа к Интернету](sql-ml-component-install-without-internet-access.md#apply-cu).

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-2017"
Начните с базовой установки. В Службах машинного обучения SQL Server базовой установкой является начальный выпуск. 
::: moniker-end

::: moniker range="=sql-server-2016"
Начните с базовой установки.  В службах R SQL Server 2016 можно сначала установить начальный выпуск с пакетом обновления 1 (SP1) или с пакетом обновления 2 (SP2). 
::: moniker-end

Вы также можете применить накопительные пакеты обновления.

::: moniker range="=sql-server-ver15"

## <a name="sql-server-2019-cabs"></a>CAB-файлы для SQL Server 2019

CAB-файлы перечислены в обратном хронологическом порядке. Скачав CAB-файлы, перенесите их на целевой компьютер, поместив в удобную папку, например, в папку  **Downloads** или в папку %temp% пользователя, который выполняет установку.

|Release | Компонент | Ссылка на скачивание | Устраненные проблемы |
|------- | --------- | ------------- | ---------------- |
|**[SQL Server 2019 CU8](https://support.microsoft.com/help/4577194)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.777_1033.cab](https://go.microsoft.com/fwlink/?linkid=2134897)  |  |
| | R Server              | [SRS_9.4.7.958_1033.cab](https://go.microsoft.com/fwlink/?linkid=2136942)  |  |
| | Microsoft Python Open | [SPO_4.5.12.479_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118341) |  |
| | Python Server         | [SPS_9.4.7.958_1033.cab](https://go.microsoft.com/fwlink/?linkid=2136731)  |  |
|**[SQL Server 2019 CU5](https://support.microsoft.com/help/4552255)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.293_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118178)  |  |
| | R Server              | [SRS_9.4.7.804_1033.cab](https://go.microsoft.com/fwlink/?linkid=2122004)  |  |
| | Microsoft Python Open | [SPO_4.5.12.479_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118341) |  |
| | Python Server         | [SPS_9.4.7.804_1033.cab](https://go.microsoft.com/fwlink/?linkid=2121809)  |  |
|**[SQL Server 2019 CU3](https://support.microsoft.com/help/4538853)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.293_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118178)  |  |
| | R Server              | [SRS_9.4.7.717_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118177)  |  |
| | Microsoft Python Open | [SPO_4.5.12.479_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118341) |  |
| | Python Server         | [SPS_9.4.7.717_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118342)  |  |
|**[SQL Server 2019 CU2](https://support.microsoft.com/help/4536075)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.125_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2085686)  |  |
| | R Server              | [SRS_9.4.7.35_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2113250)   |  |
| | Microsoft Python Open | [SPO_4.5.12.692_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2113303) |  |
| | Python Server         | [SPS_9.4.7.35_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2113067)   |  |
|**[SQL Server 2019 CU1](https://support.microsoft.com/help/4527376)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.125_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2085686)  |  |
| | R Server              | [SRS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2085792)   |  |
| | Microsoft Python Open | [SPO_4.5.12.120_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2085793) |  |
| | Python Server         | [SPS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2085685)   |  |
|**Начальный выпуск** | | | |
| | Microsoft R Open       | [SRO_3.5.2.125_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085686)  |  |
| | R Server               | [SRS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085792)|   |  |
| | Microsoft Python Open  | [SPO_4.5.12.120_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085793) |  |
| | Python Server          | [SPS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085685)   |  |

::: moniker-end

::: moniker range="=sql-server-2017"

## <a name="sql-server-2017-cabs"></a>CAB-файлы для SQL Server 2017

CAB-файлы перечислены в обратном хронологическом порядке. Скачав CAB-файлы, перенесите их на целевой компьютер, поместив в удобную папку, например, в папку  **Downloads** или в папку %temp% пользователя, который выполняет установку.

|Release  |Компонент | Ссылка на скачивание  | Устраненные проблемы | 
|---------|----------|----------------|------------------|
|**[SQL Server 2017 CU22](https://support.microsoft.com/help/4577467/)-[CU23](https://support.microsoft.com/help/5000685/)** |  |  |  |
| | Microsoft R Open      | [SRO_3.5.2.777_1033.cab](https://go.microsoft.com/fwlink/?linkid=2134897)  |  |
| | R Server              | [SRS_9.4.7.958_1033.cab](https://go.microsoft.com/fwlink/?linkid=2136942)  |  |
| | Microsoft Python Open | [SPO_4.5.12.479_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2118341) |  |
| | Python Server         | [SPS_9.4.7.958_1033.cab](https://go.microsoft.com/fwlink/?linkid=2136731)  |  |
|**[SQL Server 2017 CU19](https://support.microsoft.com/help/4535007/)-[CU20](https://support.microsoft.com/help/4541283/)** |  |  |  |
| | Microsoft R Open | [SRO_3.3.3.1900_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2106367&clcid=1033) | Исправление ошибки, из-за которой при выполнении `sp_execute_external_script` скрипта R отображалось сообщение с предупреждением |
| | R Server| [SRS_9.2.0.1900_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2106460&clcid=1033) | Нет изменений после предыдущих версий. |
| | Microsoft Python Open | [SPO_9.2.0.1400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2073897&clcid=1033) | Нет изменений после предыдущих версий. |
| | Python Server | [SPS_9.2.0.1900_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2106459&clcid=1033) | Исправлена ошибка, из-за которой при выполнении `sp_execute_external_script` скрипта Python иногда терялись данные при возврате типа данных varbinary или binary обратно в SQL Server в форме OutputDataSet. |
|**[SQL Server 2017 CU14](https://support.microsoft.com/help/4484710/)-[CU15](https://support.microsoft.com/help/4498951/)-[CU16](https://support.microsoft.com/help/4508218/)-[CU17](https://support.microsoft.com/help/4515579/)-[CU18](https://support.microsoft.com/help/4527377/)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.1400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2073898&clcid=1033)| Двоичные файлы в пакете теперь подписаны. |
| | R Server      |[SRS_9.2.0.1400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2069739&clcid=1033)| Двоичные файлы в пакете теперь подписаны. |
| | Microsoft Python Open     | [SPO_9.2.0.1400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2073897&clcid=1033)| Двоичные файлы в пакете теперь подписаны. |
| | Python Server    |[SPS_9.2.0.1400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2071421&clcid=1033)| Двоичные файлы в пакете теперь подписаны.  |
|**[SQL Server 2017 CU13](https://support.microsoft.com/help/4466404)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.1300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.1300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2038263&clcid=1033)| Содержит исправление для обновления [введенного в эксплуатацию отдельного сервера R](/machine-learning-server/what-is-operationalization), установленного с помощью программы установки SQL Server. Используйте CAB-файлы CU13 и следуйте [этим инструкциям](sql-machine-learning-standalone-windows-install.md#apply-cu), чтобы применить обновление. |
| | Microsoft Python Open     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.1300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2038197&clcid=1033)| Содержит исправление для обновления [введенного в эксплуатацию отдельного сервера Python](/machine-learning-server/what-is-operationalization), установленного с помощью программы установки SQL Server. Используйте CAB-файлы CU13 и следуйте [этим инструкциям](sql-machine-learning-standalone-windows-install.md#apply-cu), чтобы применить обновление. |
|**[SQL Server 2017 CU10](https://support.microsoft.com/help/4342123)-[CU11](https://support.microsoft.com/help/4462262)-[CU12](https://support.microsoft.com/help/4464082)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.1000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2006287&clcid=1033)| Небольшие исправления.|
| | Microsoft Python Open     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.1000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2006805&clcid=1033)| При удалении повторяющихся элементов rx_data_step Python теряет порядок строк. <br/>SPEE не удается определить тип данных в кластеризованном индексе columnstore. <br/>Возвращается пустая таблица, если все столбцы содержат значения NULL. |
|**[SQL Server 2017 CU8](https://support.microsoft.com/help/4338363)-[CU9](https://support.microsoft.com/help/4341265)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.800_1033.cab](https://go.microsoft.com/fwlink/?LinkId=874708&clcid=1033)|
| | Microsoft Python Open     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.800_1033.cab](https://go.microsoft.com/fwlink/?LinkId=874707&clcid=1033)|
|**[SQL Server 2017 CU6](https://support.microsoft.com/help/4101464)-[CU7](https://support.microsoft.com/help/4229789)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871074&clcid=1033)|
| | Microsoft Python Open     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871073&clcid=1033)| Типы данных DateTime в запросе SPEES.<br/>Улучшены сообщения об ошибках в microsoftml при отсутствии предварительно обученных моделей.<br/> Исправления для переменных и функций преобразования revoscalepy.|
|**[SQL Server 2017 CU5](https://support.microsoft.com/help/4092643)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869052&clcid=1033)| Ошибки, связанные с длинными путями в rxInstallPackages.<br/>Подключения в замыкании на себя для RxExec.
| | Microsoft Python Open    | Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869053&clcid=1033)| <br/>Подключения в замыкании на себя для rx_exec.
|**[SQL Server 2017 CU4](https://support.microsoft.com/help/4056498)** |  |   |  |
| | Microsoft R Open     | [SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866212&clcid=1033)|
| | Microsoft Python Open     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| |  Python Server    |[SPS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866213&clcid=1033)|
|**[SQL Server 2017 CU3](https://support.microsoft.com/help/4052987)** |  |  |  |
| | Microsoft R Open     |[SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)|
| | R Server      |[SRS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863893)|
| | Microsoft Python Open     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. |
| | Python Server    |[SPS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863892)| Сериализация модели Python в revoscalepy с использованием [функции rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model).<br/>Поддержка [собственной оценки](../predictions/native-scoring-predict-transact-sql.md) и усовершенствования [оценки в реальном времени](../predictions/real-time-scoring.md). 
|**[SQL Server 2017 CU1](https://support.microsoft.com/help/4038634)-[CU2](https://support.microsoft.com/help/4052574)** |  |  |  |
| | Microsoft R Open     | [SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)| Нет изменений после предыдущих версий. |
| | R Server      |[SRS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851501)|
| | Microsoft Python Open     | [SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502)| Нет изменений после предыдущих версий. | 
| | Python Server    |[SPS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851500) | Добавление rx_create_col_info для возврата информации о схеме. <br/>Улучшения [rx_exec](/machine-learning-server/python-reference/revoscalepy/rx-exec) для поддержки параллельных сценариев с использованием контекста вычислений `RxLocalParallel`.|
|**Начальный выпуск** |  |  |
| | Microsoft R Open     |[SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)|
| | R Server      |[SRS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851507)|
| | Microsoft Python Open     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502) |
| | Python Server    |[SPS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851508) |

::: moniker-end

::: moniker range="=sql-server-2016"

<a name="bkmk_2016Installers"></a>

## <a name="sql-server-2016-cabs"></a>CAB-файлы для SQL Server 2016

Для служб R SQL Server 2016 базовые выпуски — это либо версия RTM, либо версия пакета обновления.

|Release  |Ссылка на скачивание  |
|---------|---------------|
|**SQL Server 2016 с пакетом обновления 2 (SP2) CU14–CU15**     |
|Microsoft R Open      |[SRO_3.5.2.777_1033.cab](https://go.microsoft.com/fwlink/?linkid=2134897)|
|Microsoft R Server    |[SRS_9.4.7.958_1033.cab](https://go.microsoft.com/fwlink/?linkid=2136942)|
|**SQL Server 2016 с пакетом обновления 2 (SP2) CU6–CU13**     |
|Microsoft R Open     |[SRO_3.2.2.20100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2079936&clcid=1033)|
|Microsoft R Server    |[SRS_8.0.3.20100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2079933&clcid=1033)|
|**SQL Server 2016 SP2 CU1-CU5**     |
|Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
|Microsoft R Server    |[SRS_8.0.3.20000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866038)|
|**SQL Server 2016 SP2**     |
|Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
|Microsoft R Server    |[SRS_8.0.3.17000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=850317)|
|**SQL Server 2016 SP1 CU14**     |
|Microsoft R Open     |[SRO_3.2.2.16100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2080130&clcid=1033)|
|Microsoft R Server    |[SRS_8.0.3.17200_1033.cab](https://go.microsoft.com/fwlink/?LinkId=2079935&clcid=1033)|
|**SQL Server 2016 SP1 CU1-CU13**     |
|Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
|Microsoft R Server    |[SRS_8.0.3.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836818)|
|**SQL Server 2016 SP1**     |
|Microsoft R Open     |[SRO_3.2.2.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824879)|
|Microsoft R Server     |[SRS_8.0.3.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824881)|
|**SQL Server 2016 CU4-CU9**     |
|Microsoft R Open     |[SRO_3.2.2.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831785)|
|Microsoft R Server     |[SRS_8.0.3.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831676)|
|**SQL Server 2016 CU2-CU3**     |
|Microsoft R Open     |[SRO_3.2.2.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827398)|
|Microsoft R Server     |[SRS_8.0.3.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827399)|
|**SQL Server 2016 CU1**     |
|Microsoft R Open     |[SRO_3.2.2.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808803)|
|Microsoft R Server     |[SRS_8.0.3.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808805)|
|**SQL Server 2016 RTM**     |
|Microsoft R Open     |[SRO_3.2.2.803_1033.cab](https://go.microsoft.com/fwlink/?LinkId=761266)|
|Microsoft R Server     |[SRS_8.0.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=735051)|

> [!NOTE]
> 
> При установке SQL Server 2016 с пакетом обновления 1 (SP1) и накопительным обновлением 4 (CU4) или 5 (CU5) скачайте файл SRO_3.2.2.16000_1033.cab. Если вы скачали SRO_3.2.2.13000_1033.cab из FWLINK 831785, как указано в диалоговом окне программы установки, перед установкой этого накопительного обновления переименуйте данный файл в SRO_3.2.2.16000_1033.cab.

Если вы хотите просмотреть исходный код для Microsoft R, его можно скачать в виде архива в формате TAR: [Скачать установщики R Server](/machine-learning-server/install/r-server-install-windows#download)

::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

[Применение накопительных обновлений на компьютерах без доступа к Интернету](sql-ml-component-install-without-internet-access.md#apply-cu)

[Применение накопительных обновлений на компьютерах с подключением к Интернету](sql-ml-component-install-without-internet-access.md#apply-cu)

[Применение накопительных обновлений на отдельном сервере](sql-machine-learning-standalone-windows-install.md#apply-cu)
