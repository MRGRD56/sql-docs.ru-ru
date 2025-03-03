---
description: Свойства столбцов (страница «Общие»)
title: Свойства столбца (страница "Общие") | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.columnproperties.general.f1
ms.assetid: a745890b-994e-4c23-8028-5c83751e60c4
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cf180c58a2313f991be8ed0b89f40e53686b378e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748654"
---
# <a name="column-properties-general-page"></a>Свойства столбцов (страница «Общие»)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Эта страница используется для просмотра свойств выделенного столбца.  
  
 Сведения на этой странице предназначены только для чтения. Для изменения этого столбца закройте диалоговое окно **Свойства столбца** , разверните в обозревателе объектов таблицу и столбцы, щелкните правой кнопкой мыши столбец и выберите пункт **Создать**.  
  
## <a name="options"></a>Параметры  
 **имя**;  
 Имя столбца.  
  
 **Тип данных**  
 Тип данных, которые могут храниться в столбце. Если тип данных является пользовательским типом данных, то выводится пользовательский тип данных. Если тип данных не является пользовательским, выводится системный тип данных. Дополнительные сведения см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).  
  
 **Системный тип**  
 Тип данных, которые могут храниться в столбце. Если тип данных является системным типом данных, то выводится системный тип данных. Если тип данных является пользовательским, то выводится системный тип данных, на котором основан пользовательский тип данных.  
  
 **Первичный ключ**  
 Указывает, является ли столбец первичным ключом. Допустимые значения — **True** и **False**.  
  
 **Разрешить значения NULL**  
 Указывает, принимает ли столбец значения NULL. Допустимые значения — **True** и **False**.  
  
 **Вычисляемый**  
 Указывает, является ли значение столбца результатом вычисляемого выражения.  
  
 **Вычисляемый текст**  
 Указывает, что для вычисления текста столбца используется инструкция. Дополнительные сведения см. в разделе [Specify Computed Columns in a Table](../../relational-databases/tables/specify-computed-columns-in-a-table.md).  
  
 **Удостоверение**  
 Указывает, является ли данный столбец столбцом идентификаторов для таблицы. Допустимые значения — **True** и **False**.  
  
 **Начальное значение идентификатора**  
 Указывает начальное значение строки для столбца идентификаторов.  
  
 **Шаг приращения идентификатора**  
 Свойство **Шаг приращения идентификатора** задает значение, которое [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] добавляет к наибольшему существующему значению идентификатора при формировании значения идентификатора для вставляемой строки.  
  
 **Привязка по умолчанию**  
 Значение по умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , связанное со столбцом. Этот параметр пуст, если связанное значение по умолчанию отсутствует.  
  
 **Схема по умолчанию**  
 Определяет схему базы данных, имеющую значение по умолчанию, привязанное к ссылочному столбцу. Этот параметр пуст, если связанное значение по умолчанию отсутствует.  
  
 **Правило**  
 Определяет ограничение целостности данных, привязанное к столбцу. Этот параметр пуст, если отсутствует привязанное правило по умолчанию.  
  
 **Схема правила**  
 Выводит схему базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которая владеет правилом, привязанным к ссылочному столбцу. Этот параметр пуст, если отсутствует привязанное правило по умолчанию.  
  
 **Длина**  
 Указывает максимальное число символов или байтов, принимаемое столбцом.  
  
 **Параметры сортировки**  
 Выводит текущие параметры сортировки для столбца. Если этот параметр пуст, то свойство параметров сортировки наследуется от объекта.  
  
 **Точность чисел**  
 Указывает максимальное число цифр в численном типе данных с фиксированной точностью.  
  
 **Масштаб числа**  
 Указывает число цифр справа от десятичной запятой в численном типе данных с фиксированной точностью.  
  
 **Пространство имен XML-схем**  
 Определяет тип XML-столбца путем проверки на языке определения XML-схем (XSD).  
  
 **Схема «Пространство имен XML-схем»**  
 Схема [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , обладающая пространством имен XML-схемы.  
  
> [!NOTE]  
>  Имеется несколько широко применяемых, но разных значений слова «схема». [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует схемы для организации объектов базы данных. Это похоже на владение. XML использует схему для определения организации XML-данных в виде последовательности пространств имен. Это способ группирования связанного XML-кода.  
  
 **Является разреженным**  
 Указывает, является ли столбец разреженным. Допустимые значения — **True** и **False**. Дополнительные сведения см. в статье [Использование разреженных столбцов](../../relational-databases/tables/use-sparse-columns.md).  
  
 **Является набором столбцов**  
 Указывает, является ли столбец набором столбцов. Допустимые значения — **True** и **False**. Дополнительные сведения см. в статье [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).  
  
 **Статус ANSI Padding**  
 Указывает, включено ли заполнение ANSI. Дополнительные сведения см. в разделе [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
 **Полнотекстовый**  
 Указывает, участвует ли столбец в полнотекстовом запросе.  
  
 **Статистическая семантика**  
 Указывает, включен ли статистический семантический поиск для столбца. Дополнительные сведения см. в разделе [Семантический поиск (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md).  
  
 **Не для репликации**  
 Указывает, доступен ли столбец для репликации. Допустимые значения — **True** и **False**.  
  
  
