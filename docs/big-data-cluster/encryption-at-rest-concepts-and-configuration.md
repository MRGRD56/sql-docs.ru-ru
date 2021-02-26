---
title: Шифрование при хранении
titleSuffix: SQL Server Big Data Clusters
description: Узнайте все о шифровании неактивных данных в кластере больших данных SQL Server 2019.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69749c0da2a5f7ef672a4673b5bc857898e9964f
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489188"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>Основные понятия шифрования неактивных данных и рекомендации по настройке

Начиная с кластеров больших данных CU8 SQL Server доступен набор функций для комплексного шифрования неактивных данных с помощью которого можно обеспечить шифрование на уровне приложения для всех данных, хранящихся на платформе. В этом руководстве описаны основные понятия, архитектура и конфигурация для набора функций для шифрования неактивных данных кластеров больших данных SQL Server.

Кластеры больших данных SQL Server хранят данные в следующих двух расположениях:

* главный экземпляр __SQL Server__;
* __HDFS__, используемый пулом носителей и Spark.

Существует два возможных подхода для прозрачного шифрования данных в кластерах больших данных SQL Server:

* __Шифрование тома__. Такой подход поддерживается платформой Kubernetes и рассматривается как лучшая методика развертывания кластеров больших данных. В этом руководстве шифрование томов не рассматривается. Инструкции по правильному шифрованию томов, которые будут использоваться для кластеров больших данных SQL Server, см. в документации по платформе или устройству Kubernetes.
* __Шифрование на уровне приложения__. Эта архитектура относится к шифрованию данных приложением, обрабатывающим данные перед их записью на диск. Если тома незащищенные, злоумышленник не сможет восстановить артефакты данных в других местах, пока для целевой системы не будут настроены одинаковые ключи шифрования.

__Набор функций шифрования неактивных данных кластеров больших данных SQL Server__ поддерживает основной сценарий __шифрования на уровне приложения__ для компонентов __SQL Server__ и __HDFS__.

Предоставляются следующие возможности:

* __Управляемое системой шифрование неактивных данных__. Это возможно только в CU8.
* __Управляемое пользователем шифрование неактивных данных (BYOK)__ с интеграцией как с управляемыми службами, так и поставщиками внешних ключей. В настоящее время поддерживаются только созданные пользователем ключи, управляемые службой.

## <a name="key-definitions"></a>Основные определения

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>Служба управления ключами кластеров больших данных SQL Server (KMS)

Размещенная контроллером служба, ответственная за управление ключами и сертификатами для функции шифрования неактивных данных для кластера BDC SQL Server. Это служба, которая поддерживает следующие функции:

* Безопасное управление и хранение ключей и сертификатов, используемых для шифрования неактивных данных.
* Совместимость для службы управления ключами с Hadoop. Она предназначена службе управления ключами для компонента HDFS в BDC.
* Управление сертификатами прозрачного шифрования данных SQL Server.

Мы будем ссылаться на эту службу в качестве __службы управления ключами BDC__ в оставшейся части этого документа. Кроме того, термин __BDC__ используется для ссылки на вычислительную платформу __кластера больших данных SQL Server__.

### <a name="system-managed-keys-and-certificates"></a>Ключи и сертификаты, управляемые системой

Служба управления ключами BDC будет управлять всеми ключами и сертификатами для SQL Server и HDFS.

### <a name="user-provided-certificates"></a>Сертификаты, предоставленные пользователем

Предоставленные пользователем ключи и сертификаты, которыми будет управлять служба управления ключами BDC, также называемая "создание собственных ключей" (BYOK).

### <a name="external-providers"></a>Внешние поставщики

Внешние решения для ключей, совместимые со службой управления ключами BDC, для внешнего делегирования. Эта возможность в настоящее время не поддерживается.

## <a name="encryption-at-rest-on-sql-server-big-data-clusters"></a>Шифрование неактивных данных в кластерах больших данных SQL Server

Внимательно прочтите этот документ, чтобы полностью оценить ваш сценарий.

В наборе функций появляется __служба контроллера управления ключами BDC__, с помощью которой можно предоставлять управляемые системой ключи и сертификаты для шифрования данных в SQL Server и HDFS. Этими ключами и сертификатами управляет служба, поэтому в этой документации содержатся операционные инструкции по взаимодействию со службой.

* Экземпляры __SQL Server__ используют установленную функцию [прозрачного шифрования данных (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).
* __HDFS__ использует собственные службы управления Hadoop в каждом модуле, чтобы взаимодействовать со службой управления ключами управления BDC на контроллере. Это включает зоны шифрования HDFS, которые обеспечивают безопасные пути в HDFS.

### <a name="sql-server-instances"></a>Экземпляры SQL Server

* На pod SQL Server будет установлен созданный системой сертификат, который будет использоваться с командами TDE. Стандартом именования сертификатов, управляемым системой, является `TDECertificate` + `timestamp`. Например, `TDECertificate2020_09_15_22_46_27`.
* Подготовленные базы данных и пользовательские базы данных главного экземпляра службы BDC не шифруются автоматически. Администраторы могут использовать установленный сертификат для шифрования любой базы данных.
* Пул вычислений и пул носителей будут автоматически шифроваться с помощью созданного системой сертификата.
* Шифрование пула данных, хотя и технически возможно при использовании команд T-SQL `EXECUTE AT`, в настоящее время не рекомендуется и не поддерживается. Использование этого метода для шифрования баз данных пула данных может быть неэффективным, а шифрование может не произойти в нужном состоянии. В результате его выполнения также создается несовместимый путь обновления к следующим выпускам.
* Смена ключей SQL Server осуществляется с помощью стандартных команд администрирования T-SQL. Полные инструкции см. в [руководстве по использованию шифрования неактивных данных (TDE) в кластерах больших данных SQL Server](encryption-at-rest-sql-server-tde.md).
* Мониторинг шифрования происходит через существующие стандартное динамическое административное представление SQL Server для TDE.
* Поддерживается резервное копирование и восстановление базы данных с поддержкой TDE в кластере.
* Поддерживается высокий уровень доступности. Если база данных на основном экземпляре SQL Server шифруется, то все вторичные реплики базы данных также будут зашифрованы.

### <a name="hdfs-encryption-zones"></a>Зоны шифрования HDFS

* [Интеграция Active Directory](active-directory-prerequisites.md) требуется для включения функции зон шифрования для HDFS.
* Созданный системой ключ будет подготовлен в системе управления ключей Hadoop. Имя ключа — `securelakekey`. В CU8 ключ по умолчанию — 256 бит, и мы поддерживаем 256-разрядное шифрование AES.
* Зона шифрования по умолчанию будет подготовлена с помощью указанного выше ключа, созданного системой, в пути с именем `/securelake`.
* Пользователи могут создавать дополнительные ключи и зоны шифрования, используя конкретные инструкции, приведенные в этом разделе. Пользователи смогут выбрать размер ключа 128, 192 или 256 во время создания ключа.
* Смена ключей зон шифрования HDFS осуществляется с помощью azdata. Полные инструкции см. в статье [Рекомендации по использованию зон шифрования HDFS в Кластерах больших данных SQL Server](encryption-at-rest-hdfs-encryption-zones.md).
* Невозможно выполнить подключение по уровням HDFS вверху зоны шифрования.

## <a name="encryption-at-rest-administration"></a>Администрирование шифрования неактивных данных

Ниже перечислены возможности администрирования шифрования неактивных данных.

* Управление функцией [TDE в SQL Server](encryption-at-rest-sql-server-tde.md) осуществляется с помощью стандартных команд T-SQL.
* Управление [зонами шифрования HDFS](encryption-at-rest-hdfs-encryption-zones.md) и ключами HDFS осуществляется с помощью команд azdata.
* Следующие функции администрирования выполняются с помощью [операционных записных книжек](cluster-manage-notebooks.md):
    - резервное копирование и восстановление ключей HDFS;
    - удаление ключей HDFS.

## <a name="configuration-guide"></a>Руководство по настройке

Во время __новых развертываний кластеров больших данных SQL Server__, CU8 и последующих, __шифрование неактивных данных будет включено и настроено по умолчанию__. Это означает:

* Компонент службы управления ключами BDC будет развернут в контроллере и создаст набор ключей и сертификатов по умолчанию.
* SQL Server будет развернут с включенным TDE, а сертификаты будут установлены контроллером.
* Служба управления ключами Hadoop (для HDFS) будет настроена для взаимодействия с сервером BDC для операций шифрования. Зоны шифрования HDFS настроены и готовы к использованию.

Применяются требования и поведение по умолчанию, описанные в предыдущем разделе.

__При новом развертывании кластера больших данных SQL Server с накопительным пакетом обновления 8 или более поздней версии либо обновлении напрямую до накопительного пакета обновления 9 никаких дополнительных действий не требуется__.

### <a name="upgrade-scenarios"></a>Варианты обновления

При обновлении существующих кластеров новое или повторное шифрование не применяется к пользовательским данным, которые ранее не были зашифрованы. Такое поведение характерно и для каждого компонента необходимо учитывать следующие особенности:

* __SQL Server__

    1. __Главный экземпляр SQL Server__. Процесс обновления не повлияет на базы данных главного экземпляра и установленные сертификаты TDE, однако мы настоятельно рекомендуем создать резервные копии баз данных и вручную установленных сертификатов TDE перед началом обновления. Также рекомендуется хранить эти артефакты вне кластера BDC SQL Server.
    1. __Пул вычисления и хранения__. Эти базы данных являются управляемыми системой, временными и будут созданы повторно и автоматически зашифрованы при обновлении кластера.
    1. __Пул данных__. Обновление не влияет на базы данных в экземплярах SQL Server, входящих в пул данных.

* __HDFS__

    1. __HDFS__. В процессе обновления не будут затронуты файлы и папки HDFS, расположенные вне зон шифрования.

### <a name="upgrading-to-cu9-from-cu8-or-earlier"></a>Обновление с версии накопительного пакета обновления 8 или более ранней до версии 9

Дополнительные действия не требуются.

### <a name="upgrading-to-cu8-from-cu6-or-earlier"></a>Обновление с версии накопительного пакета обновления 6 или более ранней до версии 8

   > [!CAUTION]
   > Перед обновлением до кластера больших данных SQL Server CU8 выполните полное резервное копирование ваших данных.


__Зоны шифрования не будут настроены__. Компонент службы управления ключами Hadoop не будет настроен для использования службы управления ключами BDC. Чтобы настроить и включить функцию зон шифрования HDFS после обновления, следуйте инструкциям в следующем разделе.

#### <a name="enable-hdfs-encryption-zones-after-upgrade-to-cu8"></a>Включение зон шифрования HDFS после обновления до накопительного пакета обновления 8

Если вы обновили кластер до накопительного пакета обновления 8 (`azdata upgrade`) и хотите включить зоны шифрования HDFS, у вас есть два варианта:

* выполнение [операционной записной книжки](cluster-manage-notebooks.md) Azure Data Studio с именем __SOP0128 — включение зон шифрования HDFS в кластерах больших данных__ для настройки;
* выполнение скрипта, как описано ниже.

Требования

- встроенный кластер [Active Directory](active-directory-prerequisites.md);

- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] настроенный и зарегистрированный в кластере в режиме AD.

Выполните следующую процедуру, чтобы перенастроить кластер с поддержкой зон шифрования.

1. После выполнения обновления, используя `azdata`, сохраните следующие скрипты.

    Требования к выполнению сценариев:
        
    * Оба скрипта должны находиться в одном каталоге. 
    * `kubectl` на `PATH
    * Файл```config``` для Kubernetes в папке ```$HOME/.kube```
    
    Этот сценарий должен называться __```run-key-provider-patch.sh```__ :

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    Этот сценарий должен называться __```updatekeyprovider.py```__ :

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    Выполните __```run-key-provider-patch.sh```__ , используя соответствующие параметры. 

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об эффективном использовании шифрования неактивных данных кластеров больших данных SQL Server см. в следующих статьях:

- [Шифрование неактивных данных SQL Server TDE](encryption-at-rest-sql-server-tde.md)
- [Шифрование неактивных данных в зонах шифрования HDFS](encryption-at-rest-hdfs-encryption-zones.md)

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в следующем обзоре:

- [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
