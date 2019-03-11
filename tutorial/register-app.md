<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="6ac90-101">В этом упражнении вы создадите регистрацию нового веб-приложения Azure AD с помощью портала реестра приложений (ARP).</span><span class="sxs-lookup"><span data-stu-id="6ac90-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="6ac90-102">Откройте браузер и перейдите на [портал регистрации приложений](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="6ac90-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="6ac90-103">Вход с использованием **личной учетной записи** (с учетной записью Майкрософт) или **рабочей или учебНой учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="6ac90-104">В верхней части страницы выберите **Добавить приложение** .</span><span class="sxs-lookup"><span data-stu-id="6ac90-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ac90-105">Если на странице отображается несколько кнопок **Добавить приложение** , выберите ту, которая соответствует списку **приложений** для конвергенции.</span><span class="sxs-lookup"><span data-stu-id="6ac90-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="6ac90-106">На странице **Регистрация приложения** задайте для параметра **имя приложения** значение **учебник радиальный график** и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-106">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Снимок экрана: создание нового приложения на веб-сайте портала регистрации приложений](./images/arp-create-app-01.png)

1. <span data-ttu-id="6ac90-108">На странице **Регистрация учебника** по радиальным графикам в разделе **свойства** скопируйте **идентификатор приложения** так, как он понадобится позже.</span><span class="sxs-lookup"><span data-stu-id="6ac90-108">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом только что созданного приложения](./images/arp-create-app-02.png)

1. <span data-ttu-id="6ac90-110">ПроКрутите окно вниз до раздела **платформы** .</span><span class="sxs-lookup"><span data-stu-id="6ac90-110">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="6ac90-111">Нажмите кнопку **Добавить платформу**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-111">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="6ac90-112">В диалоговом окне **Добавление платформы** выберите **веб**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-112">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Снимок экрана: создание платформы для приложения](./images/arp-create-app-03.png)

    1. <span data-ttu-id="6ac90-114">В поле **веб-** платформа введите `http://localhost:4200` **URL-адреса перенаправления**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-114">In the **Web** platform box, enter `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Снимок экрана: недавно добавленная веб-платформа для приложения](./images/arp-create-app-04.png)

1. <span data-ttu-id="6ac90-116">ПроКрутите страницу вниз и выберите команду **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="6ac90-116">Scroll to the bottom of the page and select **Save**.</span></span>