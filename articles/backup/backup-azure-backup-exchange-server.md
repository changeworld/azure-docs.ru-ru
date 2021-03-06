---
title: Резервное копирование сервера Exchange с помощью System Center DPM
description: Узнайте, как выполнить резервное копирование Exchange Server в службу архивации Azure с помощью System Center 2012 R2 DPM
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: ee89af311619922fa6ca585381d70ca66955f36a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91271653"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>Резервное копирование Exchange Server в службу архивации Azure с помощью System Center 2012 R2 DPM

В этой статье описывается настройка Data Protection Manager (DPM) в System Center 2012 R2 для резервного копирования Microsoft Exchange Server в службу архивации Azure.  

## <a name="updates"></a>Обновления

Для успешной регистрации сервера DPM в службе архивации Azure вам потребуется установить последний накопительный пакет обновления для System Center 2012 R2 DPM и последнюю версию агента резервного копирования Azure. Скачать последний накопительный пакет обновления можно из [каталога Microsoft](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> В примерах в этой статье установлен агент резервного копирования Azure версии 2.0.8719.0, а в System Center 2012 R2 DPM установлен накопительный пакет обновления 6.
>
>

## <a name="prerequisites"></a>Предварительные условия

Прежде чем продолжить, выполните все [предварительные требования](backup-azure-dpm-introduction.md#prerequisites-and-limitations) по использованию Microsoft Azure Backup для защиты рабочих нагрузок. Список предварительных требований:

* На сайте Azure должно быть создано хранилище службы архивации.
* Учетные данные агента и хранилища должны быть загружены на сервер DPM.
* Агент должен быть установлен на сервере DPM.
* Для регистрации сервера DPM должны использоваться учетные данные хранилища.
* Если вы защищаете Exchange 2016, выполните обновление до DPM 2012 R2 UR9 или более поздней версии.

## <a name="dpm-protection-agent"></a>Агент защиты DPM

Чтобы установить агент защиты DPM на сервере Exchange, выполните следующие действия.

1. Убедитесь, что брандмауэры настроены правильно. Ознакомьтесь со статьей [Настройка исключений брандмауэра для агента](/system-center/dpm/configure-firewall-settings-for-dpm).
2. Установите агент на сервере Exchange Server, выбрав **Management > агенты > установить** в консоль администрирования DPM. Подробные инструкции см. в статье [Установка агента защиты DPM](/system-center/dpm/deploy-dpm-protection-agent).

## <a name="create-a-protection-group-for-the-exchange-server"></a>Создание группы защиты для сервера Exchange Server

1. В консоль администрирования DPM выберите **Защита**, а затем на ленте инструментов щелкните **создать** , чтобы открыть мастер **создания новой группы защиты** .
2. На экране **приветствия** мастера нажмите кнопку **Далее**.
3. На экране **Выбор типа группы защиты** выберите **серверы** и нажмите кнопку **Далее**.
4. Выберите базу данных Exchange Server, которую необходимо защитить, и нажмите кнопку **Далее**.

   > [!NOTE]
   > Если вы защищаете Exchange 2013, проверьте [Предварительные требования к exchange 2013](/system-center/dpm/back-up-exchange).
   >
   >

    В следующем примере выбрана база данных Exchange 2010.

    ![Выберите членов группы](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Выберите способ защиты данных.

    Укажите имя группы защиты и выберите оба следующих параметра:

   * «Мне нужна краткосрочная защита с использованием диска»;
   * «Мне нужна оперативная защита».
6. Выберите **Далее**.
7. Выберите параметр **Запустить программу Eseutil для проверки целостности данных** , чтобы проверить целостность баз данных Exchange Server.

    После выбора этого параметра Проверка согласованности резервного копирования будет выполняться на сервере DPM во избежание трафика ввода-вывода, созданного при выполнении команды **eseutil** на сервере Exchange.

   > [!NOTE]
   > Чтобы использовать этот параметр, скопируйте файлы Ese.dll и Eseutil.exe в каталог C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin на сервере DPM. В противном случае возникнет следующая ошибка:   
   > ![ошибка Eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Выберите **Далее**.
9. Выберите базу данных для **резервного** копирования, а затем нажмите кнопку **Далее**.

   > [!NOTE]
   > Если не выбрать параметр "полная архивация" по крайней мере для одной копии базы данных DAG, журналы не будут обрезаны.
   >
   >
10. Настройте цели **краткосрочного резервного копирования**, а затем нажмите кнопку **Далее**.
11. Проверьте свободное место на диске и нажмите кнопку **Далее**.
12. Выберите время, когда сервер DPM будет создавать начальную репликацию, а затем нажмите кнопку **Далее**.
13. Выберите параметры проверки согласованности, а затем нажмите кнопку **Далее**.
14. Выберите базу данных, для которой требуется создать резервную копию в Azure, а затем нажмите кнопку **Далее**. Пример:

    ![Выбор оперативной защиты данных](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Определите расписание для **Azure Backup**, а затем нажмите кнопку **Далее**. Пример:

    ![Выбор расписания оперативного резервного копирования](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Обратите внимание, что точки оперативного восстановления создаются на основании точек быстрого полного восстановления. Поэтому необходимо запланировать точку восстановления в сети после времени, указанного для быстрой полной точки восстановления.
    >
    >
16. Настройте политику хранения для **Azure Backup**, а затем нажмите кнопку **Далее**.
17. Выберите вариант оперативной репликации и нажмите кнопку **Далее**.

    Если база данных имеет большой размер, процедура создания первой резервной копии по сети может занять много времени. Чтобы избежать этого, можно создать автономную резервную копию.  

    ![Выбор политики оперативного хранения](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Подтвердите параметры и нажмите кнопку **создать группу**.
19. Щелкните **Закрыть**.

## <a name="recover-the-exchange-database"></a>Восстановление базы данных Exchange

1. Чтобы восстановить базу данных Exchange, выберите **Восстановление** в консоль администрирования DPM.
2. Выберите базу данных Exchange, которую требуется восстановить.
3. Выберите точку оперативного восстановления из раскрывающегося списка *Время восстановления* .
4. Нажмите кнопку **восстановить** , чтобы запустить **Мастер восстановления**.

Для точек оперативного восстановления существует пять типов восстановления:

* **Восстановить в исходное расположение сервера Exchange.** Данные будут восстановлены на исходный сервер Exchange Server.
* **Восстановить в другую базу данных на сервере Exchange.** Данные будут восстановлены в другую базу данных на другом сервере Exchange Server.
* **Восстановить в базу данных восстановления.** Данные будут восстановлены в базу данных восстановления Exchange (RDB).
* **Копировать в сетевую папку.** Данные будут восстановлены в сетевую папку.
* **Копировать на ленту.** Если у вас имеется ленточная библиотека или изолированный ленточный накопитель, подключенные и настроенные на сервере DPM, точка восстановления будет скопирована на свободную ленту.

    ![Выбор оперативной репликации](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Часто задаваемые вопросы о службе архивации Azure](backup-azure-backup-faq.md)
