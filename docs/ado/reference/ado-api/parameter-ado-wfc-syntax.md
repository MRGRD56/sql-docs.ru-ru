---
description: Parameter (ADO — синтаксис WFC)
title: Parameter (ADO-синтаксис WFC) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Parameter collection [ADO], ADO/WFC syntax
ms.assetid: d00d1e1e-14b1-41a2-a00f-2a3cb7396f15
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a8175c2510cfcd977a0144b15f510e1061dc4f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041174"
---
# <a name="parameter-ado---wfc-syntax"></a>Parameter (ADO — синтаксис WFC)
## <a name="package-commswfcdata"></a>упаковать com. MS. WFC. Data  
  
### <a name="constructor"></a>Конструктор  
  
```  
public Parameter()  
public Parameter(String name)  
public Parameter(String name, int type)  
public Parameter(String name, int type, int dir)  
public Parameter(String name, int type, int dir, int size)  
public Parameter(String name, int type, int dir, int size, Object value)  
```  
  
### <a name="methods"></a>Методы  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
```  
  
### <a name="properties"></a>Свойства  
  
```  
public int getAttributes()  
public void setAttributes(int attr)  
public int getDirection()  
public void setDirection(int dir)  
public String getName()  
public void setName(String name)  
public int getNumericScale()  
public void setNumericScale(int scale)  
public int getPrecision()  
public void setPrecision(int prec)  
public int getSize()  
public void setSize(int size)  
public int getType()  
public void setType(int type)  
public com.ms.com.Variant getValue()  
public void setValue(Object v)  
public AdoProperties getProperties()  
```  
  
## <a name="parameter-accessor-methods"></a>Методы доступа к параметрам  
 Свойство [value](./value-property-ado.md) объекта [Parameter](./parameter-object.md) Возвращает или задает содержимое этого объекта. Содержимое представляется как вариант, тип объекта, которому можно присвоить значение и любой из нескольких типов данных.  
  
 ADO/WFC реализует свойство **value** с помощью метода **GetValue** , который возвращает объект Variant; и метод **SetValue** , принимающий вариант в качестве аргумента. Варианты в некоторых языках очень эффективны, например в Microsoft Visual Basic.  
  
 В дополнение к свойству **value** , ADO/WFC предоставляет методы *доступа* , которые используют типы данных Java для получения и установки содержимого объектов **параметров** . Большинство этих методов имеют имена в форме **получить**_DataType_ или **задать**_DataType_.  
  
 Существует одно значимое исключение: отсутствует свойство **со значением NULL** . Вместо этого существует свойство **isNull** , возвращающее логическое значение, указывающее, имеет ли поле значение null.  
  
```  
public boolean getBoolean()  
public void setBoolean(boolean v)  
public byte getByte()  
public void setByte(byte v)  
public double getDouble()  
public void setDouble(double v)  
public float getFloat()  
public void setFloat(float v)  
public int getInt()  
public void setInt(int v)  
public long getLong()  
public void setLong(long v)  
public short getShort()  
public void setShort(short v)  
public String getString()  
public void setString(String v)  
public boolean isNull()  
public void setNull()  
```  
  
## <a name="see-also"></a>См. также:  
 [Объект Parameter](./parameter-object.md)