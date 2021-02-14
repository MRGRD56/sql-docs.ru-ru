---
description: Проверка данных (надстройка MDS для Excel)
title: Проверка данных
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 71eda98f-01a4-4fff-8246-be3133782523
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 27e6cc619206c108e17aa62c907cdea083526b48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352960"
---
# <a name="validating-data-mds-add-in-for-excel"></a>Проверка данных (надстройка MDS для Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]при публикации данных происходят проверки двух типов:  
  
-   К данным применяются все определенные бизнес-правила.  
  
-   Данные проверяются на допустимость в качестве значений атрибута (например, у них должно быть определенные количество знаков или тип данных).  
  
 В обоих случаях допустимые данные публикуются в репозитории MDS. Недопустимые данные выделяются, а в столбцах состояния могут отображаться сведения об ошибке.  
  
## <a name="when-validation-occurs"></a>Когда осуществляется проверка  
 В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]проверка происходит при публикации новых или измененных данных, а также при применении бизнес-правил вручную.  
  
 Если проверка с использованием бизнес-правил не прошла, данные все равно публикуются в репозитории MDS. Если входные данные не проходят проверку, они не публикуются в репозитории.  
  
## <a name="validation-statuses"></a>Состояния проверки  
 В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]возможны приведенные ниже состояния проверки.  
  
 Сведения о дополнительных предварительных условиях см. в разделе [Состояния проверки (службы Master Data Services)](../../master-data-services/validation-statuses-master-data-services.md).  
  
|Состояние|Описание|  
|------------|-----------------|  
|Проверка завершена с ошибкой.|Одно или несколько значений в строке не прошли проверку с использованием бизнес-правил, определенных администратором MDS.|  
|Проверка завершена успешно|Все значения в строке прошли проверку с использованием бизнес-правил.|  
  
## <a name="input-statuses"></a>Состояния входных данных  
 В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]возможны приведенные ниже состояния входных данных.  
  
|Состояние|Описание|  
|------------|-----------------|  
|Ошибка|Одно или несколько значений в строке не соответствуют системным требованиям, таким как длина или тип данных. Значение не обновляется в репозитории MDS.|  
|Новая строка|Значения в строке еще не опубликованы в репозитории MDS.|  
|Только для чтения|Вошедший в систему пользователь имеет разрешения «Только для чтения» на одно или несколько значений в строке, и значения нельзя обновить.|  
|Без изменений|Ни одно значение в строке на листе не было изменено. Это не означает, что значения в хранилище не изменились. Чтобы получить последние данные на листе, в группе **Подключение и загрузка** нажмите кнопку **Загрузить или обновить**.<br /><br /> Это параметр по умолчанию для каждой строки.|  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Определяет, какие значения не соответствуют определенным бизнес-правилам.|[Применение бизнес-правил (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
|Чтобы исправить ошибки проверки, просмотрите все транзакции, выполненные для элемента.|[Просмотр всех заметок или транзакций для элемента (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>См. также  
  
-   [Обзор импорта данных из Excel (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
