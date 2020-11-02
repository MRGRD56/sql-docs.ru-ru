---
description: Выполнение операции (мастер импорта и экспорта SQL Server)
title: Выполнение операции (мастер импорта и экспорта SQL Server) | Документы Майкрософт
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.performingoperation.f1
ms.assetid: 83259509-71d6-4a64-a7f2-4e9603b30bd4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cda6ec744ca603c1a03df22bcb391b6d75bbff58
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439348"
---
# <a name="performing-operation-sql-server-import-and-export-wizard"></a>Выполнение операции (мастер импорта и экспорта SQL Server)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


После того как вы проверите значения, выбранные в мастере, и нажмете кнопку **Готова** на странице **Завершение работы мастера** , в мастере импорта и экспорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] откроется страница **Выполнение операции** . На этой странице отображается ход выполнения и результат операции, настроенной на предыдущих страницах. На этой странице никакие действия не требуются.

## <a name="screen-shot---operation-in-progress"></a>Снимок экрана: выполнение операции 
 На следующем снимке экрана показана страница **Выполнение операции** мастера в процессе выполнения операции.  
  
 ![Страница "Выполнение операции" мастера импорта и экспорта](../../integration-services/import-export-data/media/performing-operation1.png "Страница "Выполнение операции" мастера импорта и экспорта")  

## <a name="screen-shot---operation-completed"></a>Снимок экрана: операция завершена 
 На следующем снимке экрана показана страница **Выполнение операции** мастера после завершения операции. Щелкните элемент в столбце **Сообщение** , чтобы получить дополнительные сведения о соответствующем этапе.  
  
 ![Снимок экрана: страница "Успешно" в мастере импорта и экспорта.](../../integration-services/import-export-data/media/performing-operation2.png "Страница "Выполнение операции" мастера импорта и экспорта")  
  
## <a name="watch-the-progress-of-the-operation"></a>Просмотр хода выполнения операции
 **Действие**  
 Отображает каждый этап операции.  
  
 **Состояние**  
 Отображает успешное или сбойное выполнение каждого этапа.  
  
 **Message**  
 Отображает связанные с соответствующим этапом уведомления и сообщения об ошибках. Щелкните элемент в этом столбце, чтобы получить дополнительные сведения о соответствующем этапе.

## <a name="interrupt-the-operation-or-save-the-results"></a>Прерывание операции или сохранение результатов
 **Остановить**  
 Кнопка **Остановить** позволяет прервать операцию в случае необходимости.  
  
 **Отчет**  
 Позволяет просмотреть отчет о результатах, сохранить его в файл, скопировать его в буфер обмена или отправить по электронной почте.  
  
## <a name="whats-next"></a>Что дальше?  
 После успешного выполнения настроенной операции работа с мастером импорта и экспорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет закончена.  
-   Если операция запускается сразу, вы можете открыть выбранное место назначения и просмотреть скопированные мастером данные.  
-   Если вы сохранили созданный мастером пакет SSIS, его можно открыть в SQL Server Data Tools для настройки и повторного использования. Сведения о настройке сохраненного пакета и его последующем запуске см. в разделе [Сохранение пакета служб SSIS](../../integration-services/import-export-data/save-ssis-package-sql-server-import-and-export-wizard.md).

## <a name="see-also"></a>См. также раздел
[Приступая к работе с простым примером мастера импорта и экспорта](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)


