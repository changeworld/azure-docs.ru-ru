---
title: Шифрование дисков Azure с помощью Azure AD (предыдущий выпуск)
description: Эта статья содержит предварительные требования для использования шифрования дисков Microsoft Azure с виртуальными машинами IaaS.
author: msmbaldwin
ms.service: virtual-machines
ms.subservice: disks
ms.topic: conceptual
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 8db9aad279c151073f8c674854d139276a84b064
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102555386"
---
# <a name="azure-disk-encryption-with-azure-ad-previous-release"></a>Шифрование дисков Azure с помощью Azure AD (предыдущий выпуск)

**Новый выпуск шифрования дисков Azure исключает необходимость предоставления параметра приложения Azure AD для включения шифрования дисков виртуальной машины. В новом выпуске больше не требуется предоставлять учетные данные Azure AD на этапе включения шифрования. Все новые виртуальные машины должны быть зашифрованы без параметров приложения Azure AD с помощью нового выпуска. Инструкции по включению шифрования дисков виртуальной машины с помощью нового выпуска см. в статье [Шифрование дисков Azure для виртуальных машин Windows](disk-encryption-overview.md). Виртуальные машины, которые уже были зашифрованы с помощью параметров приложения Azure AD, по-прежнему поддерживаются и должны продолжать поддерживаться с использованием синтаксиса AAD.**

Эта статья дополняет [Шифрование дисков Azure для виртуальных машин Windows](disk-encryption-overview.md) с дополнительными требованиями и необходимыми компонентами для шифрования дисков Azure с помощью Azure AD (предыдущий выпуск). Раздел [Поддерживаемые виртуальные машины и операционные системы](disk-encryption-overview.md#supported-vms-and-operating-systems) остается прежним.

## <a name="networking-and-group-policy"></a> Сетевые подключения и групповая политика

**Чтобы использовать функцию шифрования дисков Azure с применением старого синтаксиса параметров AAD, конфигурация конечной точки сети виртуальной машины IaaS должна соответствовать приведенным ниже требованиям:** 
  - Виртуальная машина IaaS должна иметь возможность подключиться к конечной точке Azure Active Directory \[login.microsoftonline.com\], чтобы получить маркер для подключения к хранилищу ключей.
  - Для записи ключей шифрования в ваше хранилище ключей виртуальная машина IaaS должна иметь возможность подключиться к конечной точке хранилища ключей.
  - Виртуальная машина IaaS должна иметь возможность подключиться к конечной точке службы хранилища Azure, в которой размещен репозиторий расширений Azure, и к учетной записи хранения Azure, в которой размещены VHD-файлы.
  -  Если ваша политика безопасности ограничивает доступ к Интернету с виртуальных машин Azure, можно разрешить указанный выше универсальный код ресурса (URI) и настроить определенное правило, чтобы разрешить исходящие подключения к данным IP-адресам. Дополнительные сведения см. в статье [Доступ к Azure Key Vault из-за брандмауэра](../../key-vault/general/access-behind-firewall.md).
  - Виртуальная машина для шифрования должна быть настроена на использование TLS 1,2 в качестве протокола по умолчанию. Если протокол TLS 1,0 был явно отключен и версия .NET не обновлялась до 4,6 или более поздней версии, в следующем реестре будет включено использование ADE для выбора более новой версии TLS:

```console
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001` 
```

**групповая политика:**
 - Решение шифрования дисков Azure использует внешний предохранитель ключа BitLocker для виртуальных машин IaaS под управлением Windows. Если виртуальные машины присоединены к домену, не применяйте групповые политики, требующие использования предохранителей TPM. Сведения о групповой политике "Разрешить использование BitLocker без совместимого TPM" см. в [справке по групповым политикам BitLocker](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

-  Политика BitLocker на виртуальных машинах, присоединенных к домену, с пользовательской групповой политикой должна включать следующий параметр: [Настройка хранилища пользователя сведений о восстановлении BitLocker — > разрешить 256-разрядный ключ восстановления](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Шифрование дисков Azure завершится ошибкой, если параметры настраиваемой групповой политики для BitLocker несовместимы. На компьютерах без соответствующего параметра политики может потребоваться применить новую политику, принудительно обновить ее (gpupdate.exe /force) и перезагрузить компьютер.  

## <a name="encryption-key-storage-requirements"></a>Требования к хранилищу ключей шифрования  

Службе шифрования дисков Azure необходимо Azure Key Vault, чтобы управлять секретами и ключами шифрования дисков и контролировать их. Хранилище ключей и виртуальные машины должны находиться в одном регионе и подписке Azure.

Дополнительные сведения см. [в статье Создание и Настройка хранилища ключей для шифрования дисков Azure с помощью Azure AD (предыдущий выпуск)](disk-encryption-key-vault-aad.md).
 
## <a name="next-steps"></a>Дальнейшие действия

- [Создание и Настройка хранилища ключей для шифрования дисков Azure с помощью Azure AD (предыдущий выпуск)](disk-encryption-key-vault-aad.md)
- [Включение шифрования дисков Azure с помощью Azure AD на виртуальных машинах Windows (предыдущий выпуск)](disk-encryption-windows-aad.md)
- [Скрипт CLI для подготовки необходимых компонентов для службы "Шифрование дисков Azure"](https://github.com/ejarvi/ade-cli-getting-started)
- [Скрипт PowerShell для подготовки необходимых компонентов для службы "Шифрование дисков Azure"](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
