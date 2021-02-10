---
description: Формат сохраняемости XML
title: Формат сохраняемости XML | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence format
ms.assetid: 6e146738-ac4d-47bb-b6cd-d87b2260aead
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a1a1b832f9e34cbf99e5fd01c424bf54c50c3bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036724"
---
# <a name="xml-persistence-format"></a>Формат сохраняемости XML
ADO использует кодировку UTF-8 для сохраняемого XML-потока.  
  
 Формат ADO XML разбивается на два раздела — раздел схемы, за которым следует раздел данных. Ниже приведен пример XML-файла для таблицы грузоотправителей из базы данных Northwind. В этом примере рассматриваются различные части XML.  
  
## <a name="remarks"></a>Remarks  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"   
xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"   
xmlns:rs="urn:schemas-microsoft-com:rowset"   
xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="ShipperID" rs:number="1"   
        rs:basetable="shippers" rs:basecolumn="ShipperID"  
        rs:keycolumn="true">   
        <s:datatype dt:type="int" dt:maxLength="4" rs:precision="10"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="CompanyName" rs:number="2"   
        rs:nullable="true" rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="CompanyName">   
        <s:datatype dt:type="string" dt:maxLength="40" />   
      </s:AttributeType>   
      <s:AttributeType name="Phone" rs:number="3" rs:nullable="true"   
        rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="Phone">   
        <s:datatype dt:type="string" dt:maxLength="24"/>   
      </s:AttributeType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  
  <rs:data>   
    <z:row ShipperID="1" CompanyName="Speedy Express"   
      Phone="(503) 555-9831"/>   
    <z:row ShipperID="2" CompanyName="United Package"   
      Phone="(503) 555-3199"/>   
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>   
  </rs:data>   
</xml>  
```  
  
 В схеме показаны объявления пространств имен, раздел схемы и раздел данных. Раздел Schema содержит определения для строк, Шипперид, CompanyName и Phone.  
  
 Определения схемы соответствуют [спецификации консорциума W3C XML-Data](http://www.w3.org/TR/1998/NOTE-XML-data/) и могут быть полностью проверены (хотя проверка не будет выполняться в Internet Explorer 5). XML-Data в настоящее время является единственным поддерживаемым форматом схемы для сохраняемости набора записей.  
  
 Раздел данных содержит три строки, содержащие сведения о грузоотправителях. Для пустого набора строк раздел Data может быть пустым, но \<rs:data> теги должны присутствовать. Без данных можно написать краткую сокращенную форму тегов \<rs:data/> . Любой тег с префиксом RS указывает на то, что он находится в пространстве имен, определенном в наборе строк urn: schemas-microsoft-com:.  
  
## <a name="see-also"></a>См. также:  
 [Сохранение записей в формате XML](./persisting-records-in-xml-format.md)