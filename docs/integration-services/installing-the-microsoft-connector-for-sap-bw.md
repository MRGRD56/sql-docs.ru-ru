---
description: Установка Microsoft Connector для SAP BW
title: Установка соединителя Microsoft Connector для SAP BW | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3bfb9023-9597-4f59-9085-4b9057e7702e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3244ffbd690d7f2ea79862eb1963df60607ac2d6
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384644"
---
# <a name="installing-the-microsoft-connector-for-sap-bw"></a>Установка Microsoft Connector для SAP BW

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector для SAP BW для SQL Server 2016 входит в пакет дополнительных компонентов SQL Server 2016. Чтобы установить Connector для SAP BW и сопутствующую документацию, скачайте и запустите установщик [с веб-страницы пакета дополнительных компонентов SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=56833).  

> [!IMPORTANT]
> Корпорация Майкрософт не планирует предоставлять обновленные версии соединителя для SAP BW. Майкрософт не владеет исходным кодом компонентов SAP BW сторонних разработчиков и не может их обновлять. Вы можете приобрести новейшие компоненты подключения SAP у независимого поставщика программного обеспечения, который является партнером Майкрософт, например у [Theobald Software](https://theobald-software.com/en/xtract-is-productinfo.html). Независимые поставщики программного обеспечения, являющиеся партнерами Майкрософт, адаптировали свои компоненты подключений SAP под SSIS для установки в Azure.

> [!IMPORTANT]  
>  Для работы с документацией по Microsoft Connector для SAP BW вы должны иметь представление о среде SAP Netweaver BW. Дополнительные сведения о SAP Netweaver BW или сведения о настройке объектов и процессов SAP Netweaver BW см. в документации SAP.  
  
> [!IMPORTANT]  
>  Для извлечения данных из SAP Netweaver BW требуется дополнительная лицензия SAP. Обратитесь к SAP, чтобы уточнить требования.  
  
## <a name="required-sap-files"></a>Необходимые файлы SAP  
 Чтобы использовать [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector для SAP BW, не нужно устанавливать клиентское ПО SAP (пользовательский графический интерфейс SAP) на локальном компьютере.  
  
 Однако необходимо скопировать файл соединителя SAP .NET (librfc32.dll) во вложенную папку в системном каталоге Windows. (Обычно это каталог **C:\Windows\system32**.)  
  
## <a name="considerations-for-64-bit-computers"></a>Замечания для 64-разрядных компьютеров  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector для SAP BW полностью поддерживает 64-разрядную версию [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. На 64-разрядном компьютере [!INCLUDE[msCoName](../includes/msconame-md.md)] Connector для SAP BW имеет следующие дополнительные требования:  
  
-   Для запуска пакетов в 64-разрядном режиме в любой 64-разрядной ОС Windows скопируйте 64-разрядную версию файла графического пользовательского интерфейса SAP librfc32.dll в папку **system32** в каталоге Windows. (Обычно это каталог **C:\Windows\system32**.)  
  
-   Для запуска пакетов в 32-разрядном режиме в любой 64-разрядной ОС Windows скопируйте файл графического пользовательского интерфейса SAP librfc32.dll в папку **SysWow64** в каталоге Windows. (Обычно это каталог **C:\Windows\SysWow64**.)  
  
  
