---
title: Обнаружение и классификация данных в SqlClient
description: Сведения о том, как проверить, поддерживает ли база данных SQL Server классификацию данных, и о том, как получить доступ к сведениям о классификации данных с помощью объекта SqlDataReader.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: bac8c09b862e43549ade3a6d98e34fdc97596131
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835771"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>Обнаружение и классификация данных в SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Набор функций [обнаружения и классификации данных SQL](../../../relational-databases/security/sql-data-discovery-and-classification.md) служит для обнаружения конфиденциальных данных в базах данных, их классификации, назначения им меток и создания отчетов. SqlClient включает API, предоставляющий сведения об обнаружении и классификации данных только для чтения, если базовый источник поддерживает эту функцию. Доступ к этим сведениям осуществляется через SqlDataReader.

Microsoft.Data.SqlClient v2.1.0 добавляет поддержку информации `Sensitivity Rank` согласно Классификации данных. `Sensitivity Rank` — это идентификатор, основанный на предварительно заданном наборе значений, определяющих ранг конфиденциальности. Он может использоваться другими службами, например службой "Расширенная защита от угроз", для обнаружения аномалий в зависимости от их ранга. Следующие API Классификации данных теперь доступны в пространстве имен Microsoft.Data.SqlClient.DataClassification:

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> Microsoft.Data.SqlClient считывает информацию `Sensitivity Rank` только в том случае, если SQL Server поддерживает Классификацию данных с ранжированием. Для серверов используется старая версия Классификации данных без ранжирования, и запросы возвращают значение ранга NOT DEFINED.

В этом примере приложения показано, как получить доступ к свойствам классификации данных SqlDataReader.

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**См. также:**  

 - [Функции SQL Server и ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [ADD SENSITIVITY CLASSIFICATION](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)