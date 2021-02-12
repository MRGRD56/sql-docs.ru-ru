---
title: Соединения
description: Чтобы загрузить данные в надстройка Master Data Services для Excel, сначала создайте подключение. Каждый раз при запуске Excel необходимо подключиться к репозиторию.
ms.custom: microsoft-excel-add-in
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 2f2b2f9d-7744-460e-83cd-56d34ea70ff0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: dfdbfc96b3a865aa04fdb4ae22406f0b37ea99ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272435"
---
# <a name="connections-mds-add-in-for-excel"></a>Соединения (настройка MDS для Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Для скачивания данных в [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]необходимо сначала создать соединение. Соединение — это данные, по которым веб-служба [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] узнает, с какой базой данных MDS нужно устанавливать соединение.  
  
 Строкой подключения обычно является URL-адрес веб-приложения [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], например `https://contoso/mds`.  
  
 При каждом запуске Excel необходимо устанавливать соединение с репозиторием MDS. Единственное исключение — когда активный лист уже содержит данные, управляемые MDS. В этом случае соединение устанавливается автоматически при каждом обновлении и при каждой публикации данных на листе.  
  
 Можно создать несколько подключений. Последнее соединение, к которому было обращение, считается соединением по умолчанию.  
  
 Одновременно могут быть соединены несколько пользователей. Но если несколько пользователей попытаются опубликовать одни и те же данные, могут возникнуть конфликты. Дополнительные сведения см. в разделе [Обзор импорта данных из Excel (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md).  
  
## <a name="connect-automatically-and-load-frequently-used-data"></a>Автоматическое соединение и загрузка часто используемых данных  
 Если вы всегда соединяетесь с одним и тем же сервером и загружаете один и тот же набор данных, то можно создать файл ярлыка запроса, который содержит сведения о соединении и фильтрах. Дополнительные сведения о файлах запросов см. в разделе [файлы ярлыков запросов &#40;надстройки MDS для Excel&#41;](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
## <a name="data-quality-services"></a>Data Quality Services  
 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] включает функциональные возможности служб Data Quality Services, которые помогут сопоставить данные, прежде чем публиковать их в репозитории MDS. Если при установлении соединения база данных DQS установлена на том же экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , что и база данных MDS, то на ленте появятся кнопки служб DQS. Если база данных DQS_Main не существует на этом экземпляре, то эти кнопки не отображаются и функции качества данных будут недоступны.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Установите соединение с базой данных служб [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|[Соединение с репозиторием MDS (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)|  
|Загрузка данных MDS в Excel.|[Export Data to Excel from Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
|Фильтрация данных MDS перед их загрузкой в Excel.|[Фильтрация данных перед их экспортом (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>См. также  
  
-   [Обзор экспорта данных в Excel (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
-   [Файлы ярлыков запросов (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md)  
  
-   [Надстройка Master Data Services для Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
