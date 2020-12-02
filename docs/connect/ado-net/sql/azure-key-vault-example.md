---
description: Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted
title: Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted | Документация Майкрософт
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 51c99e679855ca762b7adc5763086b4ded9b6cf5
ms.sourcegitcommit: 4c3949f620d09529658a2172d00bfe37aeb1a387
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2020
ms.locfileid: "96123902"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

В этом примере демонстрируется использование поставщика Azure Key Vault при доступе к зашифрованным столбцам.

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - Для использования Always Encrypted без безопасных анклавов для приложения .NET Standard требуется **Microsoft.Data.SqlClient** версии 2.1.0 или более поздней. Поддерживаемая версия .NET Standard — 2.0 или более поздняя. 
>
> - Для использования Always Encrypted в Linux и macOS требуется **Microsoft.Data.SqlClient** версии 2.1.0 или более поздней.

## <a name="see-also"></a>См. также:

- [Example demonstrating use of Azure Key Vault provider with Always Encrypted enabled with Secure Enclaves](azure-key-vault-enclave-example.md) (Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted с безопасными анклавами)
- [Руководство. Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Использование Always Encrypted с поставщиком данных Microsoft .NET для SQL Server](sqlclient-support-always-encrypted.md)
