<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="6eea2-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6eea2-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="6eea2-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="6eea2-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="6eea2-103">На этом этапе вы интегрируете [библиотеку проверки](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) подлинности (Майкрософт) для радиальной интеграции в приложение.</span><span class="sxs-lookup"><span data-stu-id="6eea2-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="6eea2-104">Создайте новый файл в `./src` каталоге `oauth.ts` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="6eea2-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="6eea2-105">Замените `YOUR_APP_ID_HERE` идентификатором приложения на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="6eea2-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6eea2-106">Если вы используете систему управления версиями (например, Git), то теперь мы бы не могли исключить `oauth.ts` файл из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="6eea2-107">Откройте `./src/app/app.module.ts` и добавьте приведенные `import` ниже операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="6eea2-108">Затем добавьте `MsalModule` к `imports` массиву внутри `@NgModule` объявления и инициализируйте его с помощью идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule,
  MsalModule.forRoot({
    clientID: OAuthSettings.appId
  })
],
```

## <a name="implement-sign-in"></a><span data-ttu-id="6eea2-109">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="6eea2-109">Implement sign-in</span></span>

<span data-ttu-id="6eea2-110">Сначала необходимо определить простой `User` класс для хранения сведений о пользователе, который отображается в приложении.</span><span class="sxs-lookup"><span data-stu-id="6eea2-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="6eea2-111">Создайте новый файл в `./src/app` папке с именем `user.ts` и добавьте приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="6eea2-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="6eea2-112">Теперь создайте службу проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6eea2-112">Now create an authentication service.</span></span> <span data-ttu-id="6eea2-113">Создав службу для этого, вы можете легко вставить ее в любые компоненты, которым необходим доступ к методам проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6eea2-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="6eea2-114">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="6eea2-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="6eea2-115">После завершения выполнения команды откройте `./src/app/auth.service.ts` файл и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="6eea2-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

```TypeScript
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
    let result = await this.msalService.loginPopup(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
      });

    if (result) {
      this.authenticated = true;
      // Temporary placeholder
      this.user = new User();
      this.user.displayName = "Adele Vance";
      this.user.email = "AdeleV@contoso.com";
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
    let result = await this.msalService.acquireTokenSilent(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
      });

    // Temporary to display token in an error box
    if (result) this.alertsService.add('Token acquired', result);
    return result;
  }
}
```

<span data-ttu-id="6eea2-116">Теперь, когда у вас есть служба проверки подлинности, ее можно вставить в компоненты, которые выполняют вход в систему.</span><span class="sxs-lookup"><span data-stu-id="6eea2-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="6eea2-117">Начните с `NavBarComponent`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="6eea2-118">Откройте `./src/app/nav-bar/nav-bar.component.ts` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="6eea2-119">Добавьте `import { AuthService } from '../auth.service';` в начало файла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="6eea2-120">Удалите свойства `authenticated` и `user` свойства из класса и удалите код, который задает их в `ngOnInit`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="6eea2-121">Чтобы вставить `AuthService` , добавьте следующий параметр `constructor`:. `private authService: AuthService`</span><span class="sxs-lookup"><span data-stu-id="6eea2-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="6eea2-122">Замените существующий `signIn` метод следующим:</span><span class="sxs-lookup"><span data-stu-id="6eea2-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="6eea2-123">Замените существующий `signOut` метод следующим:</span><span class="sxs-lookup"><span data-stu-id="6eea2-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="6eea2-124">Когда все будет готово, код должен выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-124">When you're done, the code should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

import { AuthService } from '../auth.service';

@Component({
  selector: 'app-nav-bar',
  templateUrl: './nav-bar.component.html',
  styleUrls: ['./nav-bar.component.css']
})
export class NavBarComponent implements OnInit {

  // Should the collapsed nav show?
  showNav: boolean;

  constructor(private authService: AuthService) { }

  ngOnInit() {
    this.showNav = false;
  }

  // Used by the Bootstrap navbar-toggler button to hide/show
  // the nav in a collapsed state
  toggleNavBar(): void {
    this.showNav = !this.showNav;
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();
  }

  signOut(): void {
    this.authService.signOut();
  }
}
```

<span data-ttu-id="6eea2-125">Так как вы `authenticated` удалили `user` свойства и свойства для класса, вам также потребуется обновить `./src/app/nav-bar/nav-bar.component.html` файл.</span><span class="sxs-lookup"><span data-stu-id="6eea2-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="6eea2-126">Откройте этот файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="6eea2-127">Замените все экземпляры объекта `authenticated` на объект `authService.authenticated`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="6eea2-128">Замените все вхождения `user` на `authService.user`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="6eea2-129">Далее обновите `HomeComponent` класс.</span><span class="sxs-lookup"><span data-stu-id="6eea2-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="6eea2-130">Внесите в `NavBarComponent` класс все те же изменения `./src/app/home/home.component.ts` , которые были внесены в класс со следующими исключениями.</span><span class="sxs-lookup"><span data-stu-id="6eea2-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="6eea2-131">В `HomeComponent` классе нет `signOut` метода.</span><span class="sxs-lookup"><span data-stu-id="6eea2-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="6eea2-132">Замените `signIn` метод немного другой версией.</span><span class="sxs-lookup"><span data-stu-id="6eea2-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="6eea2-133">Этот код вызывает `getAccessToken` получение маркера доступа, который сейчас будет выводить маркер в виде ошибки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="6eea2-134">Когда вы закончите, файл должен выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-134">When your done, the file should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private authService: AuthService) { }

  ngOnInit() {
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();

    // Temporary to display the token
    if (this.authService.authenticated) {
      let token = await this.authService.getAccessToken();
    }
  }
}
```

<span data-ttu-id="6eea2-135">Наконец, выполните те же замены, что и `./src/app/home/home.component.html` в панели навигации.</span><span class="sxs-lookup"><span data-stu-id="6eea2-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="6eea2-136">Сохраните изменения и обновите браузер.</span><span class="sxs-lookup"><span data-stu-id="6eea2-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="6eea2-137">Нажмите кнопку " **щелкните здесь, чтобы войти** ", а вы будете перенаправлены на `https://login.microsoftonline.com`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="6eea2-138">Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями.</span><span class="sxs-lookup"><span data-stu-id="6eea2-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="6eea2-139">Страница приложения должна обновляться, отображая маркер.</span><span class="sxs-lookup"><span data-stu-id="6eea2-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="6eea2-140">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="6eea2-140">Get user details</span></span>

<span data-ttu-id="6eea2-141">Теперь служба проверки подлинности устанавливает постоянные значения для отображаемого имени пользователя и адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="6eea2-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="6eea2-142">Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю.</span><span class="sxs-lookup"><span data-stu-id="6eea2-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="6eea2-143">Откройте `./src/app/auth.service.ts` и добавьте приведенный `import` ниже оператор в начало файла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="6eea2-144">Добавьте к классу `AuthService` новую функцию с именем `getUser`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

```TypeScript
private async getUser(): Promise<User> {
  if (!this.authenticated) return null;

  let graphClient = Client.init({
    // Initialize the Graph client with an auth
    // provider that requests the token from the
    // auth service
    authProvider: async(done) => {
      let token = await this.getAccessToken()
        .catch((reason) => {
          done(reason, null);
        });

      if (token)
      {
        done(null, token);
      } else {
        done("Could not get an access token", null);
      }
    }
  });

  // Get the user from Graph (GET /me)
  let graphUser = await graphClient.api('/me').get();

  let user = new User();
  user.displayName = graphUser.displayName;
  // Prefer the mail property, but fall back to userPrincipalName
  user.email = graphUser.mail || graphUser.userPrincipalName;

  return user;
}
```

<span data-ttu-id="6eea2-145">В `signIn` методе откройте и удалите приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="6eea2-145">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="6eea2-146">Вместо этого добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="6eea2-146">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="6eea2-147">Этот новый код использует пакет SDK Microsoft Graph для получения сведений о пользователе, а затем создает `User` объект с использованием значений, возвращенных вызовом API.</span><span class="sxs-lookup"><span data-stu-id="6eea2-147">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="6eea2-148">Теперь измените `AuthService` класс `constructor` , чтобы проверить, что пользователь уже вошел в систему, и загрузит сведения о нем, если это так.</span><span class="sxs-lookup"><span data-stu-id="6eea2-148">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="6eea2-149">Замените существующий `constructor` ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-149">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="6eea2-150">Наконец, удалите временный код из `HomeComponent` класса.</span><span class="sxs-lookup"><span data-stu-id="6eea2-150">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="6eea2-151">Откройте `./src/app/home/home.component.ts` файл и замените существующую `signIn` функцию на приведенную ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-151">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="6eea2-152">Теперь, если вы сохраните изменения и запустите приложение, после входа на домашнюю страницу необходимо изменить пользовательский интерфейс, чтобы указать, что вы вошли в систему.</span><span class="sxs-lookup"><span data-stu-id="6eea2-152">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

<span data-ttu-id="6eea2-154">Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** .</span><span class="sxs-lookup"><span data-stu-id="6eea2-154">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="6eea2-155">При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="6eea2-155">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="6eea2-157">Хранение и обновление маркеров</span><span class="sxs-lookup"><span data-stu-id="6eea2-157">Storing and refreshing tokens</span></span>

<span data-ttu-id="6eea2-158">На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API.</span><span class="sxs-lookup"><span data-stu-id="6eea2-158">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="6eea2-159">Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="6eea2-159">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="6eea2-160">Однако этот маркер кратковременно используется.</span><span class="sxs-lookup"><span data-stu-id="6eea2-160">However, this token is short-lived.</span></span> <span data-ttu-id="6eea2-161">Срок действия маркера истечет через час после его выдачи.</span><span class="sxs-lookup"><span data-stu-id="6eea2-161">The token expires an hour after it is issued.</span></span> <span data-ttu-id="6eea2-162">Так как приложение использует библиотеку MSAL, нет необходимости внедрять логику хранения или обновления маркеров.</span><span class="sxs-lookup"><span data-stu-id="6eea2-162">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="6eea2-163">`MsalService` Кэширует маркер в хранилище браузера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-163">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="6eea2-164">`acquireTokenSilent` Метод сначала проверяет кэшированный маркер, и если срок его действия не истек, он возвращается.</span><span class="sxs-lookup"><span data-stu-id="6eea2-164">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="6eea2-165">Если срок действия истек, он отправляет необъявляемый запрос на получение нового.</span><span class="sxs-lookup"><span data-stu-id="6eea2-165">If it is expired, it makes a silent request to obtain a new one.</span></span>
