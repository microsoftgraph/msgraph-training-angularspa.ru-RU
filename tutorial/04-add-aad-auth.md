<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете [библиотеку проверки подлинности (Майкрософт) для радиальной](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) интеграции в приложение.

1. Создайте новый файл в `./src` каталоге `oauth.ts` и добавьте указанный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.ts.example":::

    Замените `YOUR_APP_ID_HERE` идентификатором приложения на портале регистрации приложений.

    > [!IMPORTANT]
    > Если вы используете систему управления версиями (например, Git), то теперь мы бы не могли исключить `oauth.ts` файл из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

1. Откройте `./src/app/app.module.ts` и добавьте приведенные `import` ниже операторы в начало файла.

    ```TypeScript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Добавьте в `MsalModule` `imports` массив внутри `@NgModule` объявления и инициализируйте его с помощью идентификатора приложения.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports":::

## <a name="implement-sign-in"></a>Реализация входа

В этом разделе вы создадите службу проверки подлинности и выполните вход и выход.

1. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate service auth
    ```

    Создав службу для этого, вы можете легко вставить ее в любые компоненты, которым необходим доступ к методам проверки подлинности.

1. После завершения выполнения команды откройте `./src/app/auth.service.ts` файл и замените его содержимое приведенным ниже кодом.

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
        let result = await this.msalService.loginPopup(OAuthSettings)
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
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.add('Token acquired', result.accessToken);
          return result.accessToken;
        }
        return null;
      }
    }
    ```

1. Откройте `./src/app/nav-bar/nav-bar.component.ts` файл и замените его содержимое на приведенный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. Откройте`./src/app/home/home.component.ts` и замените его содержимое приведенным ниже.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-27":::

Сохраните изменения и обновите браузер. Нажмите кнопку " **щелкните здесь, чтобы войти** ", а вы будете перенаправлены на `https://login.microsoftonline.com`. Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями. Страница приложения должна обновляться, отображая маркер.

### <a name="get-user-details"></a>Получение сведений о пользователе

Теперь служба проверки подлинности устанавливает постоянные значения для отображаемого имени пользователя и адреса электронной почты. Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю.

1. Откройте `./src/app/auth.service.ts` и добавьте приведенный `import` ниже оператор в начало файла.

    ```TypeScript
    import { Client } from '@microsoft/microsoft-graph-client';
    ```

1. Добавьте к классу `AuthService` новую функцию с именем `getUser`.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. В `getAccessToken` методе, который добавляет оповещение для отображения маркера доступа, в методе необходимо указать и удалить приведенный ниже код.

    ```TypeScript
    // Temporary to display token in an error box
    this.alertsService.add('Token acquired', result);
    ```

1. В `signIn` методе откройте и удалите приведенный ниже код.

    ```TypeScript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    ```

1. Вместо этого добавьте следующий код.

    ```TypeScript
    this.user = await this.getUser();
    ```

    Этот новый код использует пакет SDK Microsoft Graph для получения сведений о пользователе, а затем создает `User` объект с использованием значений, возвращенных вызовом API.

1. Измените `AuthService` класс `constructor` для проверки того, что пользователь уже вошел в систему, и загрузит сведения о нем, если это так. Замените существующий `constructor` ниже.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. Удалите временный код из `HomeComponent` класса. Откройте `./src/app/home/home.component.ts` файл и замените существующую `signIn` функцию на приведенную ниже.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet" highlight="5-6":::

Теперь, если вы сохраните изменения и запустите приложение, после входа на домашнюю страницу необходимо изменить пользовательский интерфейс, чтобы указать, что вы вошли в систему.

![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** . При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.

![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Хранение и обновление маркеров

На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер кратковременно используется. Срок действия маркера истечет через час после его выдачи. Так как приложение использует библиотеку MSAL, нет необходимости внедрять логику хранения или обновления маркеров. `MsalService` Кэширует маркер в хранилище браузера. `acquireTokenSilent` Метод сначала проверяет кэшированный маркер, и если срок его действия не истек, он возвращается. Если срок действия истек, он отправляет необъявляемый запрос на получение нового.
