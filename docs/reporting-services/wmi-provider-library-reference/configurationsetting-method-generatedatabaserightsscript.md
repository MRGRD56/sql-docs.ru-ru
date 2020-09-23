---
description: Метод GenerateDatabaseRightsScript (WMI MSReportServer_ConfigurationSetting)
title: Метод GenerateDatabaseRightsScript (WMI MSReportServer_ConfigurationSetting) | Документы Майкрософт
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- GenerateDatabaseRightsScript (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- GenerateDatabaseRightsScript method
ms.assetid: f2e6dcc9-978f-4c2c-bafe-36c330247fd0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3a54cd6367cea9caf2f72ec7412d15e878233a51
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423268"
---
# <a name="configurationsetting-method---generatedatabaserightsscript"></a>Метод ConfigurationSetting — GenerateDatabaseRightsScript
  Создает скрипт SQL, с помощью которого пользователям могут предоставляться права доступа к базе данных сервера отчетов и другим базам данных, необходимым для работы сервера отчетов. Ожидается, что участник соединится с базой данных сервера отчетов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и выполнит скрипт.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
Public Sub GenerateDatabaseRightsScript(ByVal UserName As String, _  
    ByVal DatabaseName As String, ByVal IsRemote As Boolean, _  
    ByVal IsWindowsUser As Boolean, ByRef Script As String, _  
    ByRef HRESULT As Int32)  
```  
  
```csharp  
public void GenerateDatabaseRightsScript(string UserName, string DatabaseName, bool IsRemote, bool IsWindowsUser, out string Script,   
out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>Параметры  
 *UserName*  
 Имя пользователя или идентификатор безопасности Windows пользователя, которому скрипт предоставляет права.  
  
 *DatabaseName*  
 Имя базы данных, доступ к которой скрипт предоставит пользователю.  
  
 *IsRemote*  
 Логическое значение. Показывает, является ли база данных удаленной по отношению к серверу отчетов.  
  
 *IsWindowsUser*  
 Логическое значение, которое показывает тип пользователя для указанного имени пользователя: Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 *Скрипт*  
 [out] Строка, которая содержит созданный скрипт [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 *HRESULT*  
 [out] Значение, которое указывает, окончился ли вызов успехом или сбоем.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает значение *HRESULT* , являющееся признаком успешного или неуспешного завершение вызова метода. Значение 0 указывает, что вызов метода завершился успешно. Ненулевое значение указывает, что произошла ошибка.  
  
## <a name="remarks"></a>Remarks  
 Если параметр *DatabaseName* пуст, *IsRemote* пропускается и значение файла конфигурации сервера отчетов используется в качестве имени базы данных.  
  
 Если параметр *IsWindowsUser* имеет значение **true**, то параметр *UserName* должен иметь вид \<domain>\\<username\>.  
  
 Если параметр *IsWindowsUser* имеет значение **true**, то созданный скрипт предоставляет пользователю права для входа на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], при этом база данных сервера отчетов задается в качестве используемой по умолчанию, а также пользователю присваивается роль **RSExec** в базе данных сервера отчетов, во временной базе данных сервера отчетов, в базе данных master и в системной базе данных MSDB.  
  
 Если параметр *IsWindowsUser* имеет значение **true**, то метод принимает в качестве входных данных стандартные идентификаторы безопасности Windows. Если предоставлен стандартный идентификатор безопасности Windows или имя учетной записи службы, то они переводятся в строку имени пользователя. Если база данных локальная, то учетная запись переводится в соответствии с представлением текущего языкового стандарта учетной записи. Если база данных удаленная, то учетная запись представляется как учетная запись компьютера.  
  
 В приведенной ниже таблице показаны переведенные учетные записи и их удаленное представление.  
  
|Переведенная учетная запись/идентификатор безопасности|Общее имя|Удаленное имя|  
|---------------------------------------|-----------------|-----------------|  
|(S-1-5-18)|Локальная система|\<Domain>\\<ComputerName\>$|  
|.\LocalSystem|Локальная система|\<Domain>\\<ComputerName\>$|  
|ComputerName\LocalSystem|Локальная система|\<Domain>\\<ComputerName\>$|  
|локальная система;|Локальная система|\<Domain>\\<ComputerName\>$|  
|(S-1-5-20)|Сетевая служба.|\<Domain>\\<ComputerName\>$|  
|NT AUTHORITY\NetworkService|Сетевая служба.|\<Domain>\\<ComputerName\>$|  
|(S-1-5-19)|Локальная служба.|Ошибка — см. ниже.|  
|NT AUTHORITY\LocalService|Локальная служба.|Ошибка — см. ниже.|  
  
 Если в [!INCLUDE[win2kfamily](../../includes/win2kfamily-md.md)]используется встроенная учетная запись, а база данных сервера отчетов удаленная, то возвращается сообщение об ошибке.  
  
 Если указана встроенная учетная запись **LocalService** и база данных сервера отчетов удаленная, то возвращается сообщение об ошибке.  
  
 Если параметр *IsWindowsUser* имеет значение true, а значение параметра *UserName* требует перевода, то поставщик WMI определяет, на каком компьютере находится база данных сервера отчетов: на этом же или на удаленном. Чтобы определить, что установка локальная, поставщик WMI проверяет свойство DatabaseServerName на значения из приведенного ниже списка. Если одно из значений совпадает, то база данных локальная. В противном случае она является удаленной. При сравнении учитывается регистр букв.  
  
|Значение параметра DatabaseServerName|Пример|  
|---------------------------------|-------------|  
|"."||  
|"(local)"||  
|"LOCAL"||  
|localhost||  
|\<Machinename>|testlab14|  
|\<MachineFQDN>|example.redmond.microsoft.com|  
|\<IPAddress>|180.012.345,678|  
  
 Если параметр *IsWindowsUser* имеет значение **true**, то поставщик WMI вызывает метод LookupAccountName для получения идентификатора безопасности для учетной записи, а затем вызывает метод LookupAccountSID, чтобы получить имя для вставки в скрипт [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это гарантирует, что имя учетной записи успешно пройдет проверку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Если параметр *IsWindowsUser* имеет значение **false**, то созданный скрипт предоставляет роль **RSExec** в базе данных сервера отчетов, во временной базе данных сервера отчетов и в базе данных MSDB.  
  
 Если параметр *IsWindowsUser* имеет значение **false**, то для успешного выполнения скрипта пользователь SQL Server уже должен существовать в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Если для сервера отчетов не задана база данных сервера отчетов, то при вызове метода GrantRightsToDatabaseUser возвращается ошибка.  
  
 Созданный скрипт поддерживает [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005 и [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
## <a name="requirements"></a>Требования  
 **Пространство имен:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>См. также:  
 [Элементы MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
