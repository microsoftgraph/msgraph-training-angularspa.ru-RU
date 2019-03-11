<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. На этом этапе вы интегрируете [библиотеку проверки](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) подлинности (Майкрософт) для радиальной интеграции в приложение.

Создайте новый файл в `./src` каталоге `oauth.ts` и добавьте указанный ниже код.

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

Замените `YOUR_APP_ID_HERE` идентификатором приложения на портале регистрации приложений.

> [!IMPORTANT]
> Если вы используете систему управления версиями (например, Git), то теперь мы бы не могли исключить `oauth.ts` файл из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

Откройте `./src/app/app.module.ts` и добавьте приведенные `import` ниже операторы в начало файла.

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

Затем добавьте `MsalModule` к `imports` массиву внутри `@NgModule` объявления и инициализируйте его с помощью идентификатора приложения.

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

## <a name="implement-sign-in"></a>Реализация входа

Сначала необходимо определить простой `User` класс для хранения сведений о пользователе, который отображается в приложении. Создайте новый файл в `./src/app` папке с именем `user.ts` и добавьте приведенный ниже код.

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

Теперь создайте службу проверки подлинности. Создав службу для этого, вы можете легко вставить ее в любые компоненты, которым необходим доступ к методам проверки подлинности. Выполните следующую команду в командной панели CLI.

```Shell
ng generate service auth
```

После завершения выполнения команды откройте `./src/app/auth.service.ts` файл и замените его содержимое приведенным ниже кодом.

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

Теперь, когда у вас есть служба проверки подлинности, ее можно вставить в компоненты, которые выполняют вход в систему. Начните с `NavBarComponent`. Откройте `./src/app/nav-bar/nav-bar.component.ts` файл и внесите следующие изменения.

- Добавьте `import { AuthService } from '../auth.service';` в начало файла.
- Удалите свойства `authenticated` и `user` свойства из класса и удалите код, который задает их в `ngOnInit`.
- Чтобы вставить `AuthService` , добавьте следующий параметр `constructor`:. `private authService: AuthService`
- Замените существующий `signIn` метод следующим:

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- Замените существующий `signOut` метод следующим:

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

Когда все будет готово, код должен выглядеть так, как показано ниже.

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

Так как вы `authenticated` удалили `user` свойства и свойства для класса, вам также потребуется обновить `./src/app/nav-bar/nav-bar.component.html` файл. Откройте этот файл и внесите следующие изменения.

- Замените все экземпляры объекта `authenticated` на объект `authService.authenticated`.
- Замените все вхождения `user` на `authService.user`.

Далее обновите `HomeComponent` класс. Внесите в `NavBarComponent` класс все те же изменения `./src/app/home/home.component.ts` , которые были внесены в класс со следующими исключениями.

- В `HomeComponent` классе нет `signOut` метода.
- Замените `signIn` метод немного другой версией. Этот код вызывает `getAccessToken` получение маркера доступа, который сейчас будет выводить маркер в виде ошибки.

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

Когда вы закончите, файл должен выглядеть так, как показано ниже.

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

Наконец, выполните те же замены, что и `./src/app/home/home.component.html` в панели навигации.

Сохраните изменения и обновите браузер. Нажмите кнопку " **щелкните здесь, чтобы войти** ", а вы будете перенаправлены на `https://login.microsoftonline.com`. Войдите с помощью учетной записи Майкрософт и согласия с запрошенными разрешениями. Страница приложения должна обновляться, отображая маркер.

### <a name="get-user-details"></a>Получение сведений о пользователе

Теперь служба проверки подлинности устанавливает постоянные значения для отображаемого имени пользователя и адреса электронной почты. Теперь, когда у вас есть маркер доступа, вы можете получить сведения о пользователях из Microsoft Graph, чтобы эти значения соответствовали текущему пользователю. Откройте `./src/app/auth.service.ts` и добавьте приведенный `import` ниже оператор в начало файла.

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

Добавьте к классу `AuthService` новую функцию с именем `getUser`.

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

В `signIn` методе откройте и удалите приведенный ниже код.

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

Вместо этого добавьте следующий код.

```TypeScript
this.user = await this.getUser();
```

Этот новый код использует пакет SDK Microsoft Graph для получения сведений о пользователе, а затем создает `User` объект с использованием значений, возвращенных вызовом API.

Теперь измените `AuthService` класс `constructor` , чтобы проверить, что пользователь уже вошел в систему, и загрузит сведения о нем, если это так. Замените существующий `constructor` ниже.

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

Наконец, удалите временный код из `HomeComponent` класса. Откройте `./src/app/home/home.component.ts` файл и замените существующую `signIn` функцию на приведенную ниже.

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

Теперь, если вы сохраните изменения и запустите приложение, после входа на домашнюю страницу необходимо изменить пользовательский интерфейс, чтобы указать, что вы вошли в систему.

![Снимок экрана домашней страницы после входа](./images/add-aad-auth-01.png)

Щелкните аватар пользователя в правом верхнем углу, чтобы получить доступ к ссылке **выхода** . При нажатии кнопки **выйти** сбрасывается сеанс и возвращается на домашнюю страницу.

![Снимок экрана с раскрывающимся меню со ссылкой "выйти"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Хранение и обновление маркеров

На этом шаге приложение имеет маркер доступа, который отправляется в `Authorization` заголовке вызовов API. Это маркер, который позволяет приложению получать доступ к Microsoft Graph от имени пользователя.

Однако этот маркер кратковременно используется. Срок действия маркера истечет через час после его выдачи. Так как приложение использует библиотеку MSAL, нет необходимости внедрять логику хранения или обновления маркеров. `MsalService` Кэширует маркер в хранилище браузера. `acquireTokenSilent` Метод сначала проверяет кэшированный маркер, и если срок его действия не истек, он возвращается. Если срок действия истек, он отправляет необъявляемый запрос на получение нового.