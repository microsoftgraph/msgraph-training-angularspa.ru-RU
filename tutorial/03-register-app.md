<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="111bb-101">В этом упражнении будет создаваться новая регистрация веб-приложений Azure AD с помощью центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="111bb-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="111bb-102">Откройте браузер и перейдите в [Центр администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="111bb-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="111bb-103">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="111bb-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="111bb-104">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="111bb-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="111bb-105">Снимок экрана регистрации приложения</span><span class="sxs-lookup"><span data-stu-id="111bb-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="111bb-106">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="111bb-106">Select **New registration**.</span></span> <span data-ttu-id="111bb-107">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="111bb-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="111bb-108">Введите **имя** `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="111bb-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="111bb-109">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="111bb-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="111bb-110">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Single-page application (SPA)` и задайте значение `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="111bb-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:4200`.</span></span>

    ![Снимок экрана со страницей регистрации приложения](./images/aad-register-an-app.png)

1. <span data-ttu-id="111bb-112">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="111bb-112">Select **Register**.</span></span> <span data-ttu-id="111bb-113">На странице **Учебник по** угловому графику скопируйте значение ID приложения **(клиента)** и сохраните его, оно потребуется на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="111bb-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с идентификатором приложения для новой регистрации](./images/aad-application-id.png)
