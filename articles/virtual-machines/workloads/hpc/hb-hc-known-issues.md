---
title: Устранение известных проблем с помощью виртуальных машин HPC и GPU с виртуальными машинами Azure | Документация Майкрософт
description: Сведения об устранении известных проблем с размерами виртуальных машин HPC и GPU в Azure.
author: vermagit
ms.service: virtual-machines
ms.subservice: hpc
ms.topic: article
ms.date: 03/25/2021
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: d8c3a2d961cc5b6fd719b77dae07b6e46c3d8b65
ms.sourcegitcommit: 73d80a95e28618f5dfd719647ff37a8ab157a668
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105604844"
---
# <a name="known-issues-with-h-series-and-n-series-vms"></a>Известные проблемы с виртуальными машинами серий H и N

В этой статье вы попытаетесь перечислить последние распространенные проблемы и их решения при использовании виртуальных машин HPC и GPU серии [H](../../sizes-hpc.md) и [N](../../sizes-gpu.md) .

## <a name="mofed-installation-on-ubuntu"></a>Установка МОФЕД в Ubuntu
В Ubuntu-18,04, Mellanox ОФЕД показал несовместимость с версиями ядра `5.4.0-1039-azure #42` и более поздних версий, что приводит к увеличению времени загрузки виртуальной машины до 30 минут. Это было сообщено как для Mellanox ОФЕД Versions 5.2-1.0.4.0, так и 5.2-2.2.0.0.
Временное решение — использование **канонического образа: UbuntuServer: 18_04-LTS-Gen2:18.04.202101290** Marketplace или более ранней версии, а не для обновления ядра.
Эта проблема должна быть решена с помощью более новой МОФЕД (подлежит уточнению).

## <a name="mpi-qp-creation-errors"></a>Ошибки создания QP MPI
Если при выполнении любой рабочей нагрузки MPI выдается ошибка создания QP, как показано ниже, мы рекомендуем перезагрузить виртуальную машину и повторить рабочую нагрузку. Эта проблема будет исправлена в будущем.

```bash
ib_mlx5_dv.c:150  UCX  ERROR mlx5dv_devx_obj_create(QP) failed, syndrome 0: Invalid argument
```

Вы можете проверить значения максимального числа пар очередей при обнаружении проблемы следующим образом.
```bash
[user@azurehpc-vm ~]$ ibv_devinfo -vv | grep qp
max_qp: 4096
```

## <a name="accelerated-networking-on-hb-hc-hbv2-and-ndv2"></a>Ускорение работы в сети на хб, HC, HBv2 и NDv2

Служба [ускоренной](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) работы в Azure теперь доступна на виртуальных машинах с поддержкой RDMA и InfiniBand, а также размерах [ХБ](../../hb-series.md), [HC](../../hc-series.md), [HBv2](../../hbv2-series.md)и [NDv2](../../ndv2-series.md). Теперь эта возможность обеспечивает расширенные возможности (до 30 Гбит/с) и задержки в сети Azure Ethernet. Хотя это отдельно от возможностей RDMA в сети InfiniBand, некоторые изменения платформы для этой возможности могут повлиять на поведение определенных реализаций MPI при выполнении заданий через InfiniBand. В частности, интерфейс InfiniBand на некоторых виртуальных машинах может иметь немного другое имя (mlx5_1, в отличие от более ранних mlx5_0), и это может потребовать оптимизации команд MPI, особенно при использовании интерфейса УККС (обычно с Опенмпи и HPC-X). В настоящее время простейшим решением может быть использование новейшего HPC-X в CentOS-HPC образов виртуальных машин или отключение ускоренной сети, если это не требуется.
Дополнительные сведения об этом см. в этой [статье течкоммунити](https://techcommunity.microsoft.com/t5/azure-compute/accelerated-networking-on-hb-hc-and-hbv2/ba-p/2067965) с инструкциями по устранению всех наблюдаемых проблем.

## <a name="infiniband-driver-installation-on-non-sr-iov-vms"></a>Установка драйвера InfiniBand на виртуальных машинах, не использующих SR-IOV

В настоящее время H16r, H16mr и NC24r, не включены в SR-IOV. Дополнительные сведения о стеке InfiniBand разветвления см. [здесь](../../sizes-hpc.md#rdma-capable-instances).
InfiniBand можно настроить на размерах виртуальных машин с поддержкой SR-IOV с помощью драйверов ОФЕД, в то время как размер виртуальных машин не на основе SR-IOV требует наличия драйверов ND. Эта поддержка для [CentOS, RHEL и Ubuntu](configure.md)доступна соответствующим образом.

## <a name="duplicate-mac-with-cloud-init-with-ubuntu-on-h-series-and-n-series-vms"></a>Дублирование компьютеров MAC с помощью Cloud-init с Ubuntu на виртуальных машинах серии H и N

Существует известная ошибка, связанная с Cloud-init на образы виртуальных машин Ubuntu, когда она пытается открыть интерфейс для пересчета. Это может произойти либо при перезагрузке виртуальной машины, либо при попытке создать образ виртуальной машины после обобщения. В журналах загрузки виртуальной машины может отображаться примерно следующее сообщение об ошибке:
```console
“Starting Network Service...RuntimeError: duplicate mac found! both 'eth1' and 'ib0' have mac”.
```

Этот "дубликат MAC с Cloud-init в Ubuntu" является известной проблемой. Это будет разрешено в более новых ядрах. Если эта ошибка возникает, обходной путь:
1) Развертывание образа виртуальной машины (Ubuntu 18,04) Marketplace
2) Установите необходимые пакеты программного обеспечения для включения ГЕО([инструкции](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351))
3) Измените waagent. conf на Change Енаблердма = y.
4) Отключение сетевых подключений в Cloud-init
    ```console
    echo network: {config: disabled} | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
    ```
5) Измените файл конфигурации сети нетплан, созданный Cloud-init, чтобы удалить MAC.
    ```console
    sudo bash -c "cat > /etc/netplan/50-cloud-init.yaml" <<'EOF'
    network:
      ethernets:
        eth0:
          dhcp4: true
      version: 2
    EOF
    ```

## <a name="qp0-access-restriction"></a>Ограничение доступа qp0

Чтобы предотвратить доступ к оборудованию низкого уровня, что может привести к уязвимостям безопасности, пара очередей 0 недоступна для гостевых виртуальных машин. Это должно влиять только на действия, обычно связанные с администрированием сетевого адаптера ConnectX-5, и на выполнение некоторых диагностических проверок InfiniBand, таких как ибдиагнет, но не сами приложения конечных пользователей.

## <a name="dram-on-hb-series-vms"></a>DRAM на виртуальных машинах серии ХБ

Виртуальные машины серии ХБ в настоящее время могут предоставлять в Гостевые виртуальные машины только 228 ГБ ОЗУ. Аналогичным образом 458 ГБ на HBv2 и 448 ГБ на виртуальных машинах HBv3. Это связано с известным ограничением низкоуровневой оболочки Azure, чтобы предотвратить назначение страниц локальной памяти DRAM для виртуальных машин AMD КККС (в доменах NUMA), зарезервированных для гостевой виртуальной машины.

## <a name="gss-proxy"></a>Прокси-сервер GSS

В прокси-сервере GSS имеется известная ошибка в CentOS/RHEL 7,5, которая может переявляться при использовании NFS в качестве значительного снижения производительности и скорости реагирования. Это можно устранить с помощью следующих действий.

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Очистка кэша

В системах HPC часто бывает полезно очистить память после завершения задания, прежде чем следующему пользователю будет назначен тот же узел. После запуска приложений в Linux может оказаться, что объем доступной памяти сокращается при увеличении памяти буфера, несмотря на то, что не выполняются никакие приложения.

![Снимок экрана командной строки перед очисткой](./media/known-issues/cache-cleaning-1.png)

`numactl -H`При использовании будет показано, какие NUMAnode памяти буферизованы (возможно, все). В Linux пользователи могут очищать кэши тремя способами, чтобы вернуть буферную или кэшированную память в "Free". Необходимо быть корневым или иметь разрешения sudo.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Снимок экрана командной строки после очистки](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Предупреждения ядра

При загрузке виртуальной машины серии ХБ в Linux можно игнорировать следующие предупреждающие сообщения ядра. Это связано с известным ограничением низкоуровневой оболочки Azure, которое будет решаться с течением времени.

```console
[  0.004000] WARNING: CPU: 4 PID: 0 at arch/x86/kernel/smpboot.c:376 topology_sane.isra.3+0x80/0x90
[  0.004000] sched: CPU #4's llc-sibling CPU #0 is not on the same node! [node: 1 != 0]. Ignoring dependency.
[  0.004000] Modules linked in:
[  0.004000] CPU: 4 PID: 0 Comm: swapper/4 Not tainted 3.10.0-957.el7.x86_64 #1
[  0.004000] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007 05/18/2018
[  0.004000] Call Trace:
[  0.004000] [<ffffffffb8361dc1>] dump_stack+0x19/0x1b
[  0.004000] [<ffffffffb7c97648>] __warn+0xd8/0x100
[  0.004000] [<ffffffffb7c976cf>] warn_slowpath_fmt+0x5f/0x80
[  0.004000] [<ffffffffb7c02b34>] ? calibrate_delay+0x3e4/0x8b0
[  0.004000] [<ffffffffb7c574c0>] topology_sane.isra.3+0x80/0x90
[  0.004000] [<ffffffffb7c57782>] set_cpu_sibling_map+0x172/0x5b0
[  0.004000] [<ffffffffb7c57ce1>] start_secondary+0x121/0x270
[  0.004000] [<ffffffffb7c000d5>] start_cpu+0x5/0x14
[  0.004000] ---[ end trace 73fc0e0825d4ca1f ]---
```


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об оптимальной настройке рабочих нагрузок для производительности и масштабируемости см. в статье с обзором виртуальных машин серии [HB](hb-series-overview.md) и [HC](hc-series-overview.md).
- Ознакомьтесь с последними объявлениями, примерами рабочей нагрузки HPC и результатами производительности в [блогах сообщества разработчиков Azure](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Более высокоуровневое архитектурное представление выполнения рабочих нагрузок HPC см. в статье [высокопроизводительные вычисления (HPC) в Azure](/azure/architecture/topics/high-performance-computing/).
