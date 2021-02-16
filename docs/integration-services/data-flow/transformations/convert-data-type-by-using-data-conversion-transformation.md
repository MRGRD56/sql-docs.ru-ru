---
description: Преобразование типа данных с помощью преобразования "Конвертация данных"
title: Преобразование типа данных с помощью преобразования "Конвертация данных" | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- converting data types [Integration Services]
- Data Conversion transformation
- data types [Integration Services], converting
ms.assetid: 4aabbe4f-7666-4672-865a-9627bd25fbfd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9056c9d657d1d5f4c9b24004c0ce70a2d32ed6e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350734"
---
# <a name="convert-data-type-with-data-conversion-transformation"></a>Преобразование типа данных с помощью преобразования "Конвертация данных"

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Чтобы добавить и настроить преобразование «Конвертация данных», пакет должен обладать как минимум одной задачей потока данных и одним источником.  
  
### <a name="to-convert-data-to-a-different-data-type"></a>Произведение конвертации данных в другой тип данных  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]откройте проект служб [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , содержащий необходимый пакет.  
  
2.  Чтобы открыть пакет, дважды щелкните его в обозревателе решений.  
  
3.  Щелкните вкладку **Поток данных** и перетащите преобразование «Конвертация данных» из **области элементов** в область конструктора.  
  
4.  Подключите преобразование «Конвертация данных» к потоку данных, перетащив соединитель из источника или предыдущего преобразования в преобразование «Конвертация данных».  
  
5.  Дважды щелкните преобразование «Конвертация данных».  
  
6.  В диалоговом окне **Редактор преобразования "Конвертация данных"** в таблице **Доступные входные столбцы** установите флажки рядом со столбцами, данные из которых необходимо преобразовать.  
  
    > [!NOTE]  
    >  К каждому входному столбцу можно применить несколько преобразований.  
  
7.  При необходимости измените значение по умолчанию в столбце **Псевдоним вывода** .  
  
8.  Выберите новый тип данных для столбца из списка **Тип данных** . По умолчанию тип данных является типом данных входного столбца.  
  
9. Дополнительно, в зависимости от выбранного типа данных, можно обновить значения в столбцах **Длина**, **Точность**, **Масштаб** и **Кодовая страница** .  
  
10. Для настройки вывода ошибок нажмите **Настройка вывода ошибок**. Дополнительные сведения см. в статье [Отладка потока данных](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Нажмите кнопку **ОК**.  
  
12. Чтобы сохранить обновленный пакет, выберите пункт **Сохранить выбранные элементы** в меню **Файл** .  
  
## <a name="see-also"></a>См. также:  
 [Data Conversion Transformation](../../../integration-services/data-flow/transformations/data-conversion-transformation.md)   
 [Преобразования служб Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Пути служб Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Типы данных служб Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Задача потока данных](../../../integration-services/control-flow/data-flow-task.md)  
  
  
