# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="01ec2-101">Запуск завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="01ec2-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01ec2-102">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="01ec2-102">Prerequisites</span></span>

<span data-ttu-id="01ec2-103">Чтобы запустить завершенный проект в этой папке, необходимо следующее:</span><span class="sxs-lookup"><span data-stu-id="01ec2-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="01ec2-104">[Node.js](https://nodejs.org) на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="01ec2-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="01ec2-105">Если у вас нет Node.js, посетите предыдущую ссылку для параметров загрузки.</span><span class="sxs-lookup"><span data-stu-id="01ec2-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="01ec2-106">(**Примечание.** Этот учебник был написан с версией Node 14.15.0.</span><span class="sxs-lookup"><span data-stu-id="01ec2-106">(**Note:** This tutorial was written with Node version 14.15.0.</span></span> <span data-ttu-id="01ec2-107">Действия в этом руководстве могут работать с другими версиями, но они не были проверены.)</span><span class="sxs-lookup"><span data-stu-id="01ec2-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="01ec2-108">[Угловой CLI,](https://cli.angular.io/) установленный на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="01ec2-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="01ec2-109">Либо личная учетная запись Майкрософт с почтовым ящиком в Outlook.com, либо учетная запись Microsoft work или school.</span><span class="sxs-lookup"><span data-stu-id="01ec2-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="01ec2-110">Если у вас нет учетной записи Майкрософт, существует несколько вариантов получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="01ec2-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="01ec2-111">Вы можете [зарегистрироваться для новой личной учетной записи Майкрософт.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="01ec2-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="01ec2-112">Вы можете [зарегистрироваться в программе разработчиков Office 365,](https://developer.microsoft.com/office/dev-program) чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="01ec2-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="01ec2-113">Регистрация веб-приложения в центре администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01ec2-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="01ec2-114">Откройте браузер и перейдите в [Центр администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01ec2-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="01ec2-115">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="01ec2-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="01ec2-116">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="01ec2-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="01ec2-117">Снимок экрана регистрации приложения</span><span class="sxs-lookup"><span data-stu-id="01ec2-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="01ec2-118">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="01ec2-118">Select **New registration**.</span></span> <span data-ttu-id="01ec2-119">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="01ec2-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="01ec2-120">Введите **имя** `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="01ec2-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="01ec2-121">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="01ec2-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="01ec2-122">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Single-page application (SPA)` и задайте значение `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="01ec2-122">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:4200`.</span></span>

    ![Снимок экрана со страницей регистрации приложения](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="01ec2-124">Нажмите кнопку **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="01ec2-124">Choose **Register**.</span></span> <span data-ttu-id="01ec2-125">На странице **Учебник по** угловому графику скопируйте значение ID приложения **(клиента)** и сохраните его, оно потребуется на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="01ec2-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с идентификатором приложения для новой регистрации](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="01ec2-127">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="01ec2-127">Configure the sample</span></span>

1. <span data-ttu-id="01ec2-128">Переименовать `oauth.ts.example` файл в `oauth.ts` .</span><span class="sxs-lookup"><span data-stu-id="01ec2-128">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="01ec2-129">Изменить файл `oauth.ts` и внести следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="01ec2-129">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="01ec2-130">`YOUR_APP_ID_HERE`Замените **id приложения,** который вы получили на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="01ec2-130">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="01ec2-131">В интерфейсе командной строки (CLI) перейдите в этот каталог и запустите следующую команду для установки требований.</span><span class="sxs-lookup"><span data-stu-id="01ec2-131">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="01ec2-132">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="01ec2-132">Run the sample</span></span>

1. <span data-ttu-id="01ec2-133">Запустите следующую команду в CLI, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="01ec2-133">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="01ec2-134">Откройте браузер и перейдите по адресу `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="01ec2-134">Open a browser and browse to `http://localhost:4200`.</span></span>
