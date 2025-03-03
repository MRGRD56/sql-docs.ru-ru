---
title: Настройка распределенных транзакций для группы доступности
description: 'В этой статье описывается настройка распределенных транзакций для баз данных в группе доступности Always On. '
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: da3071f14be97a4bbbd9ac909926f210c49504d4
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231522"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>Настройка распределенных транзакций для группы доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] поддерживает все распределенные транзакции, включая базы данных в группе доступности. В этой статье рассказывается, как настроить группу доступности для распределенных транзакций.  

Для выполнения распределенных транзакций группу доступности необходимо настроить таким образом, чтобы базы данных регистрировались как диспетчеры ресурсов распределенных транзакций.  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2) и более поздние версии включают полную поддержку распределенных транзакций в группах доступности. В версиях [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] до пакета обновления 2 (SP2) распределенные транзакции между базами данных (т. е. транзакции между базами данных в одном экземпляре SQL Server), включающие базу данных в группе доступности, не поддерживаются. В [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] подобного ограничения нет. 
>
>Конфигурация [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] настраивается точно так же, как и конфигурация [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)].

В распределенной транзакции клиентские приложения работают с координатором распределенных транзакций Майкрософт (MS DTC или DTC), что позволяет обеспечить согласованность транзакций между несколькими источниками данных. DTC — это служба, доступная в поддерживаемых операционных системах Windows Server. Для распределенных транзакций DTC выступает как *координатор*. Экземпляр SQL Server, как правило, служит *диспетчером ресурсов*. У каждой базы данных, которая не входит в группу доступности, должен быть собственный диспетчер ресурсов. 

[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] не запрещает распределенные транзакции для баз данных в группе доступности, даже если эта группа доступности не настроена для распределенных транзакций. При этом в случае, если группа доступности не настроена для распределенных транзакций, отработка отказа может завершаться ошибкой. В частности, экземпляр новой первичной реплики [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] может не получить результат транзакции из DTC. Чтобы включить экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] и получить результат сомнительной транзакции из DTC после отработки отказа, настройте группу доступности для распределенных транзакций. 

DTC не участвует в обработке групп доступности, если только база данных не входит также в состав отказоустойчивого кластера. В группе доступности согласованность между репликами поддерживается логикой группы доступности. Первичная реплика не завершает фиксацию и не подтверждает ее вызывающей стороне, пока вторичная реплика не подтвердит, что она сохранила записи журнала в долговременном хранилище. Только после этого первичная реплика объявит транзакцию завершенной. В асинхронном режиме мы не ждем подтверждения от вторичной реплики, поэтому есть вероятность потери небольшого объема данных.

## <a name="prerequisites"></a>Предварительные требования

Перед настройкой группы доступности для поддержки распределенных транзакций необходимо выполнить следующие предварительные требования:

* Все экземпляры [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], участвующие в распределенной транзакции, должны иметь версию [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] или более позднюю.

* Группы доступности должны быть запущены на Windows Server 2016 или Windows Server 2012 R2. Для Windows Server 2012 R2 необходимо установить обновление KB3090973, доступное по адресу [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  

## <a name="create-an-availability-group-for-distributed-transactions"></a>Создание группы доступности для распределенных транзакций

Настройте группу доступности для поддержки распределенных транзакций. Разрешите группе доступности регистрировать каждую базу данных как диспетчер ресурсов. В этой статье рассказывается, как настроить группы доступности таким образом, чтобы каждая база данных могла быть диспетчером ресурсов в DTC.



Группу доступности для распределенных транзакций можно создать в [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] или более поздней версии. Чтобы создать группу доступности для распределенных транзакций, включите в определении группы доступности `DTC_SUPPORT = PER_DB`. Представленный ниже скрипт создает группу доступности для распределенных транзакций. 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>Данный скрипт представляет собой простой пример группы доступности и не предназначен для какой-либо определенной рабочей среды. 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>Изменение группы доступности для распределенных транзакций

Группу доступности для распределенных транзакций можно изменить в [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] или более поздней версии. Чтобы изменить группу доступности для распределенных транзакций, включите `DTC_SUPPORT = PER_DB` в скрипт `ALTER AVAILABILITY GROUP`. Скрипт в данном примере изменяет группу доступности, позволяя ее поддерживать распределенные транзакции. 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>Начиная с версии [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] с пакетом обновления 2 (SP2), группу доступности для распределенных транзакций можно изменять. В версиях [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] до пакета обновления 2 (SP2) потребуется удалить и вновь создать группу доступности с параметром `DTC_SUPPORT = PER_DB`. 

Чтобы отключить распределенные транзакции, используйте следующую команду Transact-SQL:

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>Распределенные транзакции: технические понятия

Распределенная транзакция охватывает две базы данных или больше. Как диспетчер транзакций, DTC координирует транзакцию между экземплярами SQL Server и другими источниками данных. В качестве диспетчера ресурсов может выступать любой экземпляр компонента Database Engine [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. После настройки группы доступности в `DTC_SUPPORT = PER_DB` в качестве диспетчеров ресурсов могут выступать и базы данных. Дополнительные сведения см. в документации по MS DTC.

Транзакция с двумя или несколькими базами данных в отдельном экземпляре компонента Database Engine, по сути, является распределенной транзакцией. Экземпляр управляет распределенной транзакцией на внутреннем уровне, для пользователя она действует как локальная транзакция. Если базы данных находятся в группе доступности, настроенной с `DTC_SUPPORT = PER_DB` (даже в пределах одного экземпляра SQL Server), [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] передает все межбазовые транзакции в DTC. 

В приложении управление распределенной транзакцией во многом похоже на управление локальной. В конце транзакции приложение запрашивает ее фиксацию или откат. Распределенной фиксацией диспетчер транзакций должен управлять иначе, чтобы свести к минимуму риск сбоя сети, в результате которого одни диспетчеры ресурсов могут фиксировать транзакцию, тогда как другие будут выполнять ее откат. Выход из положения заключается в двухфазном процессе фиксации (фаза подготовки и фаза фиксации), который называется двухфазной фиксацией.

- **Фаза подготовки**
   
   Когда диспетчер транзакции получает запрос на фиксацию, он отправляет команду подготовки всем диспетчерам ресурсов, занятым в транзакции. Каждый диспетчер ресурсов всемерно обеспечивает устойчивость транзакции, а все буферы, в которых хранятся образы журналов для этой транзакции, записываются на диск. По мере того, как каждый диспетчер ресурсов завершает фазу подготовки, он возвращает диспетчеру транзакций значение успешного или неуспешного завершения подготовки.

- **Фаза фиксации**
   
   Если диспетчер транзакций получает значения успешного завершения подготовки от всех диспетчеров ресурсов, то он отправляет команду фиксации каждому диспетчеру ресурсов. После этого диспетчеры ресурсов могут завершить фиксацию. Если все диспетчеры ресурсов сообщают об успешной фиксации, то диспетчер транзакций отправляет уведомление приложению. Если какой-либо диспетчер ресурсов сообщил о неуспешном завершении подготовки, то диспетчер транзакций отправляет команду отката всем диспетчерам ресурсов и сообщает приложению о сбое фиксации.

### <a name="detailed-steps"></a>Подробные инструкции

Представленный ниже список показывает, как приложение взаимодействует с DTC для выполнения распределенных транзакций.

1. Экземпляр SQL Server указывается в транзакции DTC. Это может произойти, если в транзакции участвуют сразу несколько диспетчеров ресурсов либо клиент запрашивает транзакцию, которая получает статус транзакции DTC.
2. Клиент выполняет определенные действия в экземпляре SQL Server в рамках транзакции DTC.
3. Клиент запускает фиксацию или отмену транзакции DTC.
    - Если клиент запускает отмену, транзакция незамедлительно прерывается.
    - Если клиент запускает фиксацию, DTC начинает протокол двухфазной фиксации, отправляя всем диспетчерам ресурсов запрос о подготовке транзакции.
4. После того как все диспетчеры ресурсов подтвердят завершение стадии подготовки, DTC сообщает всем диспетчерам ресурсов о том, что транзакция зафиксирована. Если по какой-то причине подтверждение не поступает, DTC прерывает транзакцию. 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>Результаты настройки группы доступности для распределенных транзакций

Каждая сущность, участвующая в распределенной транзакции, называется диспетчером ресурсов. Примеры диспетчеров ресурсов:

* Экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. 
* База данных в группе доступности, настроенная для распределенных транзакций.
* Служба DTC — также может быть диспетчером транзакций.
* Другие источники данных. 

Для участия в распределенных транзакциях экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] присоединяется к DTC. Обычно экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] присоединяется к DTC на локальном сервере. Каждый экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] создает диспетчер ресурсов с уникальным идентификатором диспетчера ресурсов (RMID) и регистрирует его в DTC. В конфигурации по умолчанию все базы данных в экземпляре [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] используют один и тот же RMID. 

Если база данных находится в группе доступности, копию этой базы данных, доступную для чтения и записи, либо ее первичную реплику можно переместить в другой экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. Чтобы обеспечить поддержку распределенных транзакций во время такого перемещения, каждая база данных должна выступать как отдельный диспетчер ресурсов и иметь уникальный номер RMID. Если группа доступности имеет `DTC_SUPPORT = PER_DB`, [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] создает диспетчер ресурсов для каждой базы данных и регистрируется в DTC под уникальным номером RMID. В этой конфигурации базы данных является диспетчером ресурсов для транзакций DTC.

>[!IMPORTANT]
>Обратите внимание, что DTC имеет ограничение в 32 прикрепления для каждой распределенной транзакции. Так как каждая база данных в группе доступности прикрепляется к DTC отдельно, если транзакция включает более 32 баз данных, при попытке [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] прикрепить 33-ю базу данных может возникнуть следующая ошибка:
>
>`Enlist operation failed: 0x8004d101(XACT_E_TOOMANY_ENLISTMENTS). SQL Server could not register with Microsoft Distributed Transaction Coordinator (MS DTC) as a resource manager for this transaction. The transaction may have been stopped by the client or the resource manager.`

Дополнительные сведения о распределенных транзакциях в [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] см. в статье [Распределенные транзакции](#distTran)

## <a name="manage-unresolved-transactions"></a>Управление неразрешенными транзакциями

После отработки отказа результаты активных транзакций, выполненных во время изменения RMID, восстановить нельзя. Это связано с тем, что для присоединения и восстановления используются разные экземпляры SQL Server RMID. RMID может измениться в следующих случаях:

* Изменение `DTC_SUPPORT` для группы доступности. 
* Добавление базы данных в группы доступности и удаление из нее. 
* Сброс группы доступности.

Если в перечисленных выше случаях после отработки отказа первичная реплика переходит в новый экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], этот экземпляр пытается связаться с DTC, чтобы определить результат транзакции. При этом DTC не может вернуть результат, поскольку RMID базы данных, используемый для получения результата сомнительной транзакции во время восстановления, ранее не был присоединен. В связи с этим база данных переходит в состояние SUSPECT.

Новый журнал ошибок [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] содержит запись, которая имеет следующий вид:

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

Приведенный выше пример показывает, что DTC не может повторно присоединить базу данных из новой первичной реплики к транзакции, созданной после отработки отказа. Экземпляр [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] не может определить результат распределенной транзакции и отмечает базу данных как сомнительную. Транзакция помечается как единица работы (UOW) и обозначается с помощью GUID. Чтобы восстановить базу данных, зафиксируйте транзакцию или выполните отказ вручную. 

>[!WARNING]
>Ручная фиксация или откат транзакции могут негативно сказаться на приложении. Убедитесь, что данная операция соответствует требованиям вашего приложения. 

Выполните только один из представленных ниже скриптов.

   * Чтобы зафиксировать транзакцию, обновите и выполните следующий скрипт, заменив `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` на UOW сомнительной транзакции из полученного ранее сообщения об ошибке и выполнив следующий код:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * Чтобы выполнить откат транзакции, обновите и выполните следующий скрипт, заменив `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` на UOW сомнительной транзакции из полученного ранее сообщения об ошибке и выполнив следующий код:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

После фиксации или отката транзакции базу данных можно перевести в режим онлайн, используя `ALTER DATABASE`. Обновите и выполните следующий скрипт, указав имя базы данных вместо имени сомнительной базы данных:

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

Дополнительные сведения об обработке сомнительных транзакций см. в статье [Обработка транзакций вручную](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10)).

## <a name="next-steps"></a>Next Steps  

[Распределенные транзакции](/dotnet/framework/data/adonet/distributed-transactions)

[Группы доступности Always On: взаимодействие (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[Транзакции — группы доступности AlwaysOn и зеркальное отображение баз данных](transactions-always-on-availability-and-database-mirroring.md)  

[Поддержка транзакций XA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10))

[Как это работает: сеанс/SPID (–2) для транзакций DTC](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions)
