<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8a965-101">В этом упражнении вы расширит приложение от предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a965-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="8a965-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="8a965-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="8a965-103">На этом этапе вы интегрируете библиотеку проверки подлинности [Microsoft для Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) в приложение.</span><span class="sxs-lookup"><span data-stu-id="8a965-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="8a965-104">Создайте новый файл в **каталоге ./src** с именем **oauth.ts** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="8a965-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="8a965-105">Замените `YOUR_APP_ID_HERE` с помощью ID приложения с портала регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="8a965-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8a965-106">Если вы используете источник управления, например git, то сейчас самое время исключить файл **oauth.ts** из-под контроля источника, чтобы избежать случайной утечки вашего ID приложения.</span><span class="sxs-lookup"><span data-stu-id="8a965-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="8a965-107">Откройте **./src/app/app.module.ts** и добавьте следующие утверждения в `import` верхнюю часть файла.</span><span class="sxs-lookup"><span data-stu-id="8a965-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="8a965-108">Добавьте следующую функцию под `import` утверждениями.</span><span class="sxs-lookup"><span data-stu-id="8a965-108">Add the following function below the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. <span data-ttu-id="8a965-109">Добавьте массив `MsalModule` `imports` внутри `@NgModule` объявления.</span><span class="sxs-lookup"><span data-stu-id="8a965-109">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. <span data-ttu-id="8a965-110">Добавьте массив `MSALInstanceFactory` и в массив внутри `MsalService` `providers` `@NgModule` объявления.</span><span class="sxs-lookup"><span data-stu-id="8a965-110">Add the `MSALInstanceFactory` and `MsalService` to the `providers` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a><span data-ttu-id="8a965-111">Реализация входа в систему</span><span class="sxs-lookup"><span data-stu-id="8a965-111">Implement sign-in</span></span>

<span data-ttu-id="8a965-112">В этом разделе вы создадим службу проверки подлинности и внедрим вход и вход.</span><span class="sxs-lookup"><span data-stu-id="8a965-112">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="8a965-113">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="8a965-113">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="8a965-114">Создав для этого службу, вы можете легко ввести ее в любые компоненты, которые нуждаются в доступе к методам проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8a965-114">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="8a965-115">После завершения команды откройте **./src/app/auth.service.ts** и замените содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="8a965-115">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { AccountInfo } from '@azure/msal-browser';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user?: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = undefined;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        const result = await this.msalService
          .loginPopup(OAuthSettings)
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Login failed',
              JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.msalService.instance.setActiveAccount(result.account);
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      async signOut(): Promise<void> {
        await this.msalService.logout().toPromise();
        this.user = undefined;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        const result = await this.msalService
          .acquireTokenSilent({
            scopes: OAuthSettings.scopes
          })
          .toPromise()
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
        return '';
      }
    }
    ```

1. <span data-ttu-id="8a965-116">Откройте **./src/app/nav-bar/nav-bar.component.ts** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="8a965-116">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. <span data-ttu-id="8a965-117">Откройте **./src/app/home/home.component.ts** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="8a965-117">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

<span data-ttu-id="8a965-118">Сохраните изменения и обновите браузер.</span><span class="sxs-lookup"><span data-stu-id="8a965-118">Save your changes and refresh the browser.</span></span> <span data-ttu-id="8a965-119">Нажмите **кнопку Нажмите здесь,** чтобы войти в кнопку, и вы должны быть перенаправлены `https://login.microsoftonline.com` на .</span><span class="sxs-lookup"><span data-stu-id="8a965-119">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="8a965-120">Вход с учетной записью Майкрософт и согласие на запрашиваемую разрешения.</span><span class="sxs-lookup"><span data-stu-id="8a965-120">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="8a965-121">Страница приложения должна обновиться, показывая маркер.</span><span class="sxs-lookup"><span data-stu-id="8a965-121">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="8a965-122">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="8a965-122">Get user details</span></span>

<span data-ttu-id="8a965-123">Сейчас служба проверки подлинности задает постоянные значения для имени и адреса электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="8a965-123">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="8a965-124">Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю.</span><span class="sxs-lookup"><span data-stu-id="8a965-124">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="8a965-125">Откройте **./src/app/auth.service.ts** и добавьте следующие утверждения в `import` верхнюю часть файла.</span><span class="sxs-lookup"><span data-stu-id="8a965-125">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="8a965-126">Добавьте к классу `AuthService` новую функцию с именем `getUser`.</span><span class="sxs-lookup"><span data-stu-id="8a965-126">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. <span data-ttu-id="8a965-127">Найдите и удалите следующий код в `getAccessToken` методе, который добавляет оповещение для отображения маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="8a965-127">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="8a965-128">Найдите и удалите следующий код из `signIn` метода.</span><span class="sxs-lookup"><span data-stu-id="8a965-128">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="8a965-129">На его месте добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="8a965-129">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="8a965-130">Этот новый код использует SDK Microsoft Graph для получения сведений пользователя, а затем создает объект с использованием значений, возвращаемого `User` вызовом API.</span><span class="sxs-lookup"><span data-stu-id="8a965-130">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="8a965-131">Измените класс, чтобы проверить, вошел ли пользователь, и `constructor` `AuthService` загрузив его данные, если это так.</span><span class="sxs-lookup"><span data-stu-id="8a965-131">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="8a965-132">Замените `constructor` существующее следующим.</span><span class="sxs-lookup"><span data-stu-id="8a965-132">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. <span data-ttu-id="8a965-133">Удалите временный код из `HomeComponent` класса.</span><span class="sxs-lookup"><span data-stu-id="8a965-133">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="8a965-134">Откройте **./src/app/home/home.component.ts** и замените существующую функцию `signIn` на следующую.</span><span class="sxs-lookup"><span data-stu-id="8a965-134">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

<span data-ttu-id="8a965-135">Теперь, если вы сохраните изменения и запустите приложение, после регистрации вы должны вернуться на домашную страницу, но пользовательский интерфейс должен измениться, чтобы указать, что вы подписаны.</span><span class="sxs-lookup"><span data-stu-id="8a965-135">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Снимок экрана с домашней страницей после входа в систему](./images/add-aad-auth-01.png)

<span data-ttu-id="8a965-137">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к **ссылке Sign Out.**</span><span class="sxs-lookup"><span data-stu-id="8a965-137">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="8a965-138">**Щелкнув кнопку "Выйти",** вы сбросит сеанс и возвращает вас на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="8a965-138">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Снимок экрана с раскрывающимся меню с пунктом "Выход"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="8a965-140">Хранение и обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="8a965-140">Storing and refreshing tokens</span></span>

<span data-ttu-id="8a965-141">На этом этапе у приложения есть маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="8a965-141">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="8a965-142">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="8a965-142">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="8a965-143">Однако этот маркер недолговечен.</span><span class="sxs-lookup"><span data-stu-id="8a965-143">However, this token is short-lived.</span></span> <span data-ttu-id="8a965-144">Срок действия маркера истекает через час после его выпуска.</span><span class="sxs-lookup"><span data-stu-id="8a965-144">The token expires an hour after it is issued.</span></span> <span data-ttu-id="8a965-145">Поскольку приложение использует библиотеку MSAL, не нужно внедрять логику хранения маркеров или обновления.</span><span class="sxs-lookup"><span data-stu-id="8a965-145">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="8a965-146">`MsalService`Кэширования маркера в хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="8a965-146">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="8a965-147">Метод сначала проверяет кэш-маркер, и если срок его действия не истек, он `acquireTokenSilent` возвращает его.</span><span class="sxs-lookup"><span data-stu-id="8a965-147">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="8a965-148">Если срок действия истек, он делает молчаливый запрос на получение нового.</span><span class="sxs-lookup"><span data-stu-id="8a965-148">If it is expired, it makes a silent request to obtain a new one.</span></span>
