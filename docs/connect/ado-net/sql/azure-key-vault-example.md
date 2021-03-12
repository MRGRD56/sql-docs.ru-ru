---
description: Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted
title: Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 48c9fbbe5afbc5115aa52dbb13d258e6595b6efc
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464877"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

В этом примере демонстрируется использование поставщика Azure Key Vault при доступе к зашифрованным столбцам.

## <a name="azurekeyvaultprovider-v20"></a>AzureKeyVaultProvider версии 2.0 и выше

[!code-csharp [Azure Key Vault Provider 2.0 Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample_2_0.cs#1)]

## <a name="azurekeyvaultprovider-v1x"></a>AzureKeyVaultProvider версии 1.x

[!code-csharp [Azure Key Vault Provider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
>
> - Для использования Always Encrypted без безопасных анклавов для приложения .NET Standard требуется **Microsoft.Data.SqlClient** версии 2.1.0 или более поздней. Поддерживаемая версия .NET Standard — 2.0 или более поздняя.
>
> - Для использования Always Encrypted в Linux и macOS требуется **Microsoft.Data.SqlClient** версии 2.1.0 или более поздней.

## <a name="see-also"></a>См. также раздел

- [Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted с безопасными анклавами](azure-key-vault-enclave-example.md)
- [Руководство. Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Использование Always Encrypted с поставщиком данных Microsoft .NET для SQL Server](sqlclient-support-always-encrypted.md)
