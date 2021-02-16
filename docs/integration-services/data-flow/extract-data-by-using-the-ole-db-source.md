---
description: Извлечение данных с помощью источника OLE DB
title: Извлечение данных с помощью источника "OLE DB" | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], OLE DB
- OLE DB source [Integration Services]
ms.assetid: 4ca6eeb5-b60e-4b81-86dd-0674be8ae8d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 864463e65b5857c3d24710f286f17817d2f48a46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339774"
---
# <a name="extract-data-by-using-the-ole-db-source"></a>Извлечение данных с помощью источника OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Чтобы добавить и настроить источник OLE DB, пакет уже должен иметь по крайней мере одну задачу потока данных.  
  
### <a name="to-extract-data-using-an-ole-db-source"></a>Извлечение данных при помощи источника OLE DB  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте проект служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий необходимый пакет.  
  
2.  Чтобы открыть пакет, дважды щелкните его в обозревателе решений.  
  
3.  Щелкните вкладку **Поток данных** и перетащите источник OLE DB из **области элементов** в область конструктора.  
  
4.  Дважды щелкните источник OLE DB.  
  
5.  В диалоговом окне **Редактор источника OLE DB** на странице **Диспетчер соединений** выберите существующий диспетчер соединений OLE DB или нажмите **Создать** , чтобы создать новый диспетчер соединений. Дополнительные сведения см. в разделе [Диспетчер соединений OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
6.  Выберите метод доступа к данным.  
  
    -   **Таблица или представление** .   Выберите таблицу или представление в базе данных, с которой соединяется диспетчер соединений OLE DB.  
  
    -   **Переменная имени представления или имени таблицы** . Выберите пользовательскую переменную, которая содержит имя таблицы или представления в базе данных, с которой соединяется диспетчер соединений OLE DB.  
  
    -   **Команда SQL** .   Введите команду SQL или нажмите **Создать запрос** , чтобы написать команду SQL при помощи **Построителя запросов**.  
  
        > [!NOTE]  
        >  Команда может включать параметры. Дополнительные сведения см. в разделе [Сопоставления параметров запросов с переменными в компонентах потока данных](../../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md).  
  
    -   **Команда SQL из переменной** . Выберите пользовательскую переменную, содержащую команду SQL.  
  
        > [!NOTE]  
        >  Переменные должны определяться в области той же задачи потока данных, которая содержит источник OLE DB, либо в области того же пакета. Кроме того, переменная должна иметь строковый тип данных.  
  
7.  Чтобы обновить сопоставление между внешними и выходными столбцами, щелкните **Столбцы** и выберите другие столбцы в списке **Внешний столбец** .  
  
8.  Существует дополнительная возможность обновления имен выходных столбцов путем редактирования значений в списке **Выходной столбец** .  
  
9. Чтобы настроить выход ошибок, щелкните **Вывод ошибок**. Дополнительные сведения см. в статье [Отладка потока данных](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Можно нажать **Предварительный просмотр** , чтобы просмотреть до 200 строк данных, извлеченных из источника OLE DB.  
  
11. Нажмите кнопку **ОК**.  
  
12. Чтобы сохранить обновленный пакет, выберите пункт **Сохранить выбранные элементы** в меню **Файл** .  
  
## <a name="see-also"></a>См. также:  
 [Источник OLE DB](../../integration-services/data-flow/ole-db-source.md)   
 [Преобразования служб Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Пути служб Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [Задача потока данных](../../integration-services/control-flow/data-flow-task.md)  
  
  
