---
title: Сравнение запросов FOR XML и вложенных запросов FOR XML | Документация Майкрософт
description: Узнайте о преимуществах вложенных запросов FOR XML путем просмотра примера, который сравнивает одноуровневый запрос FOR XML с вложенным запросом FOR XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML query
- queries [XML in SQL Server], comparing query types
ms.assetid: 19225b4a-ee3f-47cf-8bcc-52699eeda32c
author: rothja
ms.author: jroth
ms.openlocfilehash: ebc5eb7fdc2e4ebe1e5424e3f7df1ac52a2b193e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488362"
---
# <a name="for-xml-query-compared-to-nested-for-xml-query"></a>Сравнение запросов FOR XML и вложенных запросов FOR XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом разделе сравнивается одноуровневый запрос FOR XML с вложенным запросом FOR XML. Одним из преимуществ вложенных запросов FOR XML является то, что в них для результатов запроса можно задать сочетание XML-данных по атрибутивной или элементной модели. Это демонстрируется в примере.  
  
## <a name="example"></a>Пример  
 Следующий запрос `SELECT` позволяет получить сведения о категории и подкатегории продукта в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . В запросе нет вложенных запросов FOR XML.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT   ProductCategory.ProductCategoryID,   
         ProductCategory.Name as CategoryName,  
         ProductSubCategory.ProductSubCategoryID,   
         ProductSubCategory.Name  
FROM     Production.ProductCategory, Production.ProductSubCategory  
WHERE    ProductCategory.ProductCategoryID = ProductSubCategory.ProductCategoryID  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
GO  
```  
  
 Частичный результат:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory ProductSubCategoryID="1" Name="Mountain Bike"/>  
  <ProductSubCategory ProductSubCategoryID="2" Name="Road Bike"/>  
  <ProductSubCategory ProductSubCategoryID="3" Name="Touring Bike"/>  
</ProductCategory>  
...  
```  
  
 Если в запросе указана директива `ELEMENTS` , то результат будет представлен в элементной форме, как показано в следующем фрагменте результата:  
  
```  
<ProductCategory>  
  <ProductCategoryID>1</ProductCategoryID>  
  <CategoryName>Bike</CategoryName>  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <Name>Mountain Bike</Name>  
  </ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  </ProductSubCategory>  
</ProductCategory>  
```  
  
 Предположим, что нужно создать иерархию XML-данных, которая сочетает атрибутивную и элементную модели, как показано в следующем фрагменте:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bike</SubCategoryName></ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  <ProductSubCategory>  
     ...  
</ProductCategory>  
```  
  
 В этом фрагменте такие сведения о категории продукта, как идентификатор категории и имя категории, являются атрибутами. Однако сведения о подкатегории представлены с использованием элементов. Для конструирования элемента <`ProductCategory`> можно написать запрос `FOR XML`, как показано ниже:  
  
```  
SELECT ProductCategoryID, Name as CategoryName  
FROM Production.ProductCategory ProdCat  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 Результат:  
  
```  
< ProdCat ProductCategoryID="1" CategoryName="Bikes" />  
< ProdCat ProductCategoryID="2" CategoryName="Components" />  
< ProdCat ProductCategoryID="3" CategoryName="Clothing" />  
< ProdCat ProductCategoryID="4" CategoryName="Accessories" />  
```  
  
 Для создания необходимых вложенных элементов <`ProductSubCategory`> необходимо после этого добавить вложенный запрос `FOR XML`, как показано ниже:  
  
```  
SELECT ProductCategoryID, Name as CategoryName,  
       (SELECT ProductSubCategoryID, Name SubCategoryName  
        FROM   Production.ProductSubCategory  
        WHERE ProductSubCategory.ProductCategoryID =   
              ProductCategory.ProductCategoryID  
        FOR XML AUTO, TYPE, ELEMENTS  
       )  
FROM Production.ProductCategory  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 При рассмотрении предыдущего запроса необходимо отметить следующее.  
  
-   Внутренний запрос `FOR XML` получает сведения о подкатегории продукта. Директива `ELEMENTS` добавляется во внутренний запрос `FOR XML` , создавая элементный XML, который добавляется к XML-данным, создаваемым внешним запросом. По умолчанию внешний запрос создает XML-данные по атрибутивной модели.  
  
-   Во внутреннем запросе указана директива `TYPE` , поэтому результат имеет тип **xml** . Если директива `TYPE` не указана, то возвращается результат типа **nvarchar(max)** и XML-данные возвращаются как сущности.  
  
-   Внешний запрос также содержит директиву `TYPE` , поэтому результат запроса возвращается клиенту как тип **xml** .  
  
 Частичный результат:  
  
```  
<ProductCategory ProductCategoryID="1" CategoryName="Bike">  
  <ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bike</SubCategoryName></ProductSubCategory>  
  <ProductSubCategory>  
     ...  
  <ProductSubCategory>  
     ...  
</ProductCategory>  
```  
  
 Следующий запрос представляет собой расширение предыдущего запроса. Он показывает полную иерархию продуктов в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Это включает следующие действия.  
  
-   Категории продукции  
  
-   Подкатегории продукции в каждой категории  
  
-   Модели продуктов в каждой подкатегории  
  
-   Продукты в каждой модели  
  
 Следующий пример может быть полезен для понимания базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] :  
  
```  
SELECT ProductCategoryID, Name as CategoryName,  
       (SELECT ProductSubCategoryID, Name SubCategoryName,  
               (SELECT ProductModel.ProductModelID,   
                       ProductModel.Name as ModelName,  
                       (SELECT ProductID, Name as ProductName, Color  
                        FROM   Production.Product  
                        WHERE  Product.ProductModelID =   
                               ProductModel.ProductModelID  
                        FOR XML AUTO, TYPE)  
                FROM   (SELECT distinct ProductModel.ProductModelID,   
                               ProductModel.Name  
                        FROM   Production.ProductModel,   
                               Production.Product  
                        WHERE  ProductModel.ProductModelID =   
                               Product.ProductModelID  
                        AND    Product.ProductSubCategoryID =   
                               ProductSubCategory.ProductSubCategoryID)   
                                  ProductModel  
                FOR XML AUTO, type  
               )  
        FROM Production.ProductSubCategory  
        WHERE ProductSubCategory.ProductCategoryID =   
              ProductCategory.ProductCategoryID  
        FOR XML AUTO, TYPE, ELEMENTS  
       )  
FROM Production.ProductCategory  
ORDER BY ProductCategoryID  
FOR XML AUTO, TYPE  
```  
  
 Частичный результат:  
  
```  
<Production.ProductCategory ProductCategoryID="1" CategoryName="Bikes">  
  <Production.ProductSubCategory>  
    <ProductSubCategoryID>1</ProductSubCategoryID>  
    <SubCategoryName>Mountain Bikes</SubCategoryName>  
    <ProductModel ProductModelID="19" ModelName="Mountain-100">  
      <Production.Product ProductID="771"   
                ProductName="Mountain-100 Silver, 38" Color="Silver" />  
      <Production.Product ProductID="772"   
                ProductName="Mountain-100 Silver, 42" Color="Silver" />  
      <Production.Product ProductID="773"   
                ProductName="Mountain-100 Silver, 44" Color="Silver" />  
        ...  
    </ProductModel>  
     ...  
```  
  
 Если удалить директиву `ELEMENTS` из вложенного запроса `FOR XML` , который создает подкатегории продуктов, то весь результат будет создан по атрибутивной модели. Этот запрос можно составить без вложений. Добавление директивы `ELEMENTS` приводит к созданию XML-документа как по атрибутивной, так и по элементной модели. Этот результат нельзя сформировать с помощью одноуровневого запроса FOR XML.  
  
## <a name="see-also"></a>См. также:  
 [Использование вложенных запросов FOR XML](../../relational-databases/xml/use-nested-for-xml-queries.md)  
  
  
