---
description: Просмотр всех заметок или транзакций для элемента (надстройка MDS для Excel)
title: Просмотр заметок или транзакций
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: de90c81c-9e7f-4997-bf96-e22b97b2862c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 2dba27ae76c411f4ac095b9e8c38fa4987e32749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340375"
---
# <a name="view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel"></a>Просмотр всех заметок или транзакций для элемента (надстройка MDS для Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]можно просмотреть заметки (комментарии) и транзакции для строк данных (элементов), если необходимо узнать, как менялись данные с течением времени.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   Необходимо иметь активный лист, содержащий данные, управляемые MDS.  
  
-   Сущность **Тип журнала транзакций** должна иметь значение **Member** или **Attribute**.  
  
### <a name="to-view-annotations-or-transactions"></a>Просмотр заметок и транзакций  
  
1.  Щелкните ячейку в строке, которая содержит транзакции, подлежащие просмотру.  
  
2.  Щелкните правой кнопкой мыши и в открывшемся меню выберите **Просмотр транзакций** или **Просмотр журнала**.  
  
3.  В диалоговом окне **Просмотр транзакций** будет показан список транзакций. Чтобы просмотреть все заметки, связанные с транзакцией, щелкните строку в сетке.  
  
## <a name="see-also"></a>См. также:  
 [Обзор импорта данных из Excel (надстройка MDS для Excel)](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
