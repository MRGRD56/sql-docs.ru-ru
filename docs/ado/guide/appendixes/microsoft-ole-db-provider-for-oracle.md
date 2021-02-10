---
description: Обзор поставщик OLE DB для Oracle (Майкрософт)
title: поставщик OLE DB для Oracle (Майкрософт) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for Oracle
- OLE DB provider for Oracle [ADO]
- Oracle provider [ADO]
ms.assetid: 44fae9dd-5585-4cd6-8bbd-3248a78931b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 04865a03bdb352d36e1ac3b445c7c0b4eb7c3da2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029298"
---
# <a name="microsoft-ole-db-provider-for-oracle-overview"></a>Обзор поставщик OLE DB для Oracle (Майкрософт)
> [!IMPORTANT]
>  Эта функция будет удалена в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Вместо этого используйте поставщик OLE DB Oracle.

 Поставщик OLE DB для Oracle (Майкрософт) позволяет ADO получать доступ к базам данных Oracle.

## <a name="connection-string-parameters"></a>Параметры строки соединения
 Чтобы подключиться к поставщику, задайте для аргумента *поставщика* свойства [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) значение:

```vb
MSDAORA
```

 При чтении свойства [поставщика](../../reference/ado-api/provider-property-ado.md) также будет возвращена эта строка.

 Если запрос на соединение с набором ключей или динамическим курсором выполняется в базе данных Oracle, возникает ошибка. Oracle поддерживает только статический курсор только для чтения.

## <a name="typical-connection-string"></a>Типичная строка подключения
 Типичная строка подключения для этого поставщика:

```vb
"Provider=MSDAORA;Data Source=serverName;User ID=MyUserID; Password=MyPassword;"
```

 Строка состоит из следующих ключевых слов:

|Ключевое слово|Описание|
|-------------|-----------------|
|**Поставщик**|Указывает поставщика OLE DB для Oracle.|
|**Источник данных**|Указывает имя сервера.|
|**Идентификатор пользователя**|Указывает имя пользователя.|
|**Пароль**|Указывает пароль пользователя.|

> [!NOTE]
>  При подключении к поставщику источника данных, который поддерживает проверку подлинности Windows, следует указать **Trusted_Connection = Yes** или **Integrated Security = SSPI** вместо сведений об идентификаторе пользователя и пароле в строке подключения.

## <a name="provider-specific-connection-parameters"></a>Параметры подключения Provider-Specific
 Поставщик поддерживает несколько параметров соединения, зависящих от поставщика, помимо тех, которые определены в ADO. Как и в случае со свойствами соединения ADO, эти свойства, зависящие от поставщика, можно задать через коллекцию [свойств](../../reference/ado-api/properties-collection-ado.md) [соединения](../../reference/ado-api/connection-object-ado.md) или как часть **ConnectionString**.

 Эти параметры полностью описаны в [справочнике по программисту OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)). [Индекс динамического свойства ADO](../../reference/ado-api/ado-dynamic-property-index.md) обеспечивает перекрестную ссылку между этими именами параметров и соответствующими свойствами OLE DB.

|Параметр|Описание|
|---------------|-----------------|
|**Дескриптор окна**|Указывает описатель окна, используемый для запроса дополнительных сведений.|
|**Идентификатор локали**|Указывает уникальное 32-разрядное число (например, 1033), которое указывает параметры, относящиеся к языку пользователя. Эти настройки указывают, как форматируются даты и время, элементы сортируются в алфавитном порядке, сравниваются по строкам и т. д.|
|**Службы OLE DB**|Указывает битовую маску, указывающую OLE DB службы для включения или отключения.|
|**Командная строка**.|Указывает, следует ли запрашивать пользователя во время установки соединения.|
|**Расширенные свойства**|Строка, содержащая сведения о расширенном соединении, зависящие от поставщика. Это свойство используется только для специфичных для поставщика сведений о соединении, которые не могут быть описаны с помощью механизма свойств.|

## <a name="see-also"></a>См. также:
 Property ( [ADO) свойство](../../reference/ado-api/connectionstring-property-ado.md) [поставщика](../../reference/ado-api/provider-property-ado.md) (ADO) [объект Recordset Object (](../../reference/ado-api/recordset-object-ado.md) ADO)