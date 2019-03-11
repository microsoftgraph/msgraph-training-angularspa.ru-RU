# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="5d0ce-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="5d0ce-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d0ce-102">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="5d0ce-102">Prerequisites</span></span>

<span data-ttu-id="5d0ce-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="5d0ce-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="5d0ce-104">[Узел node. js](https://nodejs.org) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="5d0ce-105">Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="5d0ce-106">(**Примечание:** это руководство было написано с узлом версии 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="5d0ce-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="5d0ce-108">[Угловой CLI](https://cli.angular.io/) , установленный на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="5d0ce-109">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="5d0ce-110">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="5d0ce-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="5d0ce-111">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="5d0ce-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="5d0ce-112">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="5d0ce-113">Регистрация веб-приложения с помощью портала регистрации приложений</span><span class="sxs-lookup"><span data-stu-id="5d0ce-113">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="5d0ce-114">Откройте браузер и перейдите на [портал регистрации приложений](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="5d0ce-114">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="5d0ce-115">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="5d0ce-116">В верхней части страницы выберите **Добавить приложение** .</span><span class="sxs-lookup"><span data-stu-id="5d0ce-116">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="5d0ce-117">**Примечание:** Если на странице отображается несколько кнопок **Добавить приложение** , выберите ту, которая соответствует списку **приложений** для конвергенции.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-117">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="5d0ce-118">На странице **Регистрация приложения** задайте для параметра **имя приложения** значение **учебник радиальный график** и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-118">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Снимок экрана: создание нового приложения на веб-сайте портала регистрации приложений](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="5d0ce-120">На странице **Регистрация учебника** по радиальным графикам в разделе **свойства** скопируйте **идентификатор приложения** так, как он понадобится позже.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-120">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом только что созданного приложения](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="5d0ce-122">ПроКрутите окно вниз до раздела **платформы** .</span><span class="sxs-lookup"><span data-stu-id="5d0ce-122">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="5d0ce-123">Нажмите кнопку **Добавить платформу**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-123">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="5d0ce-124">В диалоговом окне **Добавление платформы** выберите **веб**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-124">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Снимок экрана: создание платформы для приложения](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="5d0ce-126">В поле **веб-** платформа введите URL-адрес `http://localhost:4200` для **URL-адресов перенаправления**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-126">In the **Web** platform box, enter the URL `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Снимок экрана: недавно добавленная веб-платформа для приложения](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="5d0ce-128">ПроКрутите страницу вниз и выберите команду **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-128">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="5d0ce-129">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="5d0ce-129">Configure the sample</span></span>

1. <span data-ttu-id="5d0ce-130">Переименуйте `oauth.ts.example` файл в `oauth.ts`.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-130">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="5d0ce-131">Измените `oauth.ts` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-131">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="5d0ce-132">Замените `YOUR_APP_ID_HERE` идентификатором **приложения** , полученНым на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="5d0ce-133">В интерфейсе командной строки (CLI) перейдите к этому каталогу и выполните следующую команду, чтобы установить требования.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-133">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="5d0ce-134">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5d0ce-134">Run the sample</span></span>

1. <span data-ttu-id="5d0ce-135">Выполните следующую команду в командной панели CLI, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-135">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="5d0ce-136">Откройте браузер и перейдите по адресу `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="5d0ce-136">Open a browser and browse to `http://localhost:4200`.</span></span>