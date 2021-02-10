---
description: Выполнение примера приложения адресной книги
title: Запуск примера приложения адресной книги | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 3a2644e9-d634-4ae6-a5b7-13fb7b317ec7
author: rothja
ms.author: jroth
ms.openlocfilehash: b4e77339de5ec7ebddbdb1314228abf6eef8c15d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031816"
---
# <a name="running-the-address-book-sample-application"></a>Выполнение примера приложения адресной книги
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Чтобы запустить приложение адресной книги, выполните следующую процедуру.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
### <a name="to-run-this-application"></a>Запуск приложения  
  
1.  Убедитесь, что Microsoft SQL Server работает. Нажмите кнопку **Пуск**, укажите пункт **программы**, выберите **Microsoft SQL Server 7,0**, а затем щелкните **Service Manager**. Если в белом кружке есть зеленая стрелка, то SQL Server выполняется. Если это не так (в белом круге будет красный квадрат), нажмите кнопку **Пуск/продолжить**.  
  
2.  В Microsoft Internet Explorer 4,0 или более поздней версии введите следующий адрес:  
  
     **https://**  **/РДС/аддрессбук/аддрбук.АСП**  
  
     где веб- *сервер* — имя сервера, на котором установлены компоненты RDS Server.  
  
3.  Затем можно попробовать различные сценарии в примере приложения адресной книги, например поиск человека по его имени, список всех людей с названием «руководитель программы» или изменение существующих записей. Нажмите кнопку **найти** , чтобы заполнить сетку данных всеми доступными именами.  
  
## <a name="see-also"></a>См. также:  
 [Объект привязки данных адресной книги](./address-book-data-binding-object.md)