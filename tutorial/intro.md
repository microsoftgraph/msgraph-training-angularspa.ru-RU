<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="db1b8-101">В этом руководстве рассказывается о том, как создать радиальное одностраничное приложение, использующее Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="db1b8-101">This tutorial teaches you how to build an Angular single-page app that uses Microsoft Graph to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="db1b8-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-angularspa).</span><span class="sxs-lookup"><span data-stu-id="db1b8-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db1b8-103">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="db1b8-103">Prerequisites</span></span>

<span data-ttu-id="db1b8-104">Прежде чем приступить к работе с этим руководством, на компьютере для разработки должен быть установлен [node. js](https://nodejs.org) .</span><span class="sxs-lookup"><span data-stu-id="db1b8-104">Before you start this tutorial, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="db1b8-105">Если у вас нет Node. js, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="db1b8-105">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="db1b8-106">Это руководство было написано с узлом версии 10.15.3.</span><span class="sxs-lookup"><span data-stu-id="db1b8-106">This tutorial was written with Node version 10.15.3.</span></span> <span data-ttu-id="db1b8-107">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="db1b8-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="db1b8-108">Отзывы</span><span class="sxs-lookup"><span data-stu-id="db1b8-108">Feedback</span></span>

<span data-ttu-id="db1b8-109">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-angularspa).</span><span class="sxs-lookup"><span data-stu-id="db1b8-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>