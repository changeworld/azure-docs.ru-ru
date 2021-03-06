---
title: Определение терминов SAP HANA в Azure (крупные экземпляры) | Документация Майкрософт
description: Определение терминов SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 92ef2e59dab1921eae8e7d88249e75116601c597
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101670865"
---
# <a name="know-the-terms"></a>Определение терминов

В этом руководстве широко используются некоторые общие определения. Ознакомьтесь со следующими терминами и их описанием.

- **IaaS**: инфраструктура как услуга.
- **PaaS**: платформа как услуга.
- **SaaS**: программное обеспечение как услуга.
- **Компонент SAP**: отдельное приложение SAP, например ERP Central Component (ECC), Business Warehouse (BW), Solution Manager или Enterprise Portal (EP). Компоненты SAP могут основываться на традиционной технологии ABAP или Java или быть приложениями не на основе NetWeaver, например бизнес-объектами.
- **Среда SAP**: один или несколько компонентов SAP, логически сгруппированных для выполнения бизнес-функции (разработка, оценка качества, обучение, аварийное восстановление или производство).
- **Ландшафт SAP**: термин обозначает все ресурсы SAP, доступные в ИТ-ландшафте. Ландшафт SAP включает все рабочие и нерабочие среды.
- **Система SAP**: сочетание уровня СУБД и уровня приложений системы разработки SAP ERP, тестовой системы SAP BW и рабочей системы SAP CRM. В развертываниях Azure не поддерживается разделение этих двух уровней между локальной средой и Azure. Система SAP должна быть развернута полностью в локальной среде или полностью в Azure. При этом вы можете развернуть различные системы ландшафта SAP как в Azure, так и локально. Например, системы разработки и тестирования SAP CRM можно развернуть в Azure, а рабочую систему SAP CRM — в локальной среде. Решение SAP HANA в Azure (крупные экземпляры) предназначено для размещения уровня приложений систем SAP на виртуальных машинах и соответствующего экземпляра SAP HANA в единице в стеке крупных экземпляров SAP HANA в Azure.
- **Стек крупных экземпляров**: сертифицированный для программы SAP HANA TDI аппаратный стек, предназначенный для запуска экземпляров SAP HANA в Azure.
- **SAP HANA в Azure (крупные экземпляры)**: официальное название предложения в Azure, предусматривающее запуск экземпляров SAP HANA на оборудовании, сертифицированном для программы SAP HANA TDI, которые развертываются в стеках крупных экземпляров в разных регионах Azure. Связанный термин *крупные экземпляры HANA* — сокращенное название *SAP HANA в Azure (крупные экземпляры)*, которое широко используется в этом техническом руководстве по развертыванию.
- **Распределенное развертывание**: описывает сценарии, при которых в подписке Azure развернуты виртуальные машины с подключениями "сеть — сеть" или Azure ExpressRoute между локальными центрами обработки данных и Azure. В документации Azure развертывания такого рода также называются распределенными развертываниями. Такое подключение используется для расширения в Azure локальных доменов, локальной службы Active Directory, OpenLDAP и локальной службы DNS. Локальная среда распространяется на ресурсы Azure, входящие в подписки Azure. При таком расширении виртуальные машины могут быть частью локального домена. 

   Пользователи локального домена имеют доступ к серверам и могут запускать службы на этих виртуальных машинах (например, службы СУБД). Возможно взаимодействие и разрешение имен между виртуальными машинами, развернутыми в локальной сети и в Azure. Это типичный сценарий, при котором развертывается большинство ресурсов SAP. Дополнительные сведения см. в статьях [VPN-шлюз для Azure](../../../vpn-gateway/vpn-gateway-about-vpngateways.md) и [Создание подключения на основе виртуальной сети типа "сеть — сеть" c помощью портала Azure](../../../vpn-gateway/tutorial-site-to-site-portal.md).
- **Клиент**: пользователь, развернутый в стеке крупных экземпляров HANA, изолируется в *клиент*. Клиент изолируется от других клиентов на уровне сети, хранилища и вычислений. Таким образом, единицы хранения и вычисления, назначенные разным клиентам, не могут видеть друг друга или взаимодействовать друг с другом на уровне стека крупных экземпляров HANA. Пользователь может выполнить развертывание в разные клиенты. Но и в таком случае клиенты не будут взаимодействовать на уровне стека крупных экземпляров HANA.
- **Категория номеров SKU**: для больших экземпляров HANA предлагаются следующие две категории номеров SKU.
    - **Класс тип I**: S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224 и S224m
    - **Класс типа II**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm и S960m.
- **Метка**: определяет внутренний размер развертывания Майкрософт для крупных экземпляров Hana. Прежде чем приступать к развертыванию единиц крупных экземпляров HANA, необходимо развернуть в месторасположении центра обработки данных метку, предназначенную для крупных экземпляров HANA и включающую в себя данные о вычислительных ресурсах, сети и стойках хранилища. Такое развертывание называется меткой крупных экземпляров HANA или, начиная с ревизии 4 (см. ниже), используется альтернативный термин **строка крупных экземпляров (Large Instance Row)**
- **Редакция**: для крупных экземпляров Hana существуют две разные версии меток. Они отличаются по архитектуре и по соответствию узлам виртуальных машин Azure.
    - "Редакция 3" (Rev 3): является исходным проектом, который развертывался начиная с середины 2016 г.
    - "Редакция 4" (Rev 4): это новый проект, который обеспечивает более точное сходство с узлами виртуальных машин Azure и более низкую задержку в сети между виртуальными машинами Azure и единицами крупных экземпляров HANA. 
    - "Редакция 4.2" (Rev 4.2): на существующих контроллерах домена версии 4 ресурсы переносятся в инфраструктуру BareMetal.  Клиенты имеют доступ к своим ресурсам как к экземплярам BareMetal на портале Azure. 

Существует множество дополнительных ресурсов, посвященных развертыванию рабочих нагрузок SAP в облаке. Если вы планируете развернуть SAP HANA в Azure, рекомендуется ознакомиться с особенностями сред Azure IaaS и подготовиться к развертыванию рабочих нагрузок SAP в такой среде. Прежде чем продолжить работу с руководством, ознакомьтесь со статьей [Размещение и выполнение сценариев рабочей нагрузки SAP с помощью Azure](get-started.md). 

**Дальнейшие действия**
- См. раздел [HLI Certification](hana-certification.md) (Сертификация HLI)