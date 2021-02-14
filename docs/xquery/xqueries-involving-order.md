---
title: Запросы XQuery использующие, включающий заказ | Документация Майкрософт
description: Просмотрите примеры запросы XQuery использующие, основанные на последовательности, в которой узлы отображаются в документе.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- XQuery, sequence
- ordered expressions [XQuery]
ms.assetid: 4f1266c5-93d7-402d-94ed-43f69494c04b
author: rothja
ms.author: jroth
ms.openlocfilehash: d877c75223c42a8dff3a9a4c4615975a136cd6b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352332"
---
# <a name="xqueries-involving-order"></a>Запросы XQuery, использующие упорядочивание
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Реляционные базы данных не используют концепцию последовательности. Например, нельзя создать запрос типа «Получить первого заказчика из базы данных». Однако можно запросить XML-документ и получить первый \<Customer> элемент. Затем всегда будет появляться информация об этом же заказчике.  
  
 В этом подразделе описаны запросы, основанные на последовательности появления узлов в документе.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-retrieve-manufacturing-steps-at-the-second-work-center-location-for-a-product"></a>A. Получить этапы производства на втором участке цеха по изготовлению продукта  
 Для определенной модели продукта следующий запрос извлекает этапы производства на втором участке цеха в последовательности участков цехов в производственном процессе.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    <ManuStep ProdModelID = "{sql:column("Production.ProductModel.ProductModelID")}"  
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
     <Location>  
       { (//AWMI:root/AWMI:Location)[2]/@* }  
       <Steps>  
         { for $s in (//AWMI:root/AWMI:Location)[2]//AWMI:step  
           return  
              <Step>  
               { string($s) }  
              </Step>  
         }  
        </Steps>  
      </Location>  
     </ManuStep>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Выражения в фигурных скобках заменяются результатом их оценки. Дополнительные сведения см. в разделе [Построение XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md).  
  
-   **@\*** Извлекает все атрибуты второго расположения рабочего центра.  
  
-   Итерация FLWOR (для... RETURN) извлекает все <`step`> дочерние элементы второго расположения рабочего центра.  
  
-   [Функция SQL: column () (XQuery)](../xquery/xquery-extension-functions-sql-column.md) включает реляционное значение в создаваемый XML.  
  
 Результат:  
  
```  
<ManuStep ProdModelID="7" ProductModelName="HL Touring Frame">  
  <Location LocationID="20" SetupHours="0.15"   
              MachineHours="2"  LaborHours="1.75" LotSize="1">  
  <Steps>  
   <Step>Assemble all frame components following blueprint 1299.</Step>  
     ...  
  </Steps>  
 </Location>  
</ManuStep>    
```  
  
 Предыдущий запрос извлекает только текстовые узлы. Если вместо этого требуется <весь `step` элемент>, удалите функцию **String ()** из запроса:  
  
### <a name="b-find-all-the-material-and-tools-used-at-the-second-work-center-location-in-the-manufacturing-of-a-product"></a>Б. Найти все материалы и инструменты, используемые на втором участке цеха для производства продукта  
 Для определенной модели продукта следующий запрос извлекает инструменты и материалы, используемые на втором участке цеха в последовательности участков цехов в производственном процессе.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <Location>  
      { (//AWMI:root/AWMI:Location)[1]/@* }  
       <Tools>  
         { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:tool  
           return  
              <Tool>  
                { string($s) }  
              </Tool>  
          }  
        </Tools>  
        <Materials>  
            { for $s in (//AWMI:root/AWMI:Location)[1]//AWMI:step//AWMI:material  
              return  
                 <Material>  
                    { string($s) }  
                 </Material>  
             }  
         </Materials>  
  </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Запрос конструирует элемент <лока `tion`> и извлекает его значения атрибутов из базы данных.  
  
-   Запрос использует две итерации FLWOR (FOR...RETURN): одна необходима для получения инструментов, вторая — для получения используемых материалов.  
  
 Результат:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
          MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
    <Tool>router with a carbide tip 15</Tool>  
    <Tool>Forming Tool FT-15</Tool>  
  </Tools>  
  <Materials>  
    <Material>aluminum sheet MS-2341</Material>  
  </Materials>  
</Location>  
```  
  
### <a name="c-retrieve-the-first-two-product-feature-descriptions-from-the-product-catalog"></a>В. Извлечь описания характеристик первых двух продуктов из каталога продуктов  
 Для конкретной модели продукта запрос получает первые два описания характеристик из `Features` элемента <> в каталоге моделей продуктов.  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <ProductModel ProductModelID= "{ data( (/p1:ProductDescription/@ProductModelID)[1] ) }"  
                   ProductModelName = "{ data( (/p1:ProductDescription/@ProductModelName)[1] ) }" >  
       {  
         for $F in /p1:ProductDescription/p1:Features  
         return   
           $F/*[position() <= 2]   
       }  
     </ProductModel>  
      ') as x  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
 Текст запроса конструирует XML, который включает `ProductModel` элемент <> с атрибутами ProductModelID и ProductModelName.  
  
-   Запрос использует для... Цикл возврата для получения описания функций модели продукта. Функция **позиционирования ()** используется для получения первых двух функций.  
  
 Результат:  
  
```xml
<ProductModel ProductModelID="19" ProductModelName="Mountain 100">  
 <p1:Warranty   
  xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
  <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
 <p2:Maintenance   
  xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <p2:NoOfYears>10</p2:NoOfYears>  
  <p2:Description>maintenance contact available through your dealer   
            or any AdventureWorks retail store.  
  </p2:Description>  
 </p2:Maintenance>  
</ProductModel>   
```  
  
### <a name="d-find-the-first-two-tools-used-at-the-first-work-center-location-in-the-manufacturing-process-of-the-product"></a>Г. Найти первые два инструмента, используемые в производственном процессе на первом участке цеха  
 Для модели продукта этот запрос возвращает первые два инструмента, используемые на первом участке цеха в последовательности участков цехов в производственном процессе. Запрос указывается по производственным инструкциям, хранящимся в столбце **Instructions** таблицы **Production. ProductModel** .  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   for $Inst in (//AWMI:root/AWMI:Location)[1]  
   return   
     <Location>  
       { $Inst/@* }  
       <Tools>  
         { for $s in ($Inst//AWMI:step//AWMI:tool)[position() <= 2]  
           return  
             <Tool>  
               { string($s) }  
             </Tool>  
         }  
       </Tools>  
     </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Результат:  
  
```xml
<Location LocationID="10" SetupHours=".5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
  <Tools>  
    <Tool>T-85A framing tool</Tool>  
    <Tool>Trim Jig TJ-26</Tool>  
  </Tools>  
</Location>   
```  
  
### <a name="e-find-the-last-two-manufacturing-steps-at-the-first-work-center-location-in-the-manufacturing-of-a-specific-product"></a>Д. Найти два последних этапа производства в изготовлении определенного продукта на первом участке цеха  
 Запрос использует функцию **Last ()** для получения последних двух этапов производства.  
  
```sql
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  <LastTwoManuSteps>  
   <Last-1Step>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[(last()-1)]/text() }  
   </Last-1Step>  
   <LastStep>   
     { (/AWMI:root/AWMI:Location)[1]/AWMI:step[last()]/text() }  
   </LastStep>  
  </LastTwoManuSteps>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Результат:  
  
```xml
<LastTwoManuSteps>  
   <Last-1Step>When finished, inspect the forms for defects per   
               Inspection Specification .</Last-1Step>  
   <LastStep>Remove the frames from the tool and place them in the   
             Completed or Rejected bin as appropriate.</LastStep>  
</LastTwoManuSteps>  
```  
  
## <a name="see-also"></a>См. также:  
 [Данные XML (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [Справочник по языку XQuery (SQL Server)](../xquery/xquery-language-reference-sql-server.md)   
 [Создание XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
