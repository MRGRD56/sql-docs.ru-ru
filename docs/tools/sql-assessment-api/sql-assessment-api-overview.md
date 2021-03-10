---
title: API оценки SQL Server
description: Сведения об интерфейсе API Оценки SQL, который предоставляет механизм вычисления конфигурации SQL Server для получения рекомендаций.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 3/5/2021
ms.openlocfilehash: 37e31834df74bf91fcf31004973c1556b8f34552
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247469"
---
# <a name="sql-assessment-api"></a>API Оценки SQL

API-интерфейс Оценки SQL предоставляет механизм для оценки конфигурации SQL Server на соответствие рекомендациям. API поставляется с набором правил, содержащим рекомендации, предлагаемые командой разработчиков SQL Server. Этот набор правил совершенствуется по мере выпуска новых версий, но в то же время API реализован как решение с широкими возможностями настройки и расширения. Таким образом, пользователи могут настраивать правила по умолчанию и создавать собственные.

API Оценки SQL приходит на помощь, когда вам нужно удостоверить соответствие конфигурации SQL Server рекомендуемым лучшим методикам. После первоначальной оценки стабильность конфигурации отслеживается при помощи регулярных запланированных оценок.

API можно использовать для оценки:
 
* SQL Server на Виртуальных машинах Microsoft Azure

* Управляемый экземпляр базы данных SQL Azure

* SQL Server 2012 и более поздних версий

* SQL в системах на базе Linux

API также используется расширением Оценки SQL Server для Azure Data Studio (ADS).

>[!NOTE]
>API-интерфейс Оценки SQL обеспечивает анализ разнообразных аспектов, но не дает глубокой оценки безопасности. Чтобы с упреждением совершенствовать защиту ваших баз данных, рекомендуем воспользоваться [Оценкой уязвимостей SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment).

## <a name="rules"></a>Правила

Правила, иногда называемые проверками, определяются в файлах в формате JSON. Для набора правил требуется указать имя и версию набора правил. При использовании пользовательских наборов правил можно легко узнать, какие рекомендации исходят из какого набора правил.

Предоставляемый Майкрософт набор правил доступен на GitHub. Просмотреть [весь набор правил](https://github.com/microsoft/sql-server-samples/blob/567d49a42d4cf10e4942b19290ab80828b451b77/samples/manage/sql-assessment-api/DefaultRuleset.csv) можно в [репозитории примеров](https://aka.ms/sql-assessment-api).

## <a name="sql-assessment-cmdlets-and-associated-extensions"></a>Командлеты Оценки SQL и связанные расширения

API Оценки SQL является частью:

* [Azure Data Studio (ADS)](../../azure-data-studio/what-is-azure-data-studio.md)

    версии выпуска от июня 2020 г. и выше;

* [Управляющие объекты SQL Server (SMO)](../../relational-databases/server-management-objects-smo/installing-smo.md)

    версии выпуска от июля 2019 г. и выше;

* [модуля SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md);

    версии выпуска от июля 2019 г. и выше.

Прежде чем приступить к работе с API Оценки SQL, убедитесь в том, что выполнено следующее:

* [Установка ADS](https://techcommunity.microsoft.com/t5/sql-server/released-sql-server-assessment-extension-for-azure-data-studio/ba-p/1470603)

* [Установка объектов SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

* [Установка модуля SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md)

Модуль SqlServer получил два новых командлета для работы с API Оценки SQL:

* **Get-SqlAssessmentItem** — предоставляет список доступных проверок оценки для объекта SQL Server;

* **Invoke-SqlAssessment** — предоставляет результаты оценки.

Платформа SMO дополняется расширением для API оценки SQL, которое предоставляет следующие методы:

* **GetAssessmentItems** — возвращает доступные проверки для конкретного объекта SQL (IEnumerable<…>);

* **GetAssessmentResults** — синхронно оценивает оценку и возвращает результаты и ошибки, если таковые имеются (IEnumerable<…>);

* **GetAssessmentResultsList** — асинхронно оценивает оценку и возвращает результаты и ошибки, если таковые имеются (Task<…>).

## <a name="get-started-using-sql-assessment-cmdlets"></a>Начало работы с командлетами оценки SQL

Оценка выполняется для выбранного объекта SQL Server. В наборе правил по умолчанию существуют проверки только двух видов объектов: Server и Database (в дополнение к ним API-интерфейс поддерживает два других типа: Filegroup и AvailabilityGroup). Если вы хотите оценить экземпляр SQL и все его базы данных, следует запускать командлеты оценки SQL для каждого объекта отдельно. Или можно передать объекты для оценки в командлеты оценки SQL в переменной или конвейере.

Объекты SqlServer и RegisteredServer взаимозаменяемы, поэтому можно передавать в командлеты оценки SQL любой из них.

Чтобы приступить к работе, ознакомьтесь с приведенными ниже примерами.

1. Получите список доступных проверок для локального экземпляра по умолчанию, чтобы познакомиться с проверками. В этом примере выходные данные командлета Get-SqlInstance передаются по конвейеру в командлет Get-SqlAssessmentItem, чтобы передать в него объект экземпляра.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

2. Получите список доступных проверок для всех баз данных экземпляра. Здесь мы используем командлет Get-Item и путь, реализованный в поставщике SQL Server для Windows PowerShell, чтобы получить список баз данных, а затем передать его по конвейеру в командлет Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Кроме того, для этих же целей можно использовать командлет Get-SqlDatabase.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

3. Получите список доступных проверок для всех баз данных экземпляра. Здесь мы используем командлет Get-Item и путь, реализованный в поставщике SQL Server для Windows PowerShell, чтобы получить список баз данных, а затем передать его по конвейеру в командлет Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Кроме того, для этих же целей можно использовать командлет Get-SqlDatabase.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

4. Вызовите оценку для экземпляра и сохраните результаты в таблицу SQL. В этом примере выходные данные командлета Get-SqlInstance передаются по конвейеру в командлет Invoke-SqlAssessment, результаты которого передаются в командлет Write-SqlTableData. Командлет Invoke-Assessment в этом примере выполняется с параметром `-FlattenOutput`. Этот параметр делает выходные данные подходящими для командлета Write-SqlTableData. Если опустить этот параметр, то в последнем случае будет выдано сообщение об ошибке.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

    Теперь давайте вызовем оценку всех баз данных экземпляра и добавим результаты в ту же таблицу.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

5. Для дальнейшего анализа рекомендаций следуйте описанию и ссылкам в таблице.

6. Настройте правила в соответствии со средой и требованиями организации (см. ниже).

7. Запланируйте задачу или задание для регулярного выполнения оценки или оценки по запросу для измерения хода выполнения.

## <a name="customizing-rules"></a>Настройка правил

Правила предназначены для повышения возможностей настройки и расширения. Набор правил Майкрософт подходит для работы в большинстве сред. Однако невозможно иметь один набор правил, который будет работать для каждой отдельной среды. Пользователи могут создавать собственные файлы JSON и настраивать существующие правила или добавлять новые. Примеры настройки и полный набор правил Майкрософт доступны в [репозитории примеров](https://aka.ms/sql-assessment-api). Дополнительные сведения о запуске командлетов оценки SQL с пользовательскими файлами JSON можно получить с помощью командлета Get-Help.

### <a name="options-available-with-rule-customization-feature"></a>Доступные параметры для настройки правил

#### <a name="enablingdisabling-certain-rules-or-groups-of-rules-using-tags"></a>Включение и отключение определенных правил или групп правил (с помощью тегов)

Вы можете отключить сообщения отдельных правил, если они не применяются к среде или пока не будут выполнены запланированные действия по устранению проблемы.

#### <a name="changing-threshold-parameters"></a>Изменение пороговых значений

Отдельные правила имеют пороговые значения, которые сравниваются с текущим значением метрики для выявления проблем. Если пороговые значения по умолчанию не подходят, их можно изменить.

#### <a name="adding-more-rules-written-by-you-or-third-parties"></a>Добавление правил, написанных вами или сторонними разработчиками

Вы можете объединить наборы правил, добавив один или несколько JSON-файлов в качестве параметров вызова API оценки SQL. Ваша организация может написать эти файлы самостоятельно или получить их от третьей стороны. Например, можно использовать файл JSON, который отключает определенные правила из набора Майкрософт, затем файл JSON, написанный экспертом отрасли, который будет включать правила, полезные для вашей среды, и, наконец, третий файл JSON, который будет изменять некоторые пороговые значения второго файла.

>[!IMPORTANT]
>Мы настоятельно рекомендуем не использовать наборы правил из ненадежных источников, пока тщательно не проверите их и не убедитесь в их безопасности.

## <a name="next-steps"></a>Дальнейшие действия

* [Управляющие объекты SQL Server (SMO)](../../relational-databases/server-management-objects-smo/overview-smo.md)
* [PowerShell](../../powershell/download-sql-server-ps-module.md)
* [Оценка уязвимостей SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment)
