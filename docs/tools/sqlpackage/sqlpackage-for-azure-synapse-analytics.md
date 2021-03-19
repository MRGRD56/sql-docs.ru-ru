---
title: SqlPackage для Azure Synapse Analytics
description: Советы по использованию SqlPackage в сценариях Azure Synapse Analytics
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 500d946c036041481094292720744e7896d64622
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611025"
---
# <a name="sqlpackage-for-azure-synapse-analytics"></a>SqlPackage для Azure Synapse Analytics

Эта статья описывает функции SqlPackage.exe, ориентированные на базы данных Azure Synapse Analytics.

## <a name="extract"></a>Extract
Чтобы [извлечь](sqlpackage-extract.md) схему в Хранилище BLOB-объектов Azure, требуются следующие свойства.
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageKey

Авторизация доступа для извлечения осуществляется с помощью ключа учетной записи хранения.  Дополнительный параметр является необязательным и задает корневой путь хранилища в контейнере:
- /p:AzureStorageRootPath

Без этого свойства путь по умолчанию имеет значение `servername/databasename/timestamp/`.

## <a name="publish"></a>Публикация
Чтобы [опубликовать](sqlpackage-publish.md) схему из DACPAC в Хранилище BLOB-объектов Azure, требуются следующие свойства.
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageRootPath
- /p:AzureStorageKey или /p:AzureSharedAccessSignatureToken

Доступ к публикации можно авторизовать с помощью ключа учетной записи хранения или маркера подписанного URL-адреса (SAS).

## <a name="next-steps"></a>Next Steps
- Дополнительные сведения об [Extract](sqlpackage-extract.md)
- Дополнительные сведения о [Publish](sqlpackage-publish.md)
- Дополнительные сведения о [хранилище BLOB-объектов Azure](/azure/storage/blobs/storage-blobs-introduction)
- Дополнительные сведения о [ключах учетных записей хранения Azure](/azure/storage/common/storage-account-keys-manage)