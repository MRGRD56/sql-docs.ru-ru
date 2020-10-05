---
description: Создание собственного настраиваемого обработчика
title: Написание собственного настраиваемого обработчика | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
- customized handler in RDS [ADO]
ms.assetid: d447712a-e123-47b5-a3a4-5d366cfe8d72
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d761b781e7de4225f51fb3600ac467015a0c274
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91722725"
---
# <a name="writing-your-own-customized-handler"></a>Создание собственного настраиваемого обработчика
Если вы являетесь администратором сервера IIS, которому требуется поддержка RDS по умолчанию, вам может потребоваться написать собственный обработчик, но лучше контролировать запросы пользователей и права доступа.  
  
 МСДФМАП. Обработчик реализует интерфейс **идатафакторихандлер** .  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="idatafactoryhandler-interface"></a>Интерфейс Идатафакторихандлер  
 Этот интерфейс имеет два **метода: "," и "** **Повторное соединение**". Для обоих методов требуется, чтобы свойство [CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md) было установлено в значение **адусеклиент**.  
  
 Оба метода принимают аргументы, которые отображаются после первой запятой в ключевом слове**handler =**. Например, `"Handler=progid,arg1,arg2;"` передает строку аргумента `"arg1,arg2"` и `"Handler=progid"` передает аргумент NULL.  
  
## <a name="getrecordset-method"></a>Метод WebMethod  
 Этот метод запрашивает источник данных и создает новый объект [набора записей](../../reference/ado-api/recordset-object-ado.md) с помощью предоставленных аргументов. **Набор записей** должен быть открыт с помощью **адлоккбатчоптимистик** и не должен быть открыт асинхронно.  
  
### <a name="arguments"></a>Аргументы  
 ***conn***  Строка подключения.  
  
 ***args***  Аргументы для обработчика.  
  
 ***запрос***  Текст команды для выполнения запроса.  
  
 ***ППРС***  Указатель, по которому должен возвращаться **набор записей** .  
  
## <a name="reconnect-method"></a>Повторно подключить метод  
 Этот метод обновляет источник данных. Он создает новый объект [соединения](../../reference/ado-api/connection-object-ado.md) и присоединяет заданный **набор записей**.  
  
### <a name="arguments"></a>Аргументы  
 ***conn***  Строка подключения.  
  
 ***args***  Аргументы для обработчика.  
  
 ***вытягивание***  Объект **Recordset** .  
  
## <a name="msdfhdlidl"></a>мсдфхдл. idl  
 Это определение интерфейса для **идатафакторихандлер** , которое отображается в файле **мсдфхдл. idl** .  
  
```cpp
[  
  uuid(D80DE8B3-0001-11d1-91E6-00C04FBBBFB3),  
  version(1.0)  
]  
library MSDFHDL  
{  
    importlib("stdole32.tlb");  
    importlib("stdole2.tlb");  
  
    // TLib : Microsoft ActiveX Data Objects 2.0 Library  
    // {00000200-0000-0010-8000-00AA006D2EA4}  
    #ifdef IMPLIB  
    importlib("implib\\x86\\release\\ado\\msado15.dll");  
    #else  
    importlib("msado20.dll");  
    #endif  
  
    [  
      odl,  
      uuid(D80DE8B5-0001-11d1-91E6-00C04FBBBFB3),  
      version(1.0)  
    ]  
    interface IDataFactoryHandler : IUnknown  
    {  
HRESULT _stdcall GetRecordset(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] BSTR query,  
      [out, retval] _Recordset **ppRS);  
  
// DataFactory will use the ActiveConnection property  
// on the Recordset after calling Reconnect.  
   HRESULT _stdcall Reconnect(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] _Recordset *pRS);  
    };  
};  
```  
  
## <a name="see-also"></a>См. также  
 [Раздел "Подключение файла настройки"](./customization-file-connect-section.md)   
 [Раздел журналов файлов настройки](./customization-file-logs-section.md)   
 [Раздел файла настройки SQL](./customization-file-sql-section.md)   
 [Раздел UserList файла настройки](./customization-file-userlist-section.md)   
 [Настройка в отношении фактов](./datafactory-customization.md)   
 [Требуемые параметры клиента](./required-client-settings.md)   
 [Общие сведения о файле настроек](./understanding-the-customization-file.md)