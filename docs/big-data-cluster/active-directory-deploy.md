---
title: Развертывание в режиме Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Узнайте, как обновлять кластеры больших данных SQL Server в домене Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 02/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9417444a1c9d28181529ace79b6dcff6162b7f2d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837039"
---
# <a name="deploy-sql-server-big-data-cluster-in-active-directory-mode"></a>Развертывание кластера больших данных SQL Server в режиме Active Directory

В этой статье описывается развертывание кластера больших данных SQL Server в режиме Active Directory. Для выполнения действий, описанных в этой статье, требуется доступ к домену Active Directory. Прежде чем продолжить, выполните требования, описанные в разделе [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в режиме Active Directory](active-directory-prerequisites.md).

## <a name="prepare-deployment"></a>Подготовка развертывания

Чтобы развернуть BDC с интеграцией AD, необходимо предоставить дополнительные сведения для создания в AD связанных с кластером больших данных объектов.

Используя профиль `kubeadm-prod` (или `openshift-prod`, начиная с выхода накопительного пакета обновления 5 — CU5), вы автоматически получите заполнители для сведений о безопасности и конечных точках, которые требуются для интеграции с AD.

Кроме того, необходимо предоставить учетные данные, которые [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] будет использовать для создания необходимых объектов в AD. Эти учетные данные предоставляются в качестве переменных среды

### <a name="traffic-and-ports"></a>Трафик и порты

Убедитесь, что все брандмауэры и сторонние приложения разрешают порты, требуемые для обмена данными с Active Directory. 

![Схема трафика между кластером больших данных и Active Directory. Контроллер, служба поддержки безопасности и другие службы кластеров обмениваются данными с контроллерами доменов по протоколу LDAP и Kerberos. Служба DNS-прокси BDC обменивается данными с DNS-серверами через DNS.](media/big-data-cluster-overview/big-data-cluster-active-directory-dns-traffic-ports.png)

Запросы выполняются с использованием этих протоколов между службами кластеров Kubernetes и доменом Active Directory, поэтому должны быть разрешены входящие и исходящие подключения в любом брандмауэре или стороннем приложении, прослушивающем требуемые порты для TCP и UDP. Стандартные номера портов, которые Active Directory использует:

| Служба | Порт |
|:---|:---|
| DNS | 53 |
| LDAP <BR> LDAPS | 389<BR> 636 |
| Kerberos | 88 |
| Порт глобального каталога <BR>через LDAP<BR>через LDAPS |<BR> 3268 <BR> 3269 |

## <a name="set-security-environment-variables"></a>Задание переменных среды безопасности

Следующие переменные среды предоставляют учетные данные для учетной записи доменной службы BDC, которая будет использоваться для настройки интеграции с AD. Эта учетная запись также используется BDC для обслуживания объектов AD, связанных с BDC, в дальнейшем.

```bash
export DOMAIN_SERVICE_ACCOUNT_USERNAME=<AD principal account name>
export DOMAIN_SERVICE_ACCOUNT_PASSWORD=<AD principal password>
```

## <a name="provide-security-and-endpoint-parameters"></a>Предоставление параметров безопасности и конечных точек

В дополнение к переменным среды для учетных данных вам также потребуется предоставить сведения о безопасности и конечных точках, чтобы интеграция с AD функционировала. Необходимые параметры автоматически предоставляются в составе [профиля развертывания](deployment-guidance.md#configfile) `kubeadm-prod`/`openshift-prod`.

Для интеграции с AD требуются следующие параметры. Добавьте эти параметры в файлы `control.json` и `bdc.json` с помощью команд `config replace`, показанных ниже в этой статье. Во всех примерах ниже используется пример домена `contoso.local`.

- `security.activeDirectory.ouDistinguishedName`: различающееся имя подразделения (OU), в которое будут добавлены все учетные записи AD, созданные при развертывании кластера. Если домен называется `contoso.local`, различающееся имя подразделения имеет вид: `OU=BDC,DC=contoso,DC=local`.

- `security.activeDirectory.dnsIpAddresses`: содержит список IP-адресов DNS-серверов домена. 

- `security.activeDirectory.domainControllerFullyQualifiedDns`: Список полных доменных имен контроллера домена. Полное доменное имя содержит название компьютера/узла контроллера домена. Если у вас несколько контроллеров доменов, список можно указать здесь. Например, `HOSTNAME.CONTOSO.LOCAL`.

  > [!IMPORTANT]
  > Когда несколько контроллеров домена обслуживают домен, используйте основной контроллер домена (PDC) в качестве первой записи в списке `domainControllerFullyQualifiedDns` в конфигурации безопасности. Чтобы получить имя основного контроллера домена, введите `netdom query fsmo` в командной строке и нажмите клавишу **ВВОД**.

- `security.activeDirectory.realm` **Необязательный параметр**: в большинстве случаев область равна доменному имени. В случаях, когда они не совпадают, используйте этот параметр для определения имени области (например, `CONTOSO.LOCAL`). Значение, указанное для этого параметра, должно быть полным.

- `security.activeDirectory.netbiosDomainName` **Необязательный параметр**. NetBIOS-имя домена Active Directory. В большинстве случаев это будет первая метка доменного имени AD. В иных случаях используйте этот параметр, чтобы задать NetBIOS-имя домена. Значение не должно содержать точек. Обычно это имя используется для уточнения имен учетных записей пользователей в домене. Например, CONTOSO\user, где CONTOSO — это доменное имя в формате NetBIOS.

  > [!NOTE]
  > Поддержка конфигурации, в которой доменное имя Active Directory отличается от **NetBIOS**-имени домена Active Directory, посредством *security.activeDirectory.netbiosDomainName* появилась начиная с SQL Server 2019 с накопительным пакетом обновления 9.

- `contoso.local`: имя домена DNS, которое будет использоваться для кластера (например, `security.activeDirectory.domainDnsName`).

- `security.activeDirectory.clusterAdmins`: этот параметр принимает одну группу AD. Область действия группы AD должна быть универсальной или глобальной. Члены этой группы будут иметь роль кластера `bdcAdmin`, которая предоставит им разрешения администратора в кластере. Это означает, что у них есть [разрешения `sysadmin` в SQL Server](../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles), [разрешения `superuser` в HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The_Super-User) и разрешения администратора при подключении к конечной точке контроллера.

  >[!IMPORTANT]
  >Создайте эту группу в AD перед началом развертывания. Если область для этой группы AD является локальной доменной, развертывание завершается сбоем.

- `security.activeDirectory.clusterUsers`: Список групп AD, которые являются обычными пользователями (без прав администратора) в кластере больших данных. Список может включать группы AD, областью действия которых является универсальная или глобальная группа. Они не могут быть локальными доменными группами.

Группы Active Directory в этом списке сопоставлены с ролью кластера больших данных `bdcUser`, и им необходимо предоставить доступ к SQL Server (см. раздел [разрешения SQL Server](../relational-databases/security/permissions-hierarchy-database-engine.md)) или HDFS (см. [руководство по разрешениям HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#:~:text=Permission%20Checks%20%20%20%20Operation%20%20,%20%20N%2FA%20%2029%20more%20rows%20)). При подключении к конечной точке контроллера эти пользователи могут получить только список конечных точек, доступных в кластере, с помощью команды `azdata bdc endpoint list`.

Дополнительные сведения об обновлении групп AD для этих параметров см. в разделе [Управление доступом к кластеру больших данных в режиме AD](manage-user-access.md).

  >[!TIP]
  >Чтобы включить средства просмотра HDFS при подключении к узлу SQL Server Master в Azure Data Studio, пользователю с ролью bdcUser должны быть предоставлены разрешения VIEW SERVER STATE, так как Azure Data Studio использует DMV `sys.dm_cluster_endpoints`, чтобы получить необходимую конечную точку шлюза Knox для подключения к HDFS.

  >[!IMPORTANT]
  >Создайте эти группы в AD перед началом развертывания. Если область для любой из этих групп AD является локальной доменной, развертывание завершается сбоем.

  >[!IMPORTANT]
  >Если пользователи домена имеют большое количество членств в группах, следует настроить значения параметров шлюза `httpserver.requestHeaderBuffer` (значение по умолчанию — `8192`) и HDFS `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` (значение по умолчанию — `10`) с помощью пользовательского файла конфигурации развертывания *bdc.json*. Это позволяет избежать истечения времени ожидания подключения к шлюзу и/или получения HTTP-ответов с кодом состояния 431 (*Слишком большие поля заголовка запроса*). Ниже приведен раздел файла конфигурации, в котором показано, как определить значения этих параметров и каковы рекомендуемые значения для большего числа членств в группах:

```json
{
    ...
    "spec": {
        "resources": {
            ...
            "gateway": {
                "spec": {
                    "replicas": 1,
                    "endpoints": [{...}],
                    "settings": {
                        "gateway-site.gateway.httpserver.requestHeaderBuffer": "65536"
                    }
                }
            },
            ...
        },
        "services": {
            ...
            "hdfs": {
                "resources": [...],
                "settings": {
                  "core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels": "4"
                }
            },
            ...
        }
    }
}
```

  >[!IMPORTANT]
  >Перед началом развертывания создайте в AD группы, указанные для параметров ниже. Если область для любой из этих групп AD является локальной доменной, развертывание завершается сбоем.

- `security.activeDirectory.appOwners` **Необязательный параметр**: список групп AD, имеющих разрешения на создание, удаление и запуск любого приложения. Список может включать группы AD, областью действия которых является универсальная или глобальная группа. Они не могут быть локальными доменными группами.

- `security.activeDirectory.appReaders` **Необязательный параметр**: список групп AD, имеющих разрешения на запуск любого приложения. Список может включать группы AD, областью действия которых является универсальная или глобальная группа. Они не могут быть локальными доменными группами.

Приведенная ниже таблица показывает модель авторизации для управления приложениями.

|   Авторизованные роли   |   Команда [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]   |
|----------------------|--------------------|
|   appOwner           | azdata app create  |
|   appOwner           | azdata app update  |
|   appOwner, appReader| azdata app list    |
|   appOwner, appReader| azdata app describe|
|   appOwner           | azdata app delete  |
|   appOwner, appReader| azdata app run     |

- `security.activeDirectory.subdomain`: **необязательный параметр**. Этот параметр появился в накопительном пакете обновления 5 (CU5) для SQL Server 2019 для поддержки развертывания нескольких кластеров больших данных в одном домене. С помощью этого параметра можно указать разные DNS-имена для каждого развернутого кластера больших данных. Если значение этого параметра не указано в разделе Active Directory файла `control.json`, по умолчанию для расчета значения параметра поддомена будет использоваться имя кластера больших данных (то же, что и имя пространства имен Kubernetes). 

  >[!NOTE]
  >Значение, передаваемое через параметр поддомена, является не новым доменом AD, а лишь DNS-доменом, который используется кластером больших данных для внутренних целей.

  >[!IMPORTANT]
  >Чтобы использовать эти новые возможности и развернуть несколько кластеров больших данных в одном домене, необходимо установить последнюю версию или обновить **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** , чтобы она соответствовала накопительному пакету обновления 5 (CU5) для SQL Server 2019.

  Дополнительные сведения о развертывании нескольких кластеров больших данных в одном домене Active Directory см. в разделе [Концепция: развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в режиме Active Directory](active-directory-deployment-background.md).

- `security.activeDirectory.accountPrefix`: **необязательный параметр**. Этот параметр появился в накопительном пакете обновления 5 (CU5) для SQL Server 2019 для поддержки развертывания нескольких кластеров больших данных в одном домене. Этот параметр гарантирует уникальность имен учетных записей для различных служб кластеров больших данных, которые должны различаться между двумя кластерами. Настройка имени префикса учетной записи является необязательной; по умолчанию в качестве префикса учетной записи используется имя поддомена. Если имя поддомена длиннее 12 символов, в качестве префикса учетной записи используются первые 12 символов имени поддомена.  

  >[!NOTE]
  >Длина имени учетной записи Active Directory должна составлять не более 20 символов. Кластеру больших данных необходимо использовать 8 символов для различения объектов pod и наборов с отслеживанием состояния. В итоге остается 12 символов для префикса учетной записи.

[Проверьте область группы AD](/powershell/module/addsadministration/get-adgroup), чтобы определить, является ли она DomainLocal.

Если вы еще не выполнили инициализацию файла конфигурации развертывания, можно выполнить эту команду, чтобы получить копию конфигурации. В приведенных ниже примерах используется профиль `kubeadm-prod`, то же касается и `openshift-prod`.

```bash
azdata bdc config init --source kubeadm-prod  --target custom-prod-kubeadm
```

Чтобы задать указанные выше параметры в файле `control.json`, используйте следующие команды [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Эти команды заменяют конфигурацию и предоставляют ваши значения до развертывания.

> [!IMPORTANT]
> В выпуске SQL Server 2019 CU2 немного изменилась структура раздела конфигурации безопасности в профиле развертывания, и все связанные с Active Directory параметры находятся в новом `activeDirectory` в дереве JSON в разделе `security` в файле `control.json`.

>[!NOTE]
> Помимо предоставления различных значений для поддомена, как описано в этом разделе, необходимо также использовать разные номера портов для конечных точек кластера больших данных при развертывании нескольких кластеров больших данных в одном кластере Kubernetes. Эти номера портов можно настроить во время развертывания с помощью профилей [конфигурации развертывания](deployment-custom-configuration.md).

Приведенный ниже пример основан на использовании SQL Server 2019 CU2. В нем показана замена значений параметров, связанных с AD, в конфигурации развертывания. В сведениях о домене ниже приведены примеры значений.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Кроме того (только после выхода накопительного пакета обновления 5 (CU5) для SQL Server 2019), вы можете переопределить значения по умолчанию для параметров `subdomain` и `accountPrefix`.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.subdomain=[\"bdctest\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.accountPrefix=[\"bdctest\"]"
```

Аналогичным образом, в выпусках, предшествующих SQL Server 2019 CU2, можно запустить:

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

Помимо указанных выше сведений необходимо также указать DNS-имена для разных конечных точек кластера. Записи DNS, использующие указанные DNS-имена, будут автоматически созданы на DNS-сервере при развертывании. Эти имена будут использоваться при подключении к разным конечным точкам кластера. Например, если DNS-имя для главного экземпляра SQL имеет значение `mastersql`, а поддомен будет использовать значение по умолчанию имени кластера в `control.json`, вы будете использовать `mastersql.contoso.local,31433` или `mastersql.mssql-cluster.contoso.local,31433` (в зависимости от значений, указанных в файлах конфигурации развертывания для DNS-имен конечных точек) для подключения к главному экземпляру из этих средств. 

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[1].dnsName=<monitoring services DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=<SQL Master Primary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=<SQL Master Secondary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=<Gateway (Knox) DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=<app proxy DNS name>.<Domain name. e.g. contoso.local>"
```

> [!IMPORTANT]
> Можно использовать собственные DNS-имена конечных точек, если они полностью определены и не конфликтуют между любыми двумя кластерами больших данных, развернутыми в одном домене. При необходимости можно использовать значение параметра `subdomain`, чтобы гарантировать, что DNS-имена различаются в разных кластерах. Пример:

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.<subdomain e.g. mssql-cluster>.contoso.local"
```

Здесь вы найдете пример скрипта для [развертывания кластера больших данных SQL Server в кластере Kubernetes с одним узлом (kubeadm) и интеграцией с AD](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm-ad).

> [!Note]
> В некоторых ситуациях вы не можете использовать новый параметр `subdomain`. Например, необходимо развернуть выпуск до пакета обновлений CU5, и вы уже обновили **[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]** . Это маловероятно, но, если необходимо вернуться к поведению до выхода накопительного пакета обновления 5 (CU5), можно задать для параметра `useSubdomain` значение `false` в разделе `control.json` Active Directory.  Ниже приведена соответствующая команда.

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false"
```

На данный момент у вас должны быть заданы все необходимые параметры для развертывания BDC с интеграцией с Active Directory.

Теперь можно развернуть кластер BDC, интегрированный с Active Directory, с помощью команды [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] и профиля развертывания kubeadm-prod. Полную документацию по развертыванию [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] см. в статье [Развертывание кластеров больших данных SQL Server в Kubernetes](deployment-guidance.md).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Проверка обратной DNS-записи для контроллера домена

Убедитесь в наличии обратной DNS-записи (записи PTR) для самого доменного контроллера, зарегистрированного на сервере DNS. Это можно проверить, выполнив команду `nslookup` для доменного имени на контроллере домена, чтобы убедиться, что она разрешается в IP-адрес контроллера домена.

## <a name="known-issues-and-limitations"></a>Известные проблемы и ограничения

**Ограничения, которые следует учитывать в накопительном пакете обновления 5 (CU5) для SQL Server 2019**

- В настоящее время информационная панель поиска журналов и информационная панель метрик не поддерживают проверку подлинности AD. Для проверки подлинности на этих информационных панелях можно использовать базовые имя пользователя и пароль, заданные при развертывании. Все остальные конечные точки кластера поддерживают проверку подлинности AD.

- Сейчас безопасный режим AD будет работать только в средах развертывания `kubeadm` и `openshift`, но не в AKS или ARO. Профили развертывания `kubeadm-prod` и `openshift-prod` по умолчанию включают разделы, посвященные безопасности.

- До выпуска накопительного пакета обновления 5 (CU5) для SQL Server 2019, разрешен всего один кластер больших данных на домен (Active Directory). Поддержка нескольких кластеров больших данных на домен доступна, начиная с выпуска накопительного пакета обновления 5 (CU5).

- Ни одна из групп AD, указанных в конфигурациях безопасности, не может быть в области DomainLocal. Вы можете проверить область группы AD, выполнив [эти инструкции](/powershell/module/addsadministration/get-adgroup).

- Учетная запись AD, которая может использоваться для входа в кластер больших данных, разрешена из того же домена, который был настроен для кластера больших данных. Поддержка входов из другого доверенного домена не планируется.

## <a name="next-steps"></a>Дальнейшие действия

[Подключение [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: режим Active Directory](active-directory-connect.md)

[Устранение неполадок при интеграции кластера больших данных SQL Server с Active Directory](troubleshoot-active-directory.md)

[Концепция: развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в режиме Active Directory](active-directory-deployment-background.md)
