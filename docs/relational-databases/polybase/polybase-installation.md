---
title: Установка PolyBase на компьютере по управлением Windows
description: Узнайте, как установить PolyBase в качестве одного узла или масштабируемой группы PolyBase. Вы можете использовать мастер установки или командную строку. Наконец, вы включите PolyBase.
ms.date: 02/05/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 6a8f7bffd47e5159c0a048a3783f3ea6647bc097
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610928"
---
# <a name="install-polybase-on-windows"></a>Установка PolyBase на компьютере по управлением Windows

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

Чтобы установить пробную версию SQL Server, перейдите на страницу [ознакомительных версий SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016). 
   
## <a name="prerequisites"></a>Предварительные требования  
   
- 64-разрядный выпуск SQL Server Evaluation.  
   
- Microsoft .NET Framework 4.5.  

- Минимальный объем памяти: 4 ГБ. 
   
- Минимум места на жестком диске: 2 ГБ.
  
- Рекомендация: как минимум 16 ГБ ОЗУ.
   
- Для корректной работы PolyBase должен быть включен протокол TCP/IP. TCP/IP включен по умолчанию во всех выпусках SQL Server, кроме Developer и Express. Для корректной работы PolyBase в выпусках Developer и Express нужно включить подключение по TCP/IP. См. раздел [Включение или отключение сетевого протокола сервера](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


>[!NOTE] 
> PolyBase можно установить только на одном экземпляре SQL Server на компьютере.


>[!NOTE]
>Чтобы использовать PolyBase, необходимо иметь роль системного администратора или разрешения на управление сервером базы данных.

>[!IMPORTANT]
>Чтобы использовать функцию передачи вычислений в Hadoop, в целевом кластере Hadoop должны быть базовые компоненты HDFS, YARN и MapReduce с включенным сервером журнала заданий. PolyBase отправляет запрос на передачу через MapReduce и получает сведения о состоянии c сервера журнала заданий. Без любого из этих компонентов запрос завершится сбоем.
  
## <a name="single-node-or-polybase-scale-out-group"></a>Один узел или масштабируемая группа PolyBase

Прежде чем устанавливать PolyBase на экземплярах SQL Server, вам следует выбрать режим этой установки: на одном узле или в [масштабируемой группе PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

Для использования масштабируемой группы PolyBase необходимо следующее.

- Все компьютеры должны быть в одном домене.
- При установке PolyBase учетная запись и пароль будут одни и те же.
- Экземпляры SQL Server могут взаимодействовать друг с другом по сети.
- Экземпляры SQL Server имеют одинаковую версию SQL Server.

Установить PolyBase можно либо на одном узле, либо в масштабируемой группе, и после установки изменить это нельзя. Для изменения потребуется удалить и вновь установить компонент.

## <a name="use-the-installation-wizard"></a>Использование мастера установки
   
1. Запустите файл setup.exe для SQL Server.   
   
2. Щелкните **Установка**, затем **Новая установка автономного SQL Server или добавление компонентов**.  
   
3. На странице выбора компонентов выберите пункт **Служба запросов PolyBase для внешних данных**.  

   ![Службы PolyBase](../../relational-databases/polybase/media/install-wizard.png "Службы PolyBase")  
   
   >[!NOTE]
   >PolyBase в SQL Server 2019 теперь содержит дополнительный параметр **Соединитель Java для источников данных HDFS**. Дополнительные сведения об этой функции см. в [блоге о функциях предварительной версии SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/).
   
4. На странице конфигурации сервера настройте **службу SQL Server PolyBase Engine** и **службу перемещения данных SQL Server PolyBase** на запуск под одной и той же учетной записью домена.  

   >[!IMPORTANT]
   >В масштабируемой группе PolyBase служба PolyBase Engine и служба перемещения данных PolyBase должны работать на всех узлах под одной учетной записью домена. См. раздел [Масштабируемые группы PolyBase](#enable).

5. На странице конфигурации PolyBase выберите один из двух вариантов. Дополнительные сведения: [Масштабируемые группы PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
   
   - Использование экземпляра SQL Server в качестве автономного экземпляра с поддержкой PolyBase.  
   
     Выберите этот вариант, чтобы использовать экземпляр SQL Server в качестве изолированного головного узла.  
   
   - Использование экземпляра SQL Server в составе масштабируемой группы PolyBase Этот вариант позволит брандмауэру разрешить входящие подключения. Будут разрешены подключения к ядру СУБД SQL Server, SQL Server PolyBase Engine, службе перемещения данных SQL Server PolyBase и обозревателю SQL. Брандмауэр также разрешит входящие подключения с других узлов в масштабируемой группе PolyBase.  
   
     Кроме того, при выборе этого варианта в брандмауэре будут включены подключения для координатора распределенных транзакций Майкрософт (MSDTC) и будут изменены параметры реестра для MSDTC.  
   
6. На странице конфигурации PolyBase укажите диапазон портов (не менее шести). Программа установки SQL Server выделяет первые шесть доступных портов из указанного диапазона.  

   >[!IMPORTANT]
   > После установки необходимо [включить компонент PolyBase](#enable).


##  <a name="use-a-command-prompt"></a><a name="installing"></a> Использование командной строки

Используйте значения из этой таблицы для создания сценариев установки. Служба SQL Server PolyBase Engine и служба перемещения данных SQL Server PolyBase должны работать под одной и той же учетной записью. В масштабируемой группе PolyBase обе службы PolyBase должны выполняться на всех узлах под одной доменной учетной записью.  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|Компонент SQL Server|Параметр и значения|Описание|  
|--------------------------|--------------------------|-----------------|  
|Управление программой установки SQL Server|**Обязательно**<br /><br /> /FEATURES=PolyBase|Выбирает компонент PolyBase.|  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCACCOUNT|Задает учетную запись для службы ядра. По умолчанию используется **NT Authority\NETWORK SERVICE**.|  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCPASSWORD|Задает пароль для учетной записи службы ядра.|  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCSTARTUPTYPE|Задает режим запуска для PolyBase Engine: Automatic (Автоматически, используется по умолчанию), Disabled (Отключено) или Manual (Вручную).|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCACCOUNT|Задает учетную запись для службы перемещения данных. По умолчанию используется **NT Authority\NETWORK SERVICE**.|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCPASSWORD|Задает пароль для учетной записи службы перемещения данных.|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCSTARTUPTYPE|Задает режим запуска для службы перемещения данных: Automatic (Автоматически, используется по умолчанию), Disabled (Отключено) или Manual (Вручную).|  
|PolyBase|**Необязательно**<br /><br /> /PBSCALEOUT|Указывает, используется ли этот экземпляр SQL Server в составе масштабируемой вычислительной группы PolyBase. <br />Поддерживаемые значения: True, False.|  
|PolyBase|**Необязательно**<br /><br /> /PBPORTRANGE|Указывает диапазон портов (не менее шести) для служб PolyBase. Пример<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|Компонент SQL Server|Параметр и значения|Описание|  
|--------------------------|--------------------------|-----------------|  
|Управление программой установки SQL Server|**Обязательно**<br /><br /> /FEATURES=PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore обеспечивает поддержку всех возможностей PolyBase, кроме подключения к Hadoop. PolyBaseJava обеспечивает подключение к Hadoop. PolyBase обеспечивает поддержку всех возможностей. |  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCACCOUNT|Задает учетную запись для службы ядра. По умолчанию используется **NT Authority\NETWORK SERVICE**.|  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCPASSWORD|Задает пароль для учетной записи службы ядра.|  
|Компонент SQL Server PolyBase Engine|**Необязательно**<br /><br /> /PBENGSVCSTARTUPTYPE|Задает режим запуска для PolyBase Engine: Automatic (Автоматически, используется по умолчанию), Disabled (Отключено) или Manual (Вручную).|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCACCOUNT|Задает учетную запись для службы перемещения данных. По умолчанию используется **NT Authority\NETWORK SERVICE**.|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCPASSWORD|Задает пароль для учетной записи службы перемещения данных.|  
|Перемещение данных SQL Server PolyBase |**Необязательно**<br /><br /> /PBDMSSVCSTARTUPTYPE|Задает режим запуска для службы перемещения данных: Automatic (Автоматически, используется по умолчанию), Disabled (Отключено) или Manual (Вручную).|  
|PolyBase|**Необязательно**<br /><br /> /PBSCALEOUT|Указывает, используется ли этот экземпляр SQL Server в составе масштабируемой вычислительной группы PolyBase. <br />Поддерживаемые значения: True, False.|  
|PolyBase|**Необязательно**<br /><br /> /PBPORTRANGE|Указывает диапазон портов (не менее шести) для служб PolyBase. Пример<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

После установки необходимо [включить компонент PolyBase](#enable).



**Пример**

Далее представлен пример сценария установки.  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> Включение PolyBase

Завершив установку, включите компонент PolyBase для доступа к его функциям. Используйте следующую команду Transact-SQL. Для экземпляров SQL 2019, развернутых во время установки кластера больших данных, этот параметр по умолчанию включен.


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```
## <a name="post-installation-notes"></a>Примечания после установки  

PolyBase устанавливает три пользовательские базы данных: DWConfiguration, DWDiagnostics и DWQueue. Эти базы данных предназначены для PolyBase. Не изменяйте и не удаляйте их.  

> [!CAUTION]
> При добавлении PolyBase к существующей установке SQL Server будут установлены компоненты на уровне версии установочного носителя, который может относиться к другим компонентам SQL Server. Это может привести к непредвиденному поведению или ошибкам. Всегда следуйте инструкциям по установке компонента PolyBase, переведя новый компонент на тот же уровень версии. При необходимости установите пакеты обновления (SP), накопительные обновления (CU) и (или) выпуски для общего распространения (GDR). Сведения о том, как определить версию PolyBase, см. в статье [Определение уровня версий, выпусков и обновлений SQL Server и компонентов](/troubleshoot/sql/general/determine-version-edition-update-level#polybase).
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> Как подтвердить установку  

Выполните следующую команду. Если служба PolyBase установлена, возвращается значение 1. В противном случае возвращается 0.  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>Правила брандмауэра  

Программа установки SQL Server PolyBase создает на компьютере следующие правила брандмауэра:  
   
- SQL Server PolyBase — ядро СУБД — \<SQLServerInstanceName> (TCP-In)  
   
- SQL Server PolyBase — службы PolyBase — \<SQLServerInstanceName> (TCP-In)  

- SQL Server PolyBase — обозреватель SQL — (UDP вход.).  
   
Эти правила активируются во время установки, если экземпляр SQL Server входит в масштабируемую группу PolyBase. Брандмауэр будет открыт для входящих подключений. Будут разрешены подключения к ядру СУБД SQL Server, SQL Server PolyBase Engine, службе перемещения данных SQL Server PolyBase и обозревателю SQL. Но если во время установки служба брандмауэра на компьютере не запущена, программа установки SQL Server не сможет включить эти правила. В этом случае запустите службу брандмауэра и включите эти правила после установки.  
   
#### <a name="to-enable-the-firewall-rules"></a>Включение правил брандмауэра  

1. Откройте **Панель управления**.  

2. Щелкните **Система и безопасность** и выберите **Брандмауэр Windows**.  
   
3. Щелкните **Дополнительные параметры**, а затем выберите **Правила для входящих подключений**.  
   
4. Щелкните отключенное правило правой кнопкой мыши и выберите **Включить правило**.  
   
### <a name="polybase-service-accounts"></a>Учетные записи служб PolyBase

Чтобы изменить учетные записи служб для PolyBase Engine и служб перемещения данных PolyBase, удалите и вновь установите компонент PolyBase.

## <a name="next-steps"></a>Дальнейшие действия  

См. раздел [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md).