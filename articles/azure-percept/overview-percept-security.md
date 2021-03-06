---
title: Общие сведения о безопасности Azure Percept
description: Дополнительные сведения о безопасности Azure Перцепт
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 03/24/2021
ms.custom: template-concept
ms.openlocfilehash: 93884fb87f87651054ffff0a04c4910de634a5eb
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105567650"
---
# <a name="azure-percept-security-overview"></a>Общие сведения о безопасности Azure Percept

Устройства Azure Перцепт разработаны с корнем оборудования доверия. Эта встроенная безопасность позволяет защищать данные вывода и датчики с учетом конфиденциальности, такие как камеры и микрофоны, а также обеспечивает проверку подлинности и авторизацию устройств для служб Azure Перцепт Studio.

> [!NOTE]
> Azure Перцепт DK лицензируется для использования только в средах разработки и тестирования.

## <a name="devices"></a>Устройства

### <a name="azure-percept-dk"></a>Azure Percept DK

Azure Перцепт DK включает в себя доверенный платформенный модуль (TPM) (TPM) версии 2,0, которую можно использовать для подключения устройства к службам подготовки устройств Azure (DPS) с дополнительной безопасностью. TPM — это общеотраслевой стандарт ISO из организация TCG. Дополнительные сведения о полной спецификации TPM 2,0 или спецификации ISO/IEC 11889 см. на [веб-сайте организация TCG](https://trustedcomputinggroup.org/resource/tpm-library-specification/) . Дополнительные сведения о том, как DPS может безопасно подготавливать устройства, см. в статье [Служба подготовки устройств центра Интернета вещей Azure — аттестация доверенного платформенного модуля](../iot-dps/concepts-tpm-attestation.md).

### <a name="azure-percept-system-on-modules-soms"></a>Azure Перцепт System-on-modules (SOM)

Для защиты доступа к внедренным датчикам AI в системе Microsoft Azure Перцепт концепция (SoM) и служба "звук SoM Azure Перцепт" включены модуль микроконтроллера (МИКРОКОНТРОЛЛЕРАМИ). При каждой загрузке встроенное по МИКРОКОНТРОЛЛЕРАМИ проверяет подлинность и авторизует средство AI Accelerator с помощью служб Azure Перцепт Studio, используя архитектуру механизма композиции идентификаторов устройств (КОСТей). КОСТИ работают, нарушая загрузку в слои и создавая уникальные секреты устройств (доменов обновления) для каждого уровня и конфигурации. Если в какой-либо точке цепочки загружается другой код или конфигурация, секреты будут отличаться. Дополнительные сведения о КОСТях см. в [спецификации "кости для рабочих групп](https://trustedcomputinggroup.org/work-groups/dice-architectures/)". Сведения о настройке доступа к Azure Перцепт Studio и требуемым службам см. в статье о [настройке брандмауэров для Azure ПЕРЦЕПТ DK](concept-security-configuration.md).

Устройства Azure Перцепт используют корень оборудования доверия для защиты встроенного по. Загрузочный диск обеспечивает целостность микропрограммы между ПЗУ и загрузчиком операционной системы, что, в свою очередь, обеспечивает целостность других программных компонентов, создавая цепочку доверия.

## <a name="services"></a>Службы

### <a name="iot-edge"></a>IoT Edge

Azure Перцепт DK подключается к Azure Перцепт Studio с дополнительной безопасностью и другими службами Azure, использующими протокол TLS. Azure Перцепт DK — это устройство с поддержкой Azure IoT Edge. IoT Edge среда выполнения — это набор программ, которые включают устройство в устройство IoT Edge. В совокупности компоненты среды выполнения IoT Edge позволяют устройствам IoT Edge получать код для выполнения на границе и передавать результаты. Azure Перцепт DK использует контейнеры DOCKER для изоляции рабочих нагрузок IoT Edge от операционной системы узла и приложений с поддержкой ребра. Дополнительные сведения о Azure IoT Edgeной платформе безопасности см. в статье [Диспетчер безопасности IOT Edge](../iot-edge/iot-edge-security-manager.md).

### <a name="device-update-for-iot-hub"></a>Обновление устройств для Центра Интернета вещей

Обновление устройства для центра Интернета вещей обеспечивает более безопасное, масштабируемое и надежное высокопроизводительное обновление, которое приводит к устанавливать возобновляемую безопасности на устройствах Azure Перцепт. Она предоставляет широкие возможности управления и обновления соответствия с помощью аналитических сведений. Azure Перцепт DK включает в себя предварительно интегрированное решение для обновления устройств, обеспечивающее устойчивое обновление (A/B) от встроенного по к уровням операционной системы.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Дополнительные сведения о конфигурациях брандмауэра и рекомендациях по безопасности](concept-security-configuration.md)

> [!div class="nextstepaction"]
> [Купить Azure Перцепт DK из Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)
