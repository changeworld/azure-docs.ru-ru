---
title: Краткое описание примера схемы HIPAA HITRUST 9.2
description: Краткое описание примера схемы HIPAA HITRUST 9.2. Этот пример схемы помогает клиентам оценить определенные средства управления HIPAA HITRUST 9.2.
ms.date: 04/02/2021
ms.topic: sample
ms.openlocfilehash: 168946319c11f31ee41594d82d9ff186dea232cd
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106386590"
---
# <a name="hipaa-hitrust-92-blueprint-sample"></a>Пример схемы HIPAA HITRUST 9.2

Пример схемы HIPAA HITRUST 9.2, используемый со службой [Политика Azure](../../policy/overview.md), предоставляет средства для обеспечения соответствия требованиям, чтобы вы могли оценивать отдельные средства управления HIPAA HITRUST 9.2. Схема помогает клиентам развертывать основной набор политик для любой развернутой в Azure архитектуры, в которой необходимо реализовать средства управления HIPAA HITRUST 9.2.

## <a name="control-mapping"></a>Сопоставление элементов управления

[Сопоставление средств управления службы "Политика Azure"](../../policy/samples/hipaa-hitrust-9-2.md) позволяет получить подробные сведения об определениях политик, включенных в эту схему, и о сопоставлении этих определений с **областями соответствия нормативным требованиям** и **средствами управления** в HIPAA HITRUST 9.2. Назначаемые архитектуре ресурсы оцениваются службой "Политика Azure" на предмет соответствия назначенным определениям политик. Дополнительные сведения см. в статье [Что такое служба "Политика Azure"?](../../policy/overview.md)

## <a name="deploy"></a>Развертывание

Чтобы развернуть пример схемы HIPAA HITRUST 9.2 Azure Blueprints, необходимо выполнить следующие действия:

> [!div class="checklist"]
> - создание схемы на основе примера;
> - установка метки копии образца **Опубликовано**;
> - назначение копии схемы существующей подписке;

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free), прежде чем начинать работу.

### <a name="create-blueprint-from-sample"></a>Создание схемы на основе примера

Для начала внедрите пример схемы, создав новую схему в среде на основе этого примера.

1. Выберите **Все службы** в левой области. Найдите и выберите пункт **Схемы**.

1. На странице **Начало работы** с левой стороны в разделе _Создание схемы_ щелкните кнопку **Создать**.

1. Найдите пример схемы **HIPAA HITRUST** в разделе _Другие примеры_ и выберите действие **Использовать этот пример**.

1. Введите _основные данные_ образца схемы.

   - **Имя схемы**. Укажите имя для копии примера схемы HIPAA HITRUST 9.2.
   - **Расположение определения**. Используйте кнопку с многоточием и выберите группу управления, в которой нужно сохранить копию примера.

1. В верхней части страницы выберите вкладку _Артефакты_ или внизу страницы щелкните **Далее: Артефакты**.

1. Просмотрите список артефактов, из которых состоит образец схемы. Многие артефакты имеют параметры, которые мы определим позднее. После завершения просмотра образца схемы выберите **Сохранить черновик**.

### <a name="publish-the-sample-copy"></a>Публикации копии образца

Теперь в вашей среде создана копия образца схемы. Она создана в режиме **Черновик**, и прежде чем назначить и развернуть эту копию, ее необходимо **опубликовать**. Вы можете изменить параметры своей копии этого примера схемы с учетом среды и требований, но такие изменения могут нарушить выравнивание с элементами управления HIPAA HITRUST 9.2.

1. Выберите **Все службы** в левой области. Найдите и выберите пункт **Схемы**.

1. В меню слева выберите страницу **Определения схем**. Примените фильтры, чтобы найти копию примера схемы, и выберите его.

1. В верхней части страницы выберите **Опубликовать схему**. В правой части новой страницы укажите **версию** для копии примера схемы. Это свойство позволяет вносить изменения позднее. Укажите **сведения об изменениях**, например "Первая версия, опубликованная из примера схемы HIPAA HITRUST 9.2". Затем щелкните **Опубликовать** в нижней части страницы.

### <a name="assign-the-sample-copy"></a>Назначение копии образца

После успешной **публикации** копию образца схемы можно назначить подписке в группе управления, в которой ее сохранили. На этом шаге указывают параметры, которые позволяют сделать каждое развертывание образца схемы уникальным.

1. Выберите **Все службы** в левой области. Найдите и выберите пункт **Схемы**.

1. В меню слева выберите страницу **Определения схем**. Примените фильтры, чтобы найти копию примера схемы, и выберите его.

1. В верхней части страницы определения схемы выберите **Назначить схему**.

1. Укажите значения параметра для назначения схемы.

   - Основы

     - **Подписки**. Выберите одну или несколько подписок, которые находятся в группе управления, в которой вы сохранили копию образца схемы. Если вы выберите более одной подписки, для каждой из них будет создано назначение с использованием введенных параметров.
     - **Имя назначения.** Имя автоматически заполняется на основе имени схемы.
       Измените его при желании или сохраните вариант по умолчанию.
     - **Расположение.** Выберите регион, в котором будет создано управляемое удостоверение. Azure Blueprint использует это управляемое удостоверение для развертывания всех артефактов в назначенную схему. Дополнительные сведения см. в статье [Управляемые удостоверения для ресурсов Azure](../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Версия определения схемы**. Выберите **опубликованную** версию копии примера схемы.

   - Блокировка назначения

     Выберите режим блокировки для схемы с учетом своей среды. Дополнительные сведения см. в разделе [Блокировка ресурсов схем](../concepts/resource-locking.md).

   - Управляемое удостоверение

     Сохраните значение по умолчанию _Назначено системой_ для управляемого удостоверения.

   - Параметры артефакта

     Параметры, определенные в этом разделе, применяются к артефакту, под которым они определены. Поскольку эти параметры определяются во время назначения схемы, они являются [динамическими](../concepts/parameters.md#dynamic-parameters). Полный список параметров артефактов с описаниями можно найти в [таблице параметров артефактов](#artifact-parameters-table).

1. После ввода всех параметров в нижней части страницы выберите **Назначить**. Это действие создает назначение схемы и начинает развертывание артефактов. Развертывание занимает около часа. Чтобы проверить состояние развертывания, откройте назначение схемы.

> [!WARNING]
> Служба Azure Blueprints и встроенные примеры схем предоставляются **бесплатно**. Ресурсы Azure оплачиваются согласно [тарифам на продукты](https://azure.microsoft.com/pricing/). С помощью [калькулятора цен](https://azure.microsoft.com/pricing/calculator/) вы можете оценить расходы на выполнение ресурсов, развертываемых этим примером схемы.

### <a name="artifact-parameters-table"></a>Таблица параметров артефактов

Следующая таблица содержит полный список параметров артефактов схемы.

|Имя артефакта |Имя параметра |Описание |
|---|---|---|
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Доступ через подключенную к Интернету конечную точку должен быть ограничен. |Включение или отключение мониторинга слишком нестрогих правил NSG для входящего трафика. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Учетные записи: состояние учетной записи "Гость" |Указывает, отключена ли локальная гостевая учетная запись. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Адаптивные элементы управления приложениями должны быть включены на виртуальных машинах. |Включение или отключение мониторинга списков разрешенных приложений в Центре безопасности Azure. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Разрешить одновременные подключения к Интернету или домену Windows |Укажите, следует ли запрещать компьютерам одновременный доступ к сети на основе домена и к сети, не являющейся доменной. При указании значения 0 одновременные подключения разрешены, а при указании значения 1 — запрещены. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Приложение API должно быть доступно только по HTTPS (версия 2) |Включение или отключение мониторинга использования HTTPS в приложении API (версия 2). |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Имена приложений (поддерживаются подстановочные знаки) |Список имен приложений, которые должны быть установлены, разделенный точками с запятой. Пример. Microsoft SQL Server 2014 (64-разрядная версия); Microsoft Visual Studio Code или Microsoft SQLServer 2014* (для указания всех приложений, имена которых начинаются с Microsoft SQL Server 2014). |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Аудит завершения процессов |Указывает, создаются ли события аудита при завершении процесса. Рекомендуется для мониторинга завершения критических процессов. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Аудит неограниченного сетевого доступа к учетным записям хранения |Включение или отключение мониторинга сетевого доступа к учетной записи хранения. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Аудит: немедленное отключение системы, если невозможно внести в журнал записи об аудите безопасности. |Выполняет аудит завершения работы системы, если невозможно зарегистрировать события безопасности. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Отпечатки сертификатов |Разделенный точками с запятой список отпечатков сертификатов, которые должны существовать в хранилище доверенных корневых сертификатов (Cert:\LocalMachine\Root). Например, THUMBPRINT1;THUMBPRINT2;THUMBPRINT3. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо включить журналы диагностики в учетных записях пакетной службы |Включение или отключение мониторинга журналов диагностики в учетных записях пакетной службы. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо включить журналы диагностики в концентраторе событий |Включение или отключение мониторинга журналов диагностики в учетных записях концентратора событий. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо включить журналы диагностики в службах поиска |Включение или отключение мониторинга журналов диагностики в службе "Поиск" Azure. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |В масштабируемых наборах виртуальных машин должны быть включены журналы диагностики. |Включение или отключение мониторинга журналов диагностики в Service Fabric. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Шифрование дисков должно применяться к виртуальным машинам. |Включение или отключение мониторинга шифрования дисков виртуальной машины. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Блокировать небезопасные гостевые входы |Указывает, разрешает ли клиент SMB небезопасные гостевые входы на сервер SMB. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |На виртуальных машинах должен применяться JIT-доступ к сети. |Включение или отключение мониторинга JIT-доступа к сети. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо закрыть порты управления на виртуальных машинах. |Включение или отключение мониторинга открытых портов управления на виртуальных машинах. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |MFA должна быть включена для учетных записей с разрешениями на запись в вашей подписке. |Включение или отключение мониторинга MFA для учетных записей с разрешениями на запись в подписке. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |MFA должна быть включена для учетных записей с разрешениями владельца в вашей подписке. |Включение или отключение мониторинга MFA для учетных записей с разрешениями владельца в подписке. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Сетевой доступ: удаленно доступные пути реестра |Указывает, какие пути реестра будут доступны по сети, независимо от пользователей и групп, перечисленных в списке управления доступом (ACL) в разделе реестра `winreg`. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Сетевой доступ: пути и подпути реестра, доступные через удаленное подключение |Указывает, какие пути и подпути реестра будут доступны по сети, независимо от пользователей и групп, перечисленных в списке управления доступом (ACL) в разделе реестра `winreg`. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Сетевой доступ: разрешать анонимный доступ к общим ресурсам. |Указывает, какие сетевые папки доступны анонимным пользователям. Конфигурация по умолчанию для этого параметра политики не оказывает практически никакого эффекта, так как все пользователи должны пройти проверку подлинности, прежде чем смогут получить доступ к общим ресурсам на сервере. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Консоль восстановления: разрешить копирование дискет и доступ ко всем дискам и папкам |Указывает, следует ли обеспечить доступность команды SET консоли восстановления, которая позволяет задать переменные среды для консоли восстановления. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Удаленная отладка должна быть отключена для приложений API. |Включение или отключение мониторинга удаленной отладки в приложении API. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Удаленная отладка в веб-приложении должна быть отключена |Включение или отключение мониторинга удаленной отладки в веб-приложении. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Требуемый период хранения (в днях) для журналов в учетных записях пакетной службы |Требуемый период хранения журналов диагностики (в днях). |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Требуемый период хранения (в днях) для журналов в службе "Поиск" Azure. |Требуемый период хранения журналов диагностики (в днях). |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Требуемый период хранения (в днях) для журналов в учетных записях концентратора событий |Требуемый период хранения журналов диагностики (в днях). |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Имя группы ресурсов учетной записи хранения (должна существовать) для развертывания параметров диагностики групп безопасности сети |Группа ресурсов, в которой будет создана учетная запись хранения. Эта группа ресурсов уже должна существовать. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Управление доступом на основе ролей (RBAC) должно использоваться в службе Kubernetes |Включение или отключение мониторинга служб Kubernetes без поддержки RBAC. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Предохранитель TDE управляемого экземпляра SQL должен быть зашифрован с помощью вашего собственного ключа |Включение или отключение мониторинга прозрачного шифрования данных (TDE) с поддержкой ваших собственных ключей. Такое шифрование обеспечивает дополнительную прозрачность и контроль для предохранителя TDE, повышенную безопасность за счет внешней службы на базе HSM, а также способствует разделению обязанностей. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |На серверах SQL предохранитель TDE должен быть зашифрован с использованием вашего ключа |Включение или отключение мониторинга прозрачного шифрования данных (TDE) с поддержкой ваших собственных ключей. Такое шифрование обеспечивает дополнительную прозрачность и контроль для предохранителя TDE, повышенную безопасность за счет внешней службы на базе HSM, а также способствует разделению обязанностей. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Префикс региональной учетной записи хранения для развертывания параметров диагностики групп безопасности сети |Этот префикс будет объединен с расположением группы безопасности сети, чтобы формировать имя созданной учетной записи хранения. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Обновления системы должны быть установлены в масштабируемых наборах виртуальных машин. |Включение или отключение отчетов об обновлениях системы в масштабируемых наборах виртуальных машин. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Обновления системы должны быть установлены в масштабируемых наборах виртуальных машин. |Включение или отключение отчетов об обновлениях системы в масштабируемых наборах виртуальных машин. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Отключить многоадресное разрешение имен |Указывает, включен ли LLMNR, дополнительный протокол разрешения имен, который осуществляет передачу данных в одной подсети с использованием многоадресной рассылки через соединение в локальной подсети. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо перенести виртуальные машины в новые ресурсы Azure Resource Manager. |Включение или отключение мониторинга классических вычислительных виртуальных машин. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Уязвимости в конфигурации безопасности в масштабируемом наборе виртуальных машин должны быть устранены. |Включение или отключение мониторинга уязвимостей ОС в масштабируемых наборах виртуальных машин. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Уязвимости должны быть устранены с помощью решения для оценки уязвимостей. |Включение или отключение обнаружения уязвимостей виртуальной машины при помощи решения для оценки уязвимостей. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Необходимо включить оценку уязвимости на управляемых экземплярах SQL. |Аудит управляемых экземпляров SQL, у которых отключена регулярная оценка уязвимостей. Решение "Оценка уязвимостей" может обнаруживать, отслеживать и помогать исправлять потенциальные уязвимости баз данных. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (домен): применение локальных правил брандмауэра |Указывает, разрешено ли локальным администраторам создавать локальные правила брандмауэра, которые применяются вместе с правилами брандмауэра, заданными в групповой политике для профиля домена. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (домен): поведение для исходящих подключений |Задает поведение исходящих подключений, которые не соответствуют исходящему правилу брандмауэра, для профиля домена. Значение по умолчанию, равное 0, означает разрешение подключений, а значение 1 — их запрет. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (домен): поведение для исходящих подключений |Задает поведение исходящих подключений, которые не соответствуют исходящему правилу брандмауэра, для профиля домена. Значение по умолчанию, равное 0, означает разрешение подключений, а значение 1 — их запрет. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (домен): отображение уведомлений |Указывает, будет ли брандмауэр Windows в режиме повышенной безопасности отображать уведомления для пользователя, если для программы заблокирован прием входящих подключений, для профиля домена. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (домен): использование параметров профиля |Указывает, использует ли брандмауэр Windows в режиме повышенной безопасности параметры профиля домена для фильтрации сетевого трафика. Если установлено значение "Выкл.", брандмауэр Windows в режиме повышенной безопасности не будет использовать правила брандмауэра и правила безопасности подключения для этого профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (частный): применение локальных правил безопасности подключений |Указывает, разрешено ли локальным администраторам создавать правила безопасности подключений, которые применяются вместе с правилами безопасности подключений, заданными в групповой политике для частного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (частный): применение локальных правил брандмауэра |Указывает, разрешено ли локальным администраторам создавать локальные правила брандмауэра, которые применяются вместе с правилами брандмауэра, заданными в групповой политике для частного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (частный): поведение для исходящих подключений |Задает поведение исходящих подключений, которые не соответствуют исходящему правилу брандмауэра, для частного профиля. Значение по умолчанию, равное 0, означает разрешение подключений, а значение 1 — их запрет. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (частный): отображение уведомлений |Указывает, будет ли брандмауэр Windows в режиме повышенной безопасности отображать уведомления для пользователя, если для программы заблокирован прием входящих подключений, для частного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (частный): использование параметров профиля |Указывает, использует ли брандмауэр Windows в режиме повышенной безопасности параметры частного профиля для фильтрации сетевого трафика. Если установлено значение "Выкл.", брандмауэр Windows в режиме повышенной безопасности не будет использовать правила брандмауэра и правила безопасности подключения для этого профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (общедоступный): применение локальных правил безопасности подключений |Указывает, разрешено ли локальным администраторам создавать правила безопасности подключений, которые применяются вместе с правилами безопасности подключений, заданными в групповой политике для общедоступного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (общедоступный): применение локальных правил брандмауэра |Указывает, разрешено ли локальным администраторам создавать локальные правила брандмауэра, которые применяются вместе с правилами брандмауэра, заданными в групповой политике для общедоступного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (общедоступный): поведение для исходящих подключений |Задает поведение исходящих подключений, которые не соответствуют исходящему правилу брандмауэра, для общедоступного профиля. Значение по умолчанию, равное 0, означает разрешение подключений, а значение 1 — их запрет. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (общедоступный): отображение уведомлений |Указывает, будет ли брандмауэр Windows в режиме повышенной безопасности отображать уведомления для пользователя, если для программы заблокирован прием входящих подключений, для общедоступного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows (общедоступный): использование параметров профиля |Указывает, использует ли брандмауэр Windows в режиме повышенной безопасности параметры общедоступного профиля для фильтрации сетевого трафика. Если установлено значение "Выкл.", брандмауэр Windows в режиме повышенной безопасности не будет использовать правила брандмауэра и правила безопасности подключения для этого профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows: домен: разрешить одноадресный ответ |Указывает, разрешает ли брандмауэр Windows в режиме повышенной безопасности локальному компьютеру получать одноадресные ответы на исходящие многоадресные или широковещательные сообщения; для профиля домена. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows: частный: разрешить одноадресный ответ |Указывает, разрешает ли брандмауэр Windows в режиме повышенной безопасности локальному компьютеру получать одноадресные ответы на исходящие многоадресные или широковещательные сообщения; для частного профиля. |
|Аудит элементов управления HITRUST/HIPAA и развертывание определенных расширений виртуальных машин для соответствия его требованиям |Брандмауэр Windows: общедоступный: разрешить одноадресный ответ |Указывает, разрешает ли брандмауэр Windows в режиме повышенной безопасности локальному компьютеру получать одноадресные ответы на исходящие многоадресные или широковещательные сообщения; для общедоступного профиля. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные статьи о схемах и способах их использования:

- Ознакомьтесь со сведениями о [жизненном цикле схем](../concepts/lifecycle.md).
- Узнайте, как использовать [статические и динамические параметры](../concepts/parameters.md).
- Научитесь настраивать [последовательность схемы](../concepts/sequencing-order.md).
- Узнайте, как применять [блокировку ресурсов схемы](../concepts/resource-locking.md).
- Узнайте, как [обновлять существующие назначения](../how-to/update-existing-assignments.md).
