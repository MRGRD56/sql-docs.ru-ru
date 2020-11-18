---
description: Подключение к SQL Server (SybaseToSQL)
title: Подключение к SQL Serverу (SybaseToSQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to SQL Server
ms.assetid: dd368a1a-45b0-40e9-b4d3-5cdb48c26606
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2988f57c5d0e3162c9b6dc54d8a7371efcf5da05
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870428"
---
# <a name="connecting-to-sql-server-sybasetosql"></a>Подключение к SQL Server (SybaseToSQL)

Чтобы перенести адаптивные серверные базы данных Sybase (ASE) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , необходимо подключиться к целевому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . При подключении SSMA получает метаданные обо всех базах данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и отображает метаданные базы данных в **обозревателе метаданных SQL Server**. SSMA хранит сведения о том, к какому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вы подключены, но не хранят пароли.

Подключение будет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] оставаться активным до тех пор, пока проект не будет закрыт. При повторном открытии проекта необходимо повторно подключиться к, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Если требуется активное соединение с сервером. Вы можете работать в автономном режиме, пока объекты базы данных не будут загружены в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и не перенесены.

Метаданные экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не синхронизируются автоматически. Вместо этого, если требуется обновить метаданные в **SQL Server обозревателе метаданных**, необходимо вручную обновить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] метаданные, как описано в подразделе "синхронизация метаданных SQL Server" Далее в этом разделе.

## <a name="required-sql-server-permissions"></a>Обязательные SQL Server разрешения

Для учетной записи, используемой для подключения к, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] требуются другие разрешения в зависимости от действий, выполняемых учетной записью.

- Чтобы преобразовать объекты ASE в [!INCLUDE[tsql](../../includes/tsql-md.md)] синтаксис, обновить метаданные из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или сохранить преобразованный синтаксис в скрипты, учетная запись должна иметь разрешение на вход в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Чтобы загрузить объекты базы данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , учетная запись должна быть членом роли базы данных **db_ddladmin** .

- Для переноса данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учетную запись должны быть:
  - Член роли базы данных **db_owner** , если используется модуль миграции данных на стороне клиента.
  - Член роли сервера **sysadmin** , если используется модуль миграции данных на стороне сервера. Это необходимо для создания `CmdExec` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] шага задания агента во время переноса данных для запуска средства SSMAного копирования.
    > [!NOTE]
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Учетные записи-посредники агентов не поддерживаются при переносе данных на стороне сервера.

- Для запуска кода, созданного SSMA, учетная запись должна иметь `EXECUTE` разрешения для всех определяемых пользователем функций в схеме **ssma_syb** целевой базы данных. Эти функции предоставляют эквивалентные функции системных функций ASE и используются преобразованными объектами.

## <a name="establishing-a-sql-server-connection"></a>Установка подключения SQL Server

Перед преобразованием объектов базы данных ASE в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] синтаксис необходимо установить соединение с экземпляром, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] котором нужно перенести базу данных ASE или базы данных.

При определении свойств соединения также указывается база данных, в которую будут перенесены объекты и данные. Это сопоставление можно настроить на уровне схемы ASE после подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. [в разделе сопоставление схем ASE Sybase с SQL Server схемами &#40;&#41;SybaseToSQL ](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).

> [!IMPORTANT]
> Прежде чем пытаться подключиться к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , убедитесь, что экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] работает и может принимать подключения.

Подключение к диспетчеру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:
  
1. В меню **файл** выберите **подключиться к SQL Server**.
   Если ранее вы подключились к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , имя команды будет **повторно подключено к SQL Server**.

2. В диалоговом окне Соединение введите или выберите имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
   - При подключении к экземпляру по умолчанию на локальном компьютере можно ввести `localhost` или точку ( `.` ).
   - При подключении к экземпляру по умолчанию на другом компьютере введите имя компьютера.
   - При подключении к именованному экземпляру на другом компьютере введите имя компьютера, затем обратную косую черту, а затем имя экземпляра, например `MyServer\MyInstance` .

3. Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настроен для приема подключений по нестандартному порту, введите номер порта, используемый для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] соединений, в поле **порт сервера** . Для экземпляра по умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] номер порта по умолчанию — 1433. Для именованных экземпляров SSMA попытается получить номер порта из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] службы браузера.

4. В поле **база данных** введите имя целевой базы данных.
   Этот параметр недоступен при повторном подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. В поле **Проверка подлинности** выберите тип проверки подлинности, который будет использоваться для соединения. Чтобы использовать текущую учетную запись Windows, выберите **Проверка подлинности Windows**. Чтобы использовать имя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] входа, выберите **SQL Server проверка подлинности** , а затем укажите имя входа и пароль.

6. Для безопасного подключения добавляются два элемента управления: флажки **Шифровать соединение** и **TrustServerCertificate** . Флажок **TrustServerCertificate** отображается только при установленном **шифровании соединения** . Если флажок **Шифровать соединение** установлен (true) и **TrustServerCertificate** не установлен (false), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сертификат SSL будет проверен. Проверка сертификата сервера является частью SSL-подтверждения и гарантирует, что для подключения выбран правильный сервер. Чтобы убедиться в этом, сертификат должен быть установлен на стороне клиента, а также на стороне сервера.

7. Нажмите кнопку **Соединить**.

> [!IMPORTANT]
> Хотя вы можете подключаться к более поздней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , по сравнению с версией, выбранной при создании проекта миграции, преобразование объектов базы данных определяется целевой версией проекта, а не версией, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] к которой вы подключены.

## <a name="reconnecting-to-sql-server"></a>Повторное подключение к SQL Server

Подключение будет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] оставаться активным до тех пор, пока проект не будет закрыт. При повторном открытии проекта необходимо повторно подключиться к, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Если требуется активное соединение с сервером. Вы можете работать в автономном режиме до обновления метаданных, загрузки объектов базы данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и переноса данных.

Процедура повторного подключения к совпадает с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] процедурой установки соединения.

## <a name="synchronizing-sql-server-metadata"></a>Синхронизация метаданных SQL Server

Метаданные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] баз данных не обновляются автоматически. Метаданные в **SQL Server обозревателе метаданных** являются моментальным снимком метаданных при первом соединении с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или при последнем обновлении метаданных вручную. Можно вручную обновить метаданные для всех баз данных или для любой отдельной базы данных или объекта базы данных. Для синхронизации метаданных:

1. Убедитесь, что вы подключены к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

2. В **SQL Server обозревателе метаданных** установите флажок рядом с базой данных или схемой базы данных, которую требуется обновить.
   Например, чтобы обновить метаданные для всех баз данных, установите флажок рядом с пунктом **databases (базы данных**).

3. Щелкните правой кнопкой мыши **базы** данных или отдельную базу данных или схему базы данных, а затем выберите **синхронизировать с базой данных**.

## <a name="next-step"></a>Следующий шаг

Следующий шаг миграции зависит от потребностей проекта:

- Если вы хотите настроить сопоставление между базами данных и схемами и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базами данных и схемами ASE, см. раздел [сопоставление схем Sybase ASE с SQL Server схемами &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).
- Если вы хотите настроить параметры конфигурации для проектов, см. раздел [Установка параметров проекта &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).
- Если вы хотите настроить сопоставление исходных и целевых типов данных, см. раздел [сопоставления типов данных SYBASE ASE и SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).
- Если это не требуется, можно преобразовать определения объектов базы данных Sybase ASE в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определения объектов. Дополнительные сведения см. в статье [Преобразование объектов базы данных SYBASE ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).

## <a name="see-also"></a>См. также:

[Миграция баз данных Sybase ASE в SQL Server — база данных SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
