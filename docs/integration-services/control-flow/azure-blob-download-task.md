---
description: Задача скачивания BLOB-объектов Azure
title: Задача скачивания BLOB-объектов Azure | Документы Майкрософт
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21ae3810f5f52ab8d77760cc08a887b74ab65977
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783091"
---
# <a name="azure-blob-download-task"></a>Задача скачивания BLOB-объектов Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Задача скачивания BLOB-объектов Azure позволяет пакету служб SSIS скачивать файлы из хранилища BLOB-объектов Azure.

Чтобы добавить **задачу скачивания BLOB-объектов Azure**, перетащите ее в конструкторе служб SSIS и дважды щелкните или щелкните правой кнопкой мыши и выберите **Изменить** , чтобы вызвать диалоговое окно **Редактор задач скачивания BLOB-объектов Azure** .  
  
 **Задача скачивания BLOB-объектов Azure** входит в состав [пакета дополнительных компонентов SQL Server Integration Services (SSIS) для Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 Следующая таблица содержит описание полей этого диалогового окна.  

|**Поле**|**Описание**|  
|---|---|
|AzureStorageConnection|Создайте новый или укажите существующий диспетчер подключений службы хранилища Azure, который ссылается на учетную запись хранения Azure, указывающую на место размещения файлов BLOB-объектов.|  
|BlobContainer|Указывает имя контейнера BLOB-объектов, который содержит скачиваемые файлы BLOB-объектов.|  
|BlobDirectory|Указывает каталог больших двоичных объектов, который содержит скачиваемые файлы BLOB-объектов. Каталог больших двоичных объектов — это виртуальная иерархическая структура.|  
|SearchRecursively|Указывает, следует ли выполнять рекурсивный поиск в подкаталогах.|  
|LocalDirectory|Указывает локальный каталог для хранения скачанных файлов BLOB-объектов.|  
|FileName|Указывает имя фильтра для выбора файлов с указанным шаблоном имени. Например, `MySheet*.xls\*` включает такие файлы, как `MySheet001.xls` и `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Задает фильтр диапазона времени. Включаются файлы, измененные после **TimeRangeFrom** и до **TimeRangeTo**.|  
