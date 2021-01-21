---
description: Смена ключей с поддержкой анклава
title: Смена | Документация Майкрософт
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 8ef5e2e3cbf4e00619aeb4b0d1643f0d07100aad
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534311"
---
# <a name="rotate-enclave-enabled-keys"></a>Смена ключей с поддержкой анклава

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Смена ключа в Always Encrypted — это процесс замены существующего главного ключа столбца или ключа шифрования столбца новым ключом. В этой статье описываются варианты использования и рассматриваются вопросы, связанные со сменой ключей для [Always Encrypted с безопасным анклавом](always-encrypted-enclaves.md), когда в качестве ключа с поддержкой анклава выступает исходный и (или) целевой (новый) ключ. Общие рекомендации и процессы управления ключами Always Encrypted см. в статье [Общие сведения об управлении ключами для Always Encrypted](overview-of-key-management-for-always-encrypted.md). 

Смена ключа может потребоваться в целях обеспечения безопасности или соответствия требованиям. Например, смена ключа необходима в случае его компрометации или периодическая замена ключей выполняется в соответствии с политиками организации. Кроме того, смена ключей в Always Encrypted с безопасными анклавами позволяет включать или отключать функциональность серверного безопасного анклава для зашифрованных столбцов.

- При замене ключа без поддержки анклава на ключ с поддержкой анклава происходит разблокировка функциональных возможностей безопасного анклава для выполнения запросов к столбцам, защищенным с помощью ключа. Дополнительные сведения см. в статье [Включение Always Encrypted с безопасными анклавами для существующих зашифрованных столбцов](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- При замене ключа с поддержкой анклава на ключ без поддержки анклава происходит отключение функциональных возможностей безопасного анклава для выполнения запросов к столбцам, защищенным с помощью ключа.

Если смена ключа осуществляется только в целях обеспечения безопасности или соответствия требованиям без включения или отключения вычислений анклава для столбцов, целевой ключ должен иметь ту же конфигурацию анклавов, что и исходный ключ. Например, если исходный ключ поддерживает анклав, целевой ключ также должен поддерживать анклав.

В зависимости от сценария смены ключей выполните приведенные ниже действия, перейдя по ссылкам на статьи с подробными инструкциями.

1. Подготовьте новый ключ (главный ключ столбца или ключ шифрования столбца).
    - Сведения о подготовке нового ключа с поддержкой анклава см. в статье [Подготовка ключей с поддержкой анклава](always-encrypted-enclaves-provision-keys.md).
    - Сведения о подготовке ключа без поддержки в анклава см. в статьях [Подготовка ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md) и [Подготовка ключей Always Encrypted с помощью PowerShell](configure-always-encrypted-keys-using-powershell.md).
2. Замените существующий ключ новым ключом.
    - Если при смене ключа шифрования столбца исходный и целевой ключи поддерживают анклав, процесс смены (который предполагает повторное шифрование данных) можно выполнить на месте. Дополнительные сведения см. в статье [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md).
    - Подробные инструкции по смене ключей см. в статьях [Смена ключей Always Encrypted с помощью SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) и [Смена ключей Always Encrypted с помощью PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Next Steps

- [Выполнение инструкций Transact-SQL с помощью безопасных анклавов](always-encrypted-enclaves-query-columns.md)
- [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md)
- [Включение Always Encrypted с безопасными анклавами для существующих зашифрованных столбцов](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Разработка приложений с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-client-development.md)  

## <a name="see-also"></a>См. также:  
- [Управление ключами для Always Encrypted с безопасными анклавами](always-encrypted-enclaves-manage-keys.md)