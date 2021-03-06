---
title: Новые возможности Azure Key Vault
description: Последние обновления для Azure Key Vault
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 01/12/2020
ms.author: mbaldwin
ms.openlocfilehash: ef2c42ae76dc5e1511494635904228a8b574d483
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "98132194"
---
# <a name="whats-new-for-azure-key-vault"></a>Новые возможности Azure Key Vault

Ниже описаны новые возможности Azure Key Vault. Объявления о новых функциях и улучшениях также можно найти на [странице обновлений Azure для Key Vault](https://azure.microsoft.com/updates/?category=security&query=Key%20vault).

## <a name="october-2020"></a>Октябрь 2020 г.

> [!WARNING]
> Эти обновления могут повлиять на реализации Azure Key Vault.

Для поддержки [обратимого удаления по умолчанию](#soft-delete-on-by-default) в командлеты PowerShell для Azure Key Vault внесены два изменения:

- параметры DisableSoftDelete и EnableSoftDelete командлета [Update-AzKeyVault](/powershell/module/az.keyvault/update-azkeyvault) объявлены нерекомендуемыми;
- выходные данные командлета [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret) больше не содержат атрибут `SecretValueText`.

## <a name="july-2020"></a>Июль 2020 г.

> [!WARNING]
> Эти два обновления могут повлиять на реализации Azure Key Vault.

### <a name="soft-delete-on-by-default"></a>Включенное по умолчанию обратимое удаление

**Обратимое удаление должно быть включено для всех хранилищ ключей** — новых и существующих. Возможность отказаться от обратимого удаления перестанет поддерживаться в ближайшие месяцы. Подробные сведения об этом критическом изменении, а также инструкции по поиску затронутых хранилищ ключей и их предварительному обновлению см. в статье [Включение обратимого удаления во всех хранилищах ключей](soft-delete-change.md).

### <a name="azure-tls-certificate-changes"></a>Изменения TLS-сертификатов Azure

Корпорация Майкрософт обновляет службы Azure для использования TLS-сертификатов от других корневых центров сертификации (ЦС). Это изменение связано с тем, что текущие сертификаты ЦС не соответствуют одному из базовых требований организации CA/Browser Forum.  См. сведения об [изменении TLS-сертификатов Azure](../../security/fundamentals/tls-certificate-changes.md).

## <a name="june-2020"></a>Июнь 2020 г.

Решение Azure Monitor для Key Vault теперь доступно в предварительной версии.  Azure Monitor обеспечивает комплексный мониторинг хранилищ ключей в едином представлении запросов, производительности, сбоев и задержки для Key Vault. Дополнительные сведения см. в статье об [Azure Monitor для Key Vault (предварительная версия)](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="may-2020"></a>Май 2020 г.

Служба создания собственных ключей (BYOK) в Key Vault уже общедоступна. См. [спецификацию по службе создания собственных ключей в Azure Key Vault](../keys/byok-specification.md) и узнайте, как [импортировать защищенные модулем HSM ключи в Key Vault (BYOK)](../keys/hsm-protected-keys-byok.md).

## <a name="march-2020"></a>Март 2020 г.

Частные конечные точки доступны в предварительной версии. Приватный канал Azure обеспечивает доступ к Azure Key Vault и к размещенным в Azure службам клиентов или партнеров через частную конечную точку виртуальной сети.  Узнайте, как [интегрировать Key Vault с Приватным каналом Azure](private-link-service.md).

## <a name="2019"></a>2019

- Выпуск пакетов SDK Azure Key Vault нового поколения. Примеры их использования см. в кратких руководствах по секретам Azure Key Vault для [Python](../secrets/quick-create-python.md), [.NET](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md) и [Node.js](../secrets/quick-create-node.md).
- Новые политики Azure для управления сертификатами хранилищ ключей. См. статью [Встроенные определения в Политике Azure для Key Vault](../policy-reference.md).
- Расширение виртуальной машины в Azure Key Vault теперь общедоступно.  См. статьи [Расширение виртуальной машины в Key Vault для Linux](../../virtual-machines/extensions/key-vault-linux.md) и [Расширение виртуальной машины в Key Vault для Windows](../../virtual-machines/extensions/key-vault-windows.md).
- Управление секретами с учетом событий для Azure Key Vault теперь доступно в Сетке событий Azure. Дополнительные сведения см. в статье [Схема Сетки событий для событий в Azure Key Vault](../../event-grid/event-schema-key-vault.md], а также узнайте, как [получать оповещения хранилища ключей и реагировать на них с помощью Сетки событий Azure](event-grid-tutorial.md).

## <a name="2018"></a>2018

Новые функции и интеграции, выпущенные в этом году:

- Интеграция с Функциями Azure. Пример сценария с использованием [Функций Azure](../../azure-functions/index.yml) для операций с хранилищами ключей см. в статье [Автоматизация смены секретов](../secrets/tutorial-rotation.md).
- [Интеграция с помощью Azure Databricks.](/azure/databricks/scenarios/store-secrets-azure-key-vault) Благодаря ей Azure Databricks теперь поддерживает два типа областей секретов: на основе Azure Key Vault и на основе Databricks. Дополнительные сведения см. в разделе [Создание области секретов на основе Azure Key Vault](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope).
- [Конечные точки службы для виртуальной сети для Azure Key Vault](overview-vnet-service-endpoints.md).

## <a name="2016"></a>2016

Новые функции, выпущенные в этом году:

- Ключи управляемой учетной записи хранения. Функция "Ключи учетной записи хранения" упрощает интеграцию со службой хранилища Azure. Дополнительные сведения см. в статье [Ключи учетной записи хранения Azure Key Vault](../secrets/overview-storage-keys.md).
- Обратимое удаление. Функция обратимого удаления улучшает защиту данных для хранилищ ключей и его объектов. Дополнительные сведения см. в статье [Общие сведения об обратимом удалении в Azure Key Vault](./soft-delete-overview.md).

## <a name="2015"></a>2015

Новые функции, выпущенные в этом году:
- Управление сертификатами. Эта функция добавлена 26 сентября 2016 года для общедоступной версии 2015-06-01.

О выходе общедоступной версии (2015-06-01) было объявлено 24 июня 2015 г. В этот выпуск внесены следующие изменения.
- Удаление ключа: удалено поле use.
- Получение сведений о ключе: удалено поле use.
- Импорт ключа в хранилище: удалено поле use.
- Восстановление ключа: удалено поле use.
- В алгоритмах RSA изменено RSA_OAEP на RSA-OAEP. Ознакомьтесь со статьей [About keys, secrets, and certificates](about-keys-secrets-certificates.md) (Сведения о ключах, секретах и сертификатах).

О выходе второй предварительной версии (2015-02-01-preview) было объявлено 20 апреля 2015 г. Дополнительные сведения см. в записи блога [REST API Update](/archive/blogs/kv/rest-api-update) (Обновление REST API). Были обновлены следующие задачи:

- Составление списка ключей в хранилище. Добавлена поддержка разбиения на страницы для выполнения операции.
- Составление списка версий ключа. Добавлена операция перечисления версий ключа.
- Составление списка секретов в хранилище. Добавлена поддержка разбиения на страницы.
- Составление списка версий секрета. Добавлена операция перечисления версий секрета.
- Все операции. В атрибуты добавлены метки времени создания или обновления.
- Создание секрета. В секреты добавлен Content-Type.
- Создание ключа. Добавлены теги в качестве дополнительных сведений.
- Создание секрета. Добавлены теги в качестве дополнительных сведений.
- Обновление ключа. Добавлены теги в качестве дополнительных сведений.
- Обновление секрета. Добавлены теги в качестве дополнительных сведений.
- Изменен максимальный размер для секретов (с 10 КБ на 25 КБ). Ознакомьтесь со статьей [About keys, secrets, and certificates](about-keys-secrets-certificates.md) (Сведения о ключах, секретах и сертификатах).

## <a name="2014"></a>2014

О выходе первой предварительной версии (2014-12-08-preview) было объявлено 8 января 2015 г.

## <a name="next-steps"></a>Дальнейшие действия

Если у вас возникли вопросы, свяжитесь с нами через [службу поддержки](https://azure.microsoft.com/support/options/).
