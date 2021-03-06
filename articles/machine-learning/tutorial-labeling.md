---
title: Руководство по Создание проекта маркировки для классификации изображений
titleSuffix: Azure Machine Learning
description: Узнайте о том, как управлять процессом маркировки изображений, чтобы использовать их в моделях многоклассовой классификации.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: sgilley
author: sdgilley
ms.reviewer: ranku
ms.date: 04/09/2020
ms.custom: data4ml
ms.openlocfilehash: 41e93584937ca10740e9ee0be3353d1edf5efb3e
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2021
ms.locfileid: "107587686"
---
# <a name="tutorial-create-a-labeling-project-for-multi-class-image-classification"></a>Руководство по Создание проекта маркировки для многоклассовой классификации изображений 


В этом руководстве показано, как управлять процессом маркировки (добавления меток) при подготовке данных для создания моделей машинного обучения. Маркировка данных в Машинном обучении Azure предоставляется в общедоступной предварительной версии.

Если вы хотите обучить модель машинного обучения для классификации изображений, вам потребуются сотни или даже тысячи изображений с правильными метками.  Машинное обучение Azure помогает управлять работой команды специалистов, которые маркируют ваши данные.
 
В этом руководстве используются изображения кошек и собак.  Каждое изображение содержит либо кошку, либо собаку, поэтому такой проект относится к *многоклассовой* классификации. Вы узнаете, как:

> [!div class="checklist"]
>
> * Создайте учетную запись хранения Azure и передайте в нее изображения.
> * Создайте рабочую область машинного обучения Azure.
> * Создайте проект маркировки для многоклассовой классификации изображений.
> * Присвойте метки данным.  Эту задачу вы можете выполнить самостоятельно или с привлечением маркировщиков.
> * Завершите работу над проектом, проверив и экспортировав данные.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://aka.ms/AMLFree).

## <a name="create-a-workspace"></a>Создание рабочей области

Рабочая область машинного обучения Azure — это основной ресурс в облаке для экспериментов, обучения и развертывания моделей машинного обучения. Она связывает подписку и группу ресурсов Azure с легко используемым объектом в службе.

Существует много [способов создания рабочей области](how-to-manage-workspace.md). В этом руководстве показано, как создать рабочую область с помощью портала Azure, веб-консоли для управления ресурсами Azure.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

## <a name="start-a-labeling-project"></a>Запуск проекта маркировки

Далее вы будете управлять проектом маркировки данных в Студии машинного обучения Azure — объединенном интерфейсе, который включает средства машинного обучения для выполнения сценариев обработки и анализа данных специалистами всех уровней. Студия не поддерживается в браузерах Internet Explorer.

1. Войдите в [Студию машинного обучения Azure](https://ml.azure.com).

1. Выберите свою подписку и рабочую область, которую создали.

### <a name="create-a-datastore"></a><a name="create-datastore"></a>Создание хранилища данных

Хранилища данных Машинного обучения Azure используются для хранения таких данных о подключении, как идентификатор подписки и авторизация маркеров. В нашем примере показано, как с помощью хранилища данных подключиться к учетной записи хранения, которая содержит изображения для этого руководства.

1. В левой части рабочей области выберите **Хранилища данных**.

1. Выберите **+ Новое хранилище данных**.

1. Заполните форму следующими данными.

    Поле|Описание 
    ---|---
    Имя хранилища данных | Присвойте имя хранилищу данных.  Здесь мы используем **labeling_tutorial**.
    Тип хранилища данных | Выберите тип хранилища.  Здесь мы используем **хранилище BLOB-объектов Azure**, которое лучше всего подходит для изображений.
    Метод выбора учетной записи | Выберите **Ввести вручную**.
    URL-адрес | `https://azureopendatastorage.blob.core.windows.net/openimagescontainer`
    Authentication type (Тип проверки подлинности) | Выберите **Маркер SAS**.
    Ключ учетной записи | `?sv=2019-02-02&ss=bfqt&srt=sco&sp=rl&se=2025-03-25T04:51:17Z&st=2020-03-24T20:51:17Z&spr=https&sig=7D7SdkQidGT6pURQ9R4SUzWGxZ%2BHlNPCstoSRRVg8OY%3D`

1. Чтобы создать хранилище данных, щелкните **Создать**.

### <a name="create-a-labeling-project"></a>Создание проекта маркировки

Теперь у вас есть доступ к данным для маркировки. Это значит, что вы можете создать проект маркировки.

1. В верхней части страницы щелкните **Проекты**.

1. Выберите **+ Добавить проект**.

    :::image type="content" source="media/tutorial-labeling/create-project.png" alt-text="Создание проекта":::

### <a name="project-details"></a>сведения о проекте;

1. Используйте следующие сведения для ввода в форму **Сведения о проекте**.

    Поле|Описание 
    ---|---
    Имя проекта | Дайте имя вашему проекту.  Здесь мы будем использовать **tutorial-cats-n-dogs**.
    Тип задачи маркировки | Выберите **Многоклассовая классификация изображений**.  
    
    Чтобы продолжить создание проекта, щелкните **Далее**.

### <a name="select-or-create-a-dataset"></a>Выбор или создание набора данных

1.   В форме **Выбор или создание набора данных** выберите второй вариант (**Создать набор данных**), а затем щелкните ссылку **Из хранилища данных**.

1. Используйте следующие данные для ввода в форму **Создание набора данных из хранилища данных**.

    1. В форме **Основные сведения** добавьте имя. В нашем примере это **images-for-tutorial**.  При желании добавьте описание.  Выберите **Далее**.
    1. В форме **Выбор хранилища данных** выберите **Ранее созданное хранилище данных**, щелкните имя хранилища данных и щелкните **Выберите хранилище данных**.
    1. На следующей странице убедитесь, что выбрано правильное хранилище данных. В противном случае выберите элемент **Ранее созданное хранилище данных** и повторите предыдущий шаг.
    1. Затем в той же форме **Выбор хранилища данных** щелкните **Обзор** и выберите **MultiClass - DogsCats**.  Щелкните **Сохранить**, чтобы выбрать для использования путь **/MultiClass - DogsCats**.
    1. Щелкните **Далее**, чтобы подтвердить эти сведения, а затем **Создать**, чтобы создать набор данных.
    1. Выберите окружность рядом с именем набора данных в списке, например **images-for-tutorial**.

1. Чтобы продолжить создание проекта, щелкните **Далее**.

### <a name="incremental-refresh"></a>Добавочное обновление

Если вам нужно добавить новые изображения в набор данных проекта, используйте добавочное обновление, чтобы найти и добавить их.  При включении этой функции проект будет периодически проверять наличие новых изображений.  Вы не будете добавлять новые изображения в хранилище данных в этом учебнике, поэтому не отмечайте этот параметр.

Нажмите кнопку **Далее**, чтобы продолжить.

### <a name="label-classes"></a>Классы меток

1. В форме **Классы меток** введите имя метки, а затем щелкните **+ Добавить метку**, чтобы ввести следующую метку.  Для этого проекта метки мы используем метки **Cat**, **Dog** и **Uncertain**.

1. Завершив добавление меток, щелкните **Далее**.

### <a name="labeling-instructions"></a>Инструкции по маркировке

1. В форме **Инструкции по маркировке** можно указать ссылку на веб-сайт с подробными инструкциями для маркировщиков.  В нашем примере мы ничего не указываем.

1. Также вы можете ввести прямо в форму краткое описание задачи.  Введите **Руководство по маркировке — кошки и собаки**.

1. Выберите **Далее**.

1. В разделе **Маркировка с помощью Машинного обучения** ничего не указывайте. Для маркировки с помощью Машинного обучения требуется больше данных, чем мы используем в этом руководстве.

1. Щелкните **Создать проект**.

Эта страница не обновляется автоматически. Подождите немного и вручную обновите страницу. Состояние проекта должно измениться на **Создано**.

## <a name="start-labeling"></a>Запуск маркировки

Итак, вы настроили все нужные ресурсы Azure и проект маркировки данных. Теперь можно маркировать данные.

### <a name="tag-the-images"></a>Присвоение тегов изображениям

В этой части руководства показано, как временно изменить роль *администратора проекта* на *маркировщика*.  Маркировщиком могу стать все, у кого есть права участника на доступ к рабочей области.

1. В [Студии машинного обучения](https://ml.azure.com) выберите слева **Маркировка данных**, чтобы найти нужный проект.  

1. Выберите **ссылку для маркировки** для проекта.

1. Ознакомьтесь с инструкциями, а затем щелкните **Задачи**.

1. Выберите справа эскиз макета, соответствующий числу изображений для маркировки за один прием. Вам нужно будет промаркировать все эти изображения, прежде чем продолжать работу. Переключайте макеты только в тот момент, когда отображается новая страница данных без меток. Переключение макетов приводит к очистке всех несохраненных тегов на странице.

1. Выберите одно или несколько изображений, а затем нужную метку для маркировки все выбранных изображений. Этот тег отобразится под каждым из выбранных изображений.  Продолжайте процесс, пока метки не будут присвоены всем изображениям на странице.  Чтобы выбрать сразу все отображаемые изображения, щелкните **Выбрать все**. Чтобы присвоить метку, нужно выбрать хотя бы одно изображение.


    > [!TIP]
    > Первые девять тегов можно выбирать нажатием цифровых клавиш на клавиатуре.

1. Когда все изображения на странице будут промаркированы, щелкните **Отправить**, чтобы отправить эти метки.

    ![Добавление тегов к изображениям](media/tutorial-labeling/catsndogs.gif)

1. После отправки тегов для отображаемых данных Azure обновит страницу, предоставив новый набор изображений из рабочей очереди.

## <a name="complete-the-project"></a>Завершение проекта

Теперь вы снова вернетесь к роли *администратора проекта* для проекта маркировки.

Как руководителю, вам нужно проверить работу своих маркировщиков.  

### <a name="review-labeled-data"></a>Проверка помеченных данных

1. В [Студии машинного обучения](https://ml.azure.com) выберите слева **Маркировка данных**, чтобы найти нужный проект.  

1. Щелкните ссылку с именем проекта.

1. На панели мониторинга отобразится ход работы над проектом.

1. Вверху страницы щелкните **Данные**.

1. Слева выберите **Помеченные данные**, чтобы просмотреть изображения с уже присвоенными тегами.  

1. Если у вас возникнут возражения по поводу присвоенной метки, выберите соответствующее изображение и щелкните **Отклонить** внизу страницы.  Эта метка будет удалена, а изображение будет помещено в очередь изображений без меток.

### <a name="export-labeled-data"></a>Экспорт помеченных данных

Вы можете в любое время экспортировать данные меток для эксперимента машинного обучения. Пользователи часто повторяют экспорт и обучают несколько разных моделей, не дожидаясь маркировки всех изображений.

Метки изображений можно экспортировать в [формате COCO](http://cocodataset.org/#format-data) или в виде набора данных Машинного обучения Azure. Этот формат набора данных позволяет легко применить данные для обучения в Машинном обучении Azure.  

1. В [Студии машинного обучения](https://ml.azure.com) выберите слева **Маркировка данных**, чтобы найти нужный проект.  

1. Щелкните ссылку с именем проекта.

1. Щелкните **Экспорт** и выберите вариант **Экспортировать как набор данных Машинного обучения Azure**. 

    Ход выполнения экспорта отображается прямо под кнопкой **Экспорт**. 

1. Когда экспорт меток успешно завершится, щелкните **Наборы данных** слева, чтобы просмотреть результаты.

## <a name="clean-up-resources"></a>Очистка ресурсов


[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Инструкции по обучению модели машинного обучения для распознавания изображений](/azure/machine-learning/how-to-use-labeled-dataset).

