<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e8321-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e8321-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="e8321-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="e8321-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="e8321-103">На этом этапе вы интегрируете [библиотеку проверки подлинности (Майкрософт) для радиальной](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) интеграции в приложение.</span><span class="sxs-lookup"><span data-stu-id="e8321-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="e8321-104">Создайте новый файл в каталоге **./СРК** с именем **OAuth. TS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="e8321-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="e8321-105">Замените `YOUR_APP_ID_HERE` идентификатором приложения на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="e8321-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e8321-106">Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **OAuth. TS** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="e8321-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="e8321-107">Откройте **./СРК/АПП/АПП.модуле.ТС** и добавьте приведенные ниже `import` операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="e8321-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="e8321-108">Добавьте в `MsalModule` `imports` массив внутри `@NgModule` объявления и инициализируйте его с помощью идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="e8321-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports" highlight="6-11":::

## <a name="implement-sign-in"></a><span data-ttu-id="e8321-109">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="e8321-109">Implement sign-in</span></span>

<span data-ttu-id="e8321-110">В этом разделе вы создадите службу проверки подлинности и выполните вход и выход.</span><span class="sxs-lookup"><span data-stu-id="e8321-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="e8321-111">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="e8321-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="e8321-112">Создав службу для этого, вы можете легко вставить ее в любые компоненты, которым необходим доступ к методам проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="e8321-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="e8321-113">После завершения выполнения команды откройте **./СРК/АПП/АУС.сервице.ТС** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="e8321-113">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return null;
      }
    }
    ```

1. <span data-ttu-id="e8321-114">Откройте **/СРК/АПП/нав-бар/нав-бар.компонент.ТС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="e8321-114">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="e8321-115">Откройте **/СРК/АПП/Хоме/Хоме.компонент.ТС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="e8321-115">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-32":::

<span data-ttu-id="e8321-116">Сохраните изменения и обновите браузер.</span><span class="sxs-lookup"><span data-stu-id="e8321-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="e8321-117">Нажмите кнопку " **щелкните здесь, чтобы войти** ", а вы будете перенаправлены на `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="e8321-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="e8321-118">Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="e8321-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="e8321-119">Страница приложения должна обновляться, отображая маркер.</span><span class="sxs-lookup"><span data-stu-id="e8321-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="e8321-120">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="e8321-120">Get user details</span></span>

<span data-ttu-id="e8321-121">Теперь служба проверки подлинности устанавливает постоянные значения для отображаемого имени пользователя и адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="e8321-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="e8321-122">Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю.</span><span class="sxs-lookup"><span data-stu-id="e8321-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="e8321-123">Откройте **./СРК/АПП/АУС.сервице.ТС** и добавьте приведенные ниже `import` операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="e8321-123">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="e8321-124">Добавьте к классу `AuthService` новую функцию с именем `getUser`.</span><span class="sxs-lookup"><span data-stu-id="e8321-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="e8321-125">В `getAccessToken` методе, который добавляет оповещение для отображения маркера доступа, в методе необходимо указать и удалить приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="e8321-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="e8321-126">В методе откройте и удалите приведенный ниже код `signIn` .</span><span class="sxs-lookup"><span data-stu-id="e8321-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="e8321-127">Вместо этого добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="e8321-127">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="e8321-128">Этот новый код использует пакет SDK Microsoft Graph для получения сведений о пользователе, а затем создает `User` объект с использованием значений, возвращенных вызовом API.</span><span class="sxs-lookup"><span data-stu-id="e8321-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="e8321-129">Измените `constructor` класс для проверки того, что `AuthService` пользователь уже вошел в систему, и загрузит сведения о нем, если это так.</span><span class="sxs-lookup"><span data-stu-id="e8321-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="e8321-130">Замените существующий `constructor` ниже.</span><span class="sxs-lookup"><span data-stu-id="e8321-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="e8321-131">Удалите временный код из `HomeComponent` класса.</span><span class="sxs-lookup"><span data-stu-id="e8321-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="e8321-132">Откройте **/СРК/АПП/Хоме/Хоме.компонент.ТС** и замените существующую `signIn` функцию на приведенную ниже.</span><span class="sxs-lookup"><span data-stu-id="e8321-132">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet":::

<span data-ttu-id="e8321-133">Теперь, если вы сохраните изменения и запустите приложение, после входа на домашнюю страницу необходимо изменить пользовательский интерфейс, чтобы указать, что вы вошли в систему.</span><span class="sxs-lookup"><span data-stu-id="e8321-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

<span data-ttu-id="e8321-135">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** .</span><span class="sxs-lookup"><span data-stu-id="e8321-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="e8321-136">При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="e8321-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="e8321-138">Хранение и обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="e8321-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="e8321-139">На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="e8321-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="e8321-140">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="e8321-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="e8321-141">Однако этот маркер кратковременно используется.</span><span class="sxs-lookup"><span data-stu-id="e8321-141">However, this token is short-lived.</span></span> <span data-ttu-id="e8321-142">Срок действия маркера истечет через час после его выдачи.</span><span class="sxs-lookup"><span data-stu-id="e8321-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="e8321-143">Так как приложение использует библиотеку MSAL, нет необходимости внедрять логику хранения или обновления маркеров.</span><span class="sxs-lookup"><span data-stu-id="e8321-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="e8321-144">`MsalService`Кэширует маркер в хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="e8321-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="e8321-145">`acquireTokenSilent`Метод сначала проверяет кэшированный маркер, и если срок его действия не истек, он возвращается.</span><span class="sxs-lookup"><span data-stu-id="e8321-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="e8321-146">Если срок действия истек, он отправляет необъявляемый запрос на получение нового.</span><span class="sxs-lookup"><span data-stu-id="e8321-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
