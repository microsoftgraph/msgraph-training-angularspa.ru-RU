<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="82be6-101">В этом упражнении вы создадите регистрацию нового веб-приложения Azure AD с помощью центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="82be6-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="82be6-102">Откройте браузер и перейдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82be6-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="82be6-103">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="82be6-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="82be6-104">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений (Предварительная версия)** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="82be6-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="82be6-105">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="82be6-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="82be6-106">Нажмите кнопку **создать регистрацию**.</span><span class="sxs-lookup"><span data-stu-id="82be6-106">Select **New registration**.</span></span> <span data-ttu-id="82be6-107">На странице **Регистрация приложения** задайте указанные ниже значения.</span><span class="sxs-lookup"><span data-stu-id="82be6-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="82be6-108">Задайте \*\*\*\* для `Angular Graph Tutorial`параметра Name значение.</span><span class="sxs-lookup"><span data-stu-id="82be6-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="82be6-109">Установите **Поддерживаемые типы учетных** записей для **учетных записей в любом организационном каталоге и личных учетНых записях Майкрософт**.</span><span class="sxs-lookup"><span data-stu-id="82be6-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="82be6-110">В разделе **URI перенаправления**установите первый раскрывающийся список `Web` и присвойте ему значение `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="82be6-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Снимок страницы "регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="82be6-112">Выберите **регистр**.</span><span class="sxs-lookup"><span data-stu-id="82be6-112">Choose **Register**.</span></span> <span data-ttu-id="82be6-113">На странице **учебника** по радиальному графику СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="82be6-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](./images/aad-application-id.png)

1. <span data-ttu-id="82be6-115">Выберите пункт **Проверка** подлинности в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="82be6-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="82be6-116">НаХождение неЯвного раздела **предоставления** и включение **маркеров доступа** и **маркеров ID**.</span><span class="sxs-lookup"><span data-stu-id="82be6-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="82be6-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="82be6-117">Choose **Save**.</span></span>

    ![Снимок экрана с неЯвным разделом предоставления](./images/aad-implicit-grant.png)
