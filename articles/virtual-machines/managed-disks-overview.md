---
title: Обзор Хранилище дисков Azure
description: Обзор управляемых дисков Azure, которые обрабатывают учетные записи хранения при использовании виртуальных машин.
author: roygara
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: contperf-fy21q1
ms.openlocfilehash: eea5c800d7aa9c8d1e6c0c507136b86ab8bf21f3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104604038"
---
# <a name="introduction-to-azure-managed-disks"></a>Общие сведения об управляемых дисках Azure

Управляемые диски Azure — это тома хранилища на уровне блоков, управляемые Azure и используемые с виртуальными машинами Azure. Управляемые диски подобны физическому диску на локальном сервере, но при этом они виртуализированные. Все, что вам необходимо сделать, — это указать размер и тип управляемых дисков, а также выполнить их подготовку. После подготовки Azure выполнит остальную часть работы.

Доступные типы дисков: диски ценовой категории "Ультра", твердотельные накопители (SSD) ценовой категории "Премиум", диски SSD и жесткие диски (HDD) ценовой категории "Стандартный". Дополнительные сведения о типе каждого отдельного диска см. в статье, посвященной [выбору типа диска для виртуальных машин IaaS](disks-types.md).

## <a name="benefits-of-managed-disks"></a>Преимущества управляемых дисков

Рассмотрим некоторые преимущества использования управляемых дисков.

### <a name="highly-durable-and-available"></a>Высокая устойчивость и доступность

Управляемые диски обеспечивают доступность на уровне 99,999 %. Для этого предоставляются три реплики данных, обеспечивающие высокую надежность. При сбое одной реплики можно задействовать остальные, что гарантирует длительное хранение данных и высокую устойчивость к сбоям. Эта архитектура позволила Azure гарантировать надежность дисков IaaS корпоративного уровня с ведущим в отрасли нулевым показателем процента брака по итогам продаж в течение одного года.

### <a name="simple-and-scalable-vm-deployment"></a>Простое и масштабируемое развертывание виртуальной машины

При использовании управляемых дисков можно создать до 50 000 **дисков** виртуальных машин определенного типа в подписке на регион, что дает возможность создавать тысячи **виртуальных машин** в пределах одной подписки. Этот компонент также повышает масштабируемость [масштабируемых наборов виртуальных машин](../virtual-machine-scale-sets/overview.md), позволяя создавать до тысячи виртуальных машин в масштабируемом наборе с помощью образа Marketplace.

### <a name="integration-with-availability-sets"></a>Интеграция с группами доступности

Управляемые диски интегрируются с группами доступности. Это гарантирует, что диски [виртуальных машин в группе доступности](./availability-set-overview.md) достаточно изолированы друг от друга, что позволяет избежать единых точек сбоя. Диски автоматически размещаются в разных единицах масштабирования хранилища (метках). В случае сбоя стека из-за проблем с оборудованием или программным обеспечением выйдет из строя только тот экземпляр виртуальной машины, диски которой расположены в этом стеке. Предположим, есть приложение, запущенное на пяти виртуальных машинах, расположенных в одной группе доступности. Диски этих виртуальных машин не хранятся в одном стеке, поэтому если один стек выйдет из строя, другие экземпляры приложения продолжат работу.

### <a name="integration-with-availability-zones"></a>Интеграция с Зонами доступности

Управляемые диски с поддержкой [зон доступности](../availability-zones/az-overview.md) — предложение, обеспечивающее высокий уровень доступности и защищающее приложения от сбоев центров обработки данных. Зоны доступности — уникальные физические расположения в пределах одного региона Azure. Каждая зона состоит из одного или нескольких центров обработки данных, оснащенных независимыми системами электроснабжения, охлаждения и сетевого взаимодействия. Чтобы обеспечить устойчивость, во всех включенных областях используются минимум три отдельные зоны. Благодаря зонам доступности Azure предлагает наилучшее в отрасли соглашение об уровне обслуживания с гарантией времени непрерывной работы 99,99 % для виртуальных машин.

### <a name="azure-backup-support"></a>Поддержка резервного копирования Azure

Чтобы обеспечить защиту от региональных сбоев, можно использовать [Azure Backup](../backup/backup-overview.md), чтобы создать задание резервного копирования, выполняющее резервное копирование по расписанию и с учетом политик хранения резервных копий. Это позволит легко восстанавливать виртуальные машины или управляемые диски при необходимости. Служба Azure Backup сейчас поддерживает диски размером до 32 ТиБ. [Подробнее](../backup/backup-support-matrix-iaas.md) о поддержке резервного копирования виртуальной машины Azure.

#### <a name="azure-disk-backup"></a>Резервное копирование дисков Azure

Azure Backup предлагает резервное копирование дисков Azure (Предварительная версия) в виде собственного облачного решения для резервного копирования, которое защищает данные на управляемых дисках. Это простое, безопасное и экономичное решение, которое позволяет настроить защиту управляемых дисков за несколько шагов. Служба архивации дисков Azure предлагает готовое решение, которое обеспечивает управление жизненным циклом моментальных снимков для управляемых дисков путем автоматизации периодического создания моментальных снимков и хранения их в течение настроенного времени с помощью политики архивации. Дополнительные сведения о резервном копировании дисков Azure см. [в статье Обзор резервного копирования дисков Azure (Предварительная версия)](../backup/disk-backup-overview.md).

### <a name="granular-access-control"></a>Детальный контроль доступа

Вы можете использовать [управление доступом на основе ролей в Azure (Azure RBAC)](../role-based-access-control/overview.md), чтобы назначить определенные разрешения для управляемых дисков одному или нескольким пользователям. Управляемые диски позволяют выполнять различные операции, в том числе чтение, запись (создание или обновление), удаление и извлечение [универсального кода ресурса (URI) подписанного URL-адреса (SAS) ](../storage/common/storage-sas-overview.md) для диска. Вы можете предоставить пользователям доступ только к тем операциям, которые необходимы им для выполнения определенного задания. Если вы не хотите, чтобы пользователь имел возможность копировать управляемые диски в учетную запись хранения, можно не предоставлять ему доступ к действию экспорта для этого управляемого диска. Если вы не хотите, чтобы пользователь использовал URI SAS для копирования управляемого диска, можно не предоставлять ему такое разрешение для этого диска.

### <a name="upload-your-vhd"></a>Отправка виртуального жесткого диска

Прямая отправка упрощает передачу виртуального жесткого диска на управляемый диск Azure. Ранее необходимо было выполнить более сложный процесс, который включал в себя промежуточное хранение данных в учетной записи хранения. Теперь шагов меньше. Стало проще отправлять локальные виртуальные машины в Azure, отправлять их на большие управляемые диски, а также упрощен процесс резервного копирования и восстановления. Это также сокращает затраты, позволяя отправлять данные на управляемые диски напрямую, не присоединяя их к виртуальным машинам. Вы можете использовать прямую отправку для отправки виртуальных жестких дисков размером до 32 Тиб.

Сведения о том, как передавать виртуальный жесткий диск в Azure, см. в статьях [об отправке с помощью CLI](linux/disks-upload-vhd-to-managed-disk-cli.md) или [PowerShell](windows/disks-upload-vhd-to-managed-disk-powershell.md).

## <a name="security"></a>Безопасность

### <a name="private-links"></a>Приватные каналы

Поддержка закрытых ссылок для управляемых дисков может использоваться для импорта или экспорта управляемого диска внутри сети. Приватные каналы позволяют создавать URI SAS с ограниченным сроком действия для неподключенных управляемых дисков и моментальных снимков, чтобы вы могли экспортировать данные в другой регион для расширения охвата, аварийного восстановления или криминалистического анализа. Вы также можете использовать URI SAS для непосредственной отправки виртуального жесткого диска из локальной среды на пустой диск. Теперь вы можете использовать [Приватные каналы](../private-link/private-link-overview.md), чтобы разрешить экспорт и импорт управляемых дисков только в пределах вашей виртуальной сети Azure. Приватные каналы гарантируют, что данные будут передаваться только по защищенной магистральной сети Майкрософт.

Сведения о том, как включить Приватные каналы для импорта и экспорта управляемых дисков, см. в соответствующих статьях для [CLI](linux/disks-export-import-private-links-cli.md) или [портала](disks-enable-private-links-for-import-export-portal.md).

### <a name="encryption"></a>Шифрование

Управляемые диски поддерживают два разных типа шифрования. Первый из них — это шифрование на стороне сервера (SSE), которое выполняет служба хранилища, второй — шифрование дисков Azure, которые можно включить на дисках операционной системы и данных для виртуальных машин.

#### <a name="server-side-encryption"></a>Шифрование на стороне сервера

Шифрование на стороне сервера позволяет шифровать неактивные данные и защищает данные в соответствии с корпоративными обязательствами по обеспечению безопасности и соответствия требованиям. Шифрование на стороне сервера включено по умолчанию для всех управляемых дисков, моментальных снимков и образов во всех регионах, где доступны управляемые диски. При этом временные диски не шифруются с помощью шифрования на стороне сервера, если только вы не включили шифрование на узле (см. сведения о [ролях дисков и временных дисках](#temporary-disk)).

Вы можете разрешить службе Azure управлять ключами (ключи под управлением платформы) или делать это самостоятельно (ключи под управлением клиента). Дополнительные сведения см. в статье [Шифрование на стороне сервера для Хранилища дисков Azure](./disk-encryption.md).


#### <a name="azure-disk-encryption"></a>Шифрование дисков Azure

Шифрование дисков Azure позволяет шифровать диски ОС и диски данных, используемые виртуальными машинами IaaS. Шифрование включает в себя управляемые диски. Для Windows диски шифруются с помощью стандартной отраслевой технологии шифрования BitLocker. Для Linux диски шифруются с помощью технологии DM-Crypt. Процесс шифрования интегрируется с Azure Key Vault, что позволяет управлять ключами шифрования дисков. См. сведения в статьях [Шифрование дисков Azure для виртуальных машин Linux](linux/disk-encryption-overview.md) и [Шифрование дисков Azure для виртуальных машин Windows](windows/disk-encryption-overview.md).

## <a name="disk-roles"></a>Роли дисков

В Azure есть три основные роли дисков: диск данных, диск операционной системы и временный диск. Эти роли назначаются дискам, подключенным к виртуальной машине.

![Роли дисков](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Диск данных

Диск данных — управляемый диск, подключенный к виртуальной машине для хранения данных приложений или других необходимых данных. Диски данных регистрируются как диски SCSI и обозначаются любой указанной буквой. Максимальная емкость каждого диска составляет 32 767 ГиБ. Размер виртуальной машины определяет, сколько дисков данных можно подключить и какой тип хранилища можно использовать для размещения дисков.

### <a name="os-disk"></a>Диск ОС

У каждой виртуальной машины есть один подключенный диск операционной системы. На этот диск предварительно установлена операционная система которая была выбран при создании виртуальной машины. Этот диск содержит загрузочный том.

Максимальная емкость этого диска — 4 095 гиб.

### <a name="temporary-disk"></a>Временный диск

Большинство виртуальных машин содержат временный диск, который не является управляемым диском. Временный диск обеспечивает краткосрочное хранение приложений и процессов. оно предназначено только для хранения данных, таких как страницы или файлы подкачки. Данные на временном диске могут быть потеряны во время [обслуживания](./understand-vm-reboots.md) или при [повторном развертывании виртуальной машины](/troubleshoot/azure/virtual-machines/redeploy-to-new-node-windows?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). При успешной стандартной перезагрузке виртуальной машины данные на временном диске будут сохранены. Дополнительные сведения о виртуальных машинах без временных дисков см. [в статье размеры виртуальных машин Azure без локального временного диска](azure-vms-no-temp-disk.md).

В виртуальных машинах Azure Linux временный обычно диск отмечен как /dev/sdb, а в виртуальных машинах Windows — как D:. Временный диск не шифруется с помощью шифрования на стороне сервера, если только вы не включили шифрование на узле.

## <a name="managed-disk-snapshots"></a>Моментальные снимки управляемых дисков

Моментальный снимок управляемого диска — это полная отказоустойчивая копия управляемого диска, доступная только для чтения, которая по умолчанию хранится в качестве управляемого диска ценовой категории "Стандартный". С помощью моментальных снимков можно архивировать управляемые диски на любой момент времени. Эти моментальные снимки могут существовать независимо от исходного диска и использоваться для создания других управляемых дисков. 

Плата за моментальные снимки взимается в зависимости от используемого размера. Например, если создается моментальный снимок управляемого диска с подготовленной емкостью 64 Гиб и фактическим объемом используемых данных 10 Гиб, оплачиваются только используемые данные, то есть 10 Гиб. Просмотреть использованный размер моментальных снимков можно в [отчете об использовании Azure](../cost-management-billing/understand/review-individual-bill.md). Например, если размер данных моментального снимка составляет 10 ГиБ, использованное количество в **ежедневном** отчете об использовании будет отражено так: 10 ГиБ/(31 день) = 0,3226.

Дополнительные сведения о создании моментальных снимков для управляемых дисков см. в следующих статьях.

- [Создание моментального снимка управляемого диска в Windows](windows/snapshot-copy-managed-disk.md)
- [Создание моментального снимка управляемого диска в Linux](linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Изображения

Управляемые диски также поддерживают создание управляемых пользовательских образов. Вы можете создать образ из пользовательского VHD-файла в учетной записи хранения или непосредственно из универсальной (подготовленной с помощью программы sysprep) виртуальной машины. При этом создается один образ. Он содержит все управляемые диски, связанные с виртуальной машиной, включая диски операционной системы и диски данных. Таким образом, вы можете создавать сотни виртуальных машин с помощью пользовательского образа без необходимости копировать все учетные записи хранения или управлять ими.

Дополнительные сведения о создании образов см. в следующих статьях:

- [How to capture a managed image of a generalized VM in Azure](windows/capture-image-resource.md) (Как создать управляемый образ обобщенной виртуальной машины в Azure)
- [Как генерализовать и создать образ виртуальной машины Linux при помощи Azure CLI](linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Образы и моментальные снимки

Важно понять разницу между образами и моментальными дисками. При использовании управляемых дисков можно создать образ универсальной виртуальной машины, распределение которой было отменено. Этот образ будет содержать все диски, подключенные к этой виртуальной машине. Его можно использовать для создания новой виртуальной машины, которая также будет содержать все эти диски.

Моментальный снимок — это копия диска на момент создания снимка. Она относится только к одному диску. Если у виртуальной машины только один диск (диск операционной системы), можно создать моментальный снимок или образ, а затем на основе одного из них создать виртуальную машину.

Моментальный снимок содержит сведения только о хранящемся в нем диске. Поэтому моментальные снимки сложно использовать в сценариях, требующих управления несколькими дисками, таких как чередование. Моментальные снимки пришлось бы согласовывать между собой, и эта возможность пока не поддерживается.

## <a name="disk-allocation-and-performance"></a>Распределение диска и его производительность

На следующей схеме показано распределение пропускной способности и операций ввода-вывода для дисков в режиме реального времени с помощью трехуровневой системы подготовки:

![Трехуровневая система подготовки, показывающая распределение пропускной способности и операций ввода-вывода](media/virtual-machines-managed-disks-overview/real-time-disk-allocation.png)

На первом уровне подготовки задаются операции ввода-вывода на диск и пропускная способность.  На втором уровне узел сервера вычислений реализует подготовку SSD, применяя его только к данным, хранящимся на SSD сервера, в том числе к дискам с кэшированием (ReadWrite и ReadOnly), а также к локальным и временным дискам. Наконец, подготовка сети виртуальной машины выполняется на третьем уровне для любых операций ввода-вывода, которые узел вычислений отправляет в серверную часть службы хранилища Azure. При такой схеме производительность виртуальной машины зависит от различных факторов, от того, как виртуальная машина использует локальный SSD, к количеству подключенных дисков, а также к типу производительности и кэширования подключенных дисков.

В качестве примера этих ограничений виртуальная машина Standard_DS1v1 не достигают 5000 операций ввода-вывода в секунду для диска P30, независимо от того, кэширован он или нет, из-за ограничений на уровнях SSD и сети:

![Пример выделения Standard_DS1v1](media/virtual-machines-managed-disks-overview/example-vm-allocation.png)

Azure использует приоритетный сетевой канал для дискового трафика, который получает приоритет над другим низкоприоритетным сетевым трафиком. Это помогает дискам поддерживать ожидаемую производительность в случае сетевых состязаний. Аналогичным образом служба хранилища Azure обрабатывает состязание за ресурсы и другие проблемы в фоновом режиме с автоматической балансировкой нагрузки. Служба хранилища Azure выделяет необходимые ресурсы при создании диска и применяет упреждающее и реактивное распределение ресурсов для управления уровнем трафика. Благодаря этому диски могут поддерживать ожидаемые целевые значения операций ввода-вывода и пропускной способности. При необходимости метрики уровня виртуальной машины и уровня диска можно использовать для наблюдения за производительностью и настройками.

Чтобы получить рекомендации по оптимизации конфигурации виртуальной машины и диска для достижения желаемой производительности, см. статью [О проектировании для повышения производительности](premium-storage-performance.md).

## <a name="next-steps"></a>Дальнейшие действия

Если вы хотите узнать больше об управляемых дисках, ознакомьтесь со следующим документом: [Better Azure VM Resiliency with Managed Disks](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency) (Улучшена устойчивость виртуальной машины Azure к управляемым дискам).

Узнайте больше о типах дисков, предлагаемых в Azure, и выберите тип, который подходит для ваших нужд, и ознакомьтесь с их показателями производительности, почитав нашу статью о типах дисков.

> [!div class="nextstepaction"]
> [Выбор типа диска для виртуальных машин IaaS](disks-types.md)