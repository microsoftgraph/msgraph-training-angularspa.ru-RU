# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="c4c01-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="c4c01-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4c01-102">Необходимые условия</span><span class="sxs-lookup"><span data-stu-id="c4c01-102">Prerequisites</span></span>

<span data-ttu-id="c4c01-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="c4c01-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="c4c01-104">[Узел node. js](https://nodejs.org) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="c4c01-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="c4c01-105">Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="c4c01-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="c4c01-106">(**Примечание:** это руководство было написано с узлом версии 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="c4c01-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="c4c01-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="c4c01-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="c4c01-108">[Угловой CLI](https://cli.angular.io/) , установленный на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="c4c01-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="c4c01-109">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="c4c01-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="c4c01-110">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="c4c01-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="c4c01-111">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="c4c01-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="c4c01-112">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="c4c01-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="c4c01-113">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4c01-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="c4c01-114">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4c01-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="c4c01-115">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="c4c01-116">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="c4c01-117">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="c4c01-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="c4c01-118">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-118">Select **New registration**.</span></span> <span data-ttu-id="c4c01-119">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c4c01-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="c4c01-120">Введите **имя** `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="c4c01-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="c4c01-121">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="c4c01-122">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="c4c01-122">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="c4c01-124">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-124">Choose **Register**.</span></span> <span data-ttu-id="c4c01-125">На странице **учебника** по радиальному графику СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="c4c01-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="c4c01-127">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-127">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="c4c01-128">Нахождение неявного раздела **предоставления** и включение **маркеров доступа** и **маркеров ID**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-128">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="c4c01-129">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="c4c01-129">Choose **Save**.</span></span>

    ![Снимок экрана с неявным разделом предоставления](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="c4c01-131">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="c4c01-131">Configure the sample</span></span>

1. <span data-ttu-id="c4c01-132">Переименуйте `oauth.ts.example` файл в `oauth.ts`.</span><span class="sxs-lookup"><span data-stu-id="c4c01-132">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="c4c01-133">Измените `oauth.ts` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="c4c01-133">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="c4c01-134">Замените `YOUR_APP_ID_HERE` идентификатором **приложения** , полученным на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="c4c01-134">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="c4c01-135">В интерфейсе командной строки (CLI) перейдите к этому каталогу и выполните следующую команду, чтобы установить требования.</span><span class="sxs-lookup"><span data-stu-id="c4c01-135">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="c4c01-136">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c4c01-136">Run the sample</span></span>

1. <span data-ttu-id="c4c01-137">Выполните следующую команду в командной панели CLI, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="c4c01-137">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="c4c01-138">Откройте браузер и перейдите по адресу `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="c4c01-138">Open a browser and browse to `http://localhost:4200`.</span></span>
