# <a name="completed-module-add-azure-ad-authentication"></a><span data-ttu-id="52da8-101">Завершенный модуль: Добавление проверки подлинности Azure AD</span><span class="sxs-lookup"><span data-stu-id="52da8-101">Completed module: Add Azure AD authentication</span></span>

<span data-ttu-id="52da8-102">Версия проекта в этом каталоге соответствует завершению работы с руководством [Добавление проверки подлиннОсти Azure AD](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=3).</span><span class="sxs-lookup"><span data-stu-id="52da8-102">The version of the project in this directory reflects completing the tutorial up through [Add Azure AD authentication](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=3).</span></span> <span data-ttu-id="52da8-103">Если вы используете эту версию проекта, вам необходимо выполнить остальные руководства, начиная с [получения данных календаря](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=4).</span><span class="sxs-lookup"><span data-stu-id="52da8-103">If you use this version of the project, you need to complete the rest of the tutorial starting at [Get calendar data](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=4).</span></span>

> <span data-ttu-id="52da8-104">**Примечание:** Предполагается, что вы уже зарегистрировали приложение на портале регистрации приложений, как указано в разделе [Регистрация приложения на портале](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=2).</span><span class="sxs-lookup"><span data-stu-id="52da8-104">**Note:** It is assumed that you have already registered an application in the app registration portal as specified in [Register the app in the portal](https://docs.microsoft.com/graph/training/angular-tutorial?tutorial-step=2).</span></span> <span data-ttu-id="52da8-105">Эту версию примера необходимо настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="52da8-105">You need to configure this version of the sample as follows:</span></span>
>
> 1. <span data-ttu-id="52da8-106">Переименуйте `oauth.ts.example` файл в `oauth.ts`.</span><span class="sxs-lookup"><span data-stu-id="52da8-106">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
> 1. <span data-ttu-id="52da8-107">Измените `oauth.ts` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="52da8-107">Edit the `oauth.ts` file and make the following changes.</span></span>
>     1. <span data-ttu-id="52da8-108">Замените `YOUR_APP_ID_HERE` идентификатором **приложения** , полученНым на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="52da8-108">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>