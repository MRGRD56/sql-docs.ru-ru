---
title: управляемые классы SQLXML
description: Сведения о управляемых классах Microsoft SQLXML, которые предоставляют функциональные возможности SQLXML 4,0 в Microsoft .NET Framework.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- .NET Framework [SQLXML], Managed Classes
- SQL Server .NET Data Provider
- Managed Classes [SQLXML], about managed classes
- providers [SQLXML], SQL Server .NET Data Provider
- data providers [SQLXML], SQL Server .NET Data Provider
- Managed Classes [SQLXML]
- XML [SQLXML]
- SQLXML Managed Classes
- providers [SQLXML], SQLXML Managed Classes
- data providers [SQLXML], SQLXML Managed Classes
- SQLXML, Managed Classes
ms.assetid: 73a5faeb-dabf-4895-acb5-a9651b646065
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2754c46171d1cb4a9c1772ec650ca128c7e14c72
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491751"
---
# <a name="sqlxml-40-net-framework-support---managed-classes"></a>Поддержка SQLXML 4.0 на платформе .NET Framework — управляемые классы
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 поддерживает функции для создания приложений, которые обращаются к XML-данным в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], переводят данные в среду [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework, обрабатывают данные и отправляют обновления назад в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
  
  Управляемые классы [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML предоставляют возможности SQLXML 4.0 в платформе [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. С помощью управляемых классов SQLXML можно писать приложения на языке C# для доступа к XML-данным из экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], доставлять данные в среду .NET Framework, обрабатывать их и отправлять обновления обратно в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в виде дельты для применения изменений. При применении изменений к базе данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью управляемых классов SQLXML необходимо использовать схему сопоставления. Рабочий пример см. в разделе [доступ к функциям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
 Для использования управляемых классов SQLXML с SQLXML 4.0 следует установить среду Microsoft Visual Studio.  
  
> [!NOTE]  
>  Платформа .NET Framework включает поставщик данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .NET. Этот поставщик может использоваться для доступа к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] из платформы .NET. Однако он может обрабатывать только стандартные запросы SQL (то есть запросы реляционных баз данных, за исключением запросов FOR XML). В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] нельзя выполнять XML-шаблоны или запросы XPath со стороны сервера.  

 Сведения о доступе и изменении данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../../includes/msconame-md.md)] платформа .NET Framework и об использовании дельтами для обновления данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] таблицах см. в разделе [доступ к функциям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
> [!NOTE]  
>  Можно также составить приложения [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio для массовой загрузки XML-документов с использованием массовой загрузки XML. Дополнительные сведения см. в разделе [выполнение групповой загрузки XML-данных &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md). Необходимо добавить в приложение ссылку на DLL-библиотеку массовой загрузки XML (Xblkld4.dll). Это DLL-библиотека COM, для которой Visual Studio .NET автоматически создает библиотеку-упаковщик.  
  
  В этом разделе приводятся примеры приложений, демонстрирующие использование [!INCLUDE[msCoName](../../../includes/msconame-md.md)] управляемых классов SQLXML:  
 [Исполнение запросов SQL &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md)  
  [Выполнение SQL-запросов с использованием метода ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md)  
  [Обработка XML на стороне клиента &#40;управляемые классы SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/processing-xml-on-the-client-side-sqlxml-managed-classes.md)  
  [Запросы XPath, &#40;управляемые классы SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md)  
  [Выполнять запросы XPath с пространствами имен &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md)  
  [Выполнение файлов шаблонов с использованием свойства CommandText](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md)  
  [Выполнение файлов шаблонов через свойство CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md)  
  [Применение преобразования XSL &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md)  
  

  
  
