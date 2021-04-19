<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширит приложение от предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете библиотеку проверки подлинности [Microsoft для Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) в приложение.

1. Создайте новый файл в **каталоге ./src** с именем **oauth.ts** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    Замените `YOUR_APP_ID_HERE` с помощью ID приложения с портала регистрации приложений.

    > [!IMPORTANT]
    > Если вы используете источник управления, например git, то сейчас самое время исключить файл **oauth.ts** из-под контроля источника, чтобы избежать случайной утечки вашего ID приложения.

1. Откройте **./src/app/app.module.ts** и добавьте следующие утверждения в `import` верхнюю часть файла.

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Добавьте следующую функцию под `import` утверждениями.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. Добавьте массив `MsalModule` `imports` внутри `@NgModule` объявления.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. Добавьте массив `MSALInstanceFactory` и в массив внутри `MsalService` `providers` `@NgModule` объявления.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a>Реализация входа в систему

В этом разделе вы создадим службу проверки подлинности и внедрим вход и вход.

1. Запустите следующую команду в CLI.

    ```Shell
    ng generate service auth
    ```

    Создав для этого службу, вы можете легко ввести ее в любые компоненты, которые нуждаются в доступе к методам проверки подлинности.

1. После завершения команды откройте **./src/app/auth.service.ts** и замените содержимое следующим кодом.

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

1. Откройте **./src/app/nav-bar/nav-bar.component.ts** и замените его содержимое следующим.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. Откройте **./src/app/home/home.component.ts** и замените его содержимое следующим.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

Сохраните изменения и обновите браузер. Нажмите **кнопку Нажмите здесь,** чтобы войти в кнопку, и вы должны быть перенаправлены `https://login.microsoftonline.com` на . Вход с учетной записью Майкрософт и согласие на запрашиваемую разрешения. Страница приложения должна обновиться, показывая маркер.

### <a name="get-user-details"></a>Получение сведений о пользователе

Сейчас служба проверки подлинности задает постоянные значения для имени и адреса электронной почты пользователя. Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю.

1. Откройте **./src/app/auth.service.ts** и добавьте следующие утверждения в `import` верхнюю часть файла.

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. Добавьте к классу `AuthService` новую функцию с именем `getUser`.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. Найдите и удалите следующий код в `getAccessToken` методе, который добавляет оповещение для отображения маркера доступа.

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. Найдите и удалите следующий код из `signIn` метода.

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. На его месте добавьте следующий код.

    ```typescript
    this.user = await this.getUser();
    ```

    Этот новый код использует SDK Microsoft Graph для получения сведений пользователя, а затем создает объект с использованием значений, возвращаемого `User` вызовом API.

1. Измените класс, чтобы проверить, вошел ли пользователь, и `constructor` `AuthService` загрузив его данные, если это так. Замените `constructor` существующее следующим.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. Удалите временный код из `HomeComponent` класса. Откройте **./src/app/home/home.component.ts** и замените существующую функцию `signIn` на следующую.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

Теперь, если вы сохраните изменения и запустите приложение, после регистрации вы должны вернуться на домашную страницу, но пользовательский интерфейс должен измениться, чтобы указать, что вы подписаны.

![Снимок экрана с домашней страницей после входа в систему](./images/add-aad-auth-01.png)

Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к **ссылке Sign Out.** **Щелкнув кнопку "Выйти",** вы сбросит сеанс и возвращает вас на домашнюю страницу.

![Снимок экрана с раскрывающимся меню с пунктом "Выход"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Хранение и обновление маркеров

На этом этапе у приложения есть маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер недолговечен. Срок действия маркера истекает через час после его выпуска. Поскольку приложение использует библиотеку MSAL, не нужно внедрять логику хранения маркеров или обновления. `MsalService`Кэширования маркера в хранилище браузера. Метод сначала проверяет кэш-маркер, и если срок его действия не истек, он `acquireTokenSilent` возвращает его. Если срок действия истек, он делает молчаливый запрос на получение нового.
