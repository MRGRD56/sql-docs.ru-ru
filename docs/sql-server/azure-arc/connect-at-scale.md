---
title: Масштабное подключение экземпляров SQL Server к службе Azure Arc
titleSuffix: ''
description: В этой статье рассказывается, как подключить экземпляры SQL Server в качестве серверов SQL Server с поддержкой Azure Arc (предварительная версия) с помощью субъекта-службы.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: f7f3c6f2169338c8105dbeba6197576365c71e14
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556337"
---
# <a name="connect-sql-server-instances-to-azure-arc-at-scale"></a>Масштабное подключение экземпляров SQL Server к службе Azure Arc

Вы можете подключить к Azure Arc несколько экземпляров SQL Server, установленных на нескольких компьютерах Windows или Linux, с помощью того же [скрипта, который был создан для одного компьютера](connect.md). Этот скрипт выполнит подключение и регистрацию каждого компьютера и установленных на нем экземпляров SQL Server в службе Azure Arc. Для оптимальной работы рекомендуется использовать[субъект-службу](/azure/active-directory/develop/app-objects-and-service-principals) Azure Active Directory. Субъект-служба — это специальное ограниченное удостоверение управления, которому предоставляются только минимальные разрешения, необходимые для подключения компьютеров к Azure и создания ресурсов Azure для сервера с поддержкой Azure Arc и SQL Server с поддержкой Azure Arc. Это более безопасно, чем использование учетной записи с более высоким уровнем привилегий, такой как администратор клиента, и соответствует рекомендациям по обеспечению безопасности управления доступом.  

Методы установки и настройки агента Connected Machine требуют, чтобы для автоматизированного метода на компьютерах вам были предоставлены разрешения администратора. В Linux для этого используется корневая учетная запись, а в Windows следует быть членом группы локальных администраторов.

Перед началом работы ознакомьтесь с [предварительными требованиями](overview.md#prerequisites) и убедитесь, что создана настраиваемая роль с необходимыми разрешениями.

## <a name="connecting-multiple-sql-server-instances-on-windows-using-azure-powershell"></a>Подключение нескольких экземпляров SQL Server в Windows с помощью Azure PowerShell

На каждом компьютере должен быть установлен [Azure PowerShell](/powershell/azure/install-az-ps).

1. Создайте субъект-службу с помощью командлета [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) в PowerShell. Обязательно сохраните выходные данные в переменной. Иначе вы не сможете извлечь пароль, который вам понадобится позже.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role <your custom role>
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

   > [!NOTE]
   > При создании субъекта-службы ваша учетная запись должна быть владельцем или администратором доступа пользователей в подписке, которую вы хотите использовать для подключения. Если у вас нет достаточных разрешений для создания назначений ролей, вы сможете создать субъект-службу, но вам не удастся подключить компьютеры. Инструкции по созданию настраиваемой роли приведены в разделе [Необходимые разрешения](overview.md#required-permissions).

2. Извлеките пароль, сохраненный в переменной `$sp`:

   ```azurepowershell-interactive
   $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
   $credential.GetNetworkCredential().password
   ```
3. Получите значение идентификатора клиента субъекта-службы:
 
   ```azurepowershell-interactive
   $tenantId= (Get-AzContext).Tenant.Id
   ```
4. Скопируйте и сохраните пароль, идентификатор приложения и идентификатор клиента, учитывая соответствующие рекомендации по безопасности. Если вы забудете или потеряете пароль имени субъекта-службы, его можно сбросить с помощью командлета [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential).

   > [!NOTE]
   > Обратите внимание, что в настоящее время служба Azure Arc для серверов не поддерживает вход с помощью сертификата, поэтому для проверки подлинности у субъекта-службы должен быть секрет.

5. Скачайте скрипт PowerShell с портала, следуя инструкциям в разделе [Подключение SQL Server к Azure Arc](connect.md).

6. Откройте скрипт в экземпляре администратора PowerShell ISE и замените приведенные ниже переменные среды на значения, созданные при подготовке субъекта-службы, которая описана выше. Эти переменные изначально пустые.

   ```azurepowershell-interactive
   $servicePrincipalAppId="{serviceprincipalAppID}"
   $servicePrincipalSecret="{serviceprincipalPassword}"
   $servicePrincipalTenantId="{serviceprincipalTenantId}"
   ```

7. Выполнение скрипта на целевом компьютере

## <a name="connecting-multiple-sql-server-instances-on-linux-using-azure-cli"></a>Подключение нескольких экземпляров SQL Server в Linux с помощью Azure CLI

На каждом целевом компьютере должен быть установлен [Azure CLI](/cli/azure/install-azure-cli). Скрипт регистрации автоматически входит в Azure с учетными данными субъекта-службы, если они были предоставлены и никакой другой пользователь еще не вошел. Чтобы подключить экземпляры SQL Server на нескольких компьютерах Linux, выполните следующие действия.

1. Создайте субъект-службу с помощью команды ["az ad sp create-for-rbac"](/cli/azure/ad/sp#az_ad_sp_create_for_rbac).

   ```azurecli-interactive
   az ad sp create-for-rbac --name <your service principal name> --role <your custom role name>
   ```

   ```output
   { "appId": "d2ff754a-e10a-4eb6-9cdc-ce6e7a4687db",
     "displayName": "Arc-for-servers",
     "name": "http://Arc-for-servers",
     "password": {SomeRandomlyGeneratedPassword}",
     "tenant": "2530e75f-673b-4841-8270-47ca6a34ef4f"
   }
   ```

   > [!NOTE]
   > При создании субъекта-службы ваша учетная запись должна быть владельцем или администратором доступа пользователей в подписке, которую вы хотите использовать для подключения. Если у вас нет достаточных разрешений для создания назначений ролей, вы сможете создать субъект-службу, но вам не удастся подключить компьютеры. Инструкции по созданию настраиваемой роли приведены в разделе [Необходимые разрешения](overview.md#required-permissions).

2. Скачайте скрипт оболочки Linux с портала, следуя инструкциям в разделе [Подключение SQL Server к Azure Arc](connect.md).

3. Замените следующие переменные в скрипте значениями, возвращаемыми командой "az ad sp create-for-rbac". Эти переменные изначально пустые.

   ```bash
   servicePrincipalAppId="{serviceprincipalAppID}"
   servicePrincipalSecret="{serviceprincipalPassword}"
   servicePrincipalTenant="{serviceprincipalTenant}"
   ```

3. Выполнение скрипта на целевых компьютерах
 
   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="validate-successful-onboarding"></a>Проверка успешного подключения

После регистрации экземпляров SQL Server в SQL Server с поддержкой Azure Arc (предварительная версия) перейдите на [портал Azure](https://aka.ms/azureportal) и просмотрите только что созданные ресурсы Azure ARC. Вы увидите новый ресурс __Компьютер — Azure Arc__ для каждого подключенного компьютера, а также новый ресурс __SQL Server — Azure Arc__ для каждого зарегистрированного экземпляра SQL Server. 

![Успешное подключение](./media/join-at-scale/successful-onboard.png)

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как управлять компьютером с помощью [Политики Azure](/azure/governance/policy/overview), например для [гостевой конфигурации](/azure/governance/policy/concepts/guest-configuration) виртуальной машины, проверять отправку отчетов с компьютера в ожидаемую рабочую область Log Analytics, включать мониторинг с помощью [Azure Monitor с виртуальными машинами](/azure/azure-monitor/insights/vminsights-enable-policy) и т. д.

- Узнайте больше об [агенте Log Analytics](/azure/azure-monitor/platform/log-analytics-agent). Агент Log Analytics для Windows и Linux необходим, если требуется упреждающе отслеживать ОС и рабочие нагрузки на компьютере, выполнять управление с помощью runbook службы автоматизации или с помощью решений, таких как Управление обновлениями, или использовать другие службы Azure, например, [Центр безопасности Azure](/azure/security-center/security-center-intro).

- Узнайте, как [настроить экземпляр SQL Server для периодической проверки работоспособности среды с помощью оценки SQL по запросу](assess.md)

- Узнайте, как [настроить расширенную защиту данных для экземпляра SQL Server](configure-advanced-data-security.md).
