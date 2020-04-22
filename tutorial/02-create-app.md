<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы создадите новый угловой проект.

1. Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить [угловую](https://www.npmjs.com/package/@angular/cli) утилиту CLI и создать новое угловое приложение.

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. В разделе CLI будет предложено ввести дополнительные сведения. Ответьте на приглашения следующим образом.

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. После завершения выполнения команды перейдите к `graph-tutorial` каталогу в CLI и выполните следующую команду для запуска локального веб-сервера.

    ```Shell
    ng serve --open
    ```

1. Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию. Если браузер не открыт, откройте его и перейдите к [https://localhost:4200/](https://localhost:4200) странице, чтобы убедиться, что новое приложение работает.

## <a name="add-node-packages"></a>Добавление пакетов узлов

Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:

- [Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.
- [NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.
- [угловой фонтавесоме](https://github.com/FortAwesome/angular-fontawesome) используется для использования значков Фонтавесоме в радиальном.
- [фонтавесоме – SVG — основной](https://github.com/FortAwesome/Font-Awesome), [бесплатный — SVG — значки](https://github.com/FortAwesome/Font-Awesome)и [свободные сплошные SVG значки](https://github.com/FortAwesome/Font-Awesome) для значков фонтавесоме, используемых в примере.
- [время для форматирования](https://github.com/moment/moment) даты и времени.
- [msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.

1. Выполните следующие команды в командной панели CLI.

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. Выполните следующую команду в командной панели управления, чтобы добавить угловой пакет локализации (требуется для NG-начальной загрузки).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе описывается создание пользовательского интерфейса для приложения.

1. Откройте `./src/styles.css` и добавьте следующие строки.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Добавьте в приложение модули начальной загрузки и Фонтавесоме. Откройте `./src/app/app.module.ts` и замените его содержимое приведенным ниже.

    ```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { FontAwesomeModule, FaIconLibrary } from '@fortawesome/angular-fontawesome';
    import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
    import { faUserCircle } from '@fortawesome/free-regular-svg-icons';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NavBarComponent } from './nav-bar/nav-bar.component';
    import { HomeComponent } from './home/home.component';
    import { AlertsComponent } from './alerts/alerts.component';

    @NgModule({
      declarations: [
        AppComponent,
        NavBarComponent,
        HomeComponent,
        AlertsComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        NgbModule,
        FontAwesomeModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
      constructor(library: FaIconLibrary) {
        // Register the FontAwesome icons used by the app
        library.addIcons(faExternalLinkAlt, faUserCircle);
      }
     }
    ```

1. Создайте новый файл в `./src/app` папке с именем `user.ts` и добавьте приведенный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Создайте угловой компонент для верхней панели навигации на странице. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate component nav-bar
    ```

1. После выполнения команды откройте `./src/app/nav-bar/nav-bar.component.ts` файл и замените его содержимое на следующее:

    ```TypeScript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean;
      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: User;

      constructor() { }

      ngOnInit() {
        this.showNav = false;
        this.authenticated = false;
        this.user = null;
      }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: null
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = null;
      }
    }
    ```

1. Откройте `./src/app/nav-bar/nav-bar.component.html` файл и замените его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Создайте домашнюю страницу для приложения. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate component home
    ```

1. После выполнения команды откройте `./src/app/home/home.component.ts` файл и замените его содержимое на следующее:

    ```TypeScript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: any;

      constructor() { }

      ngOnInit() {
        this.authenticated = false;
        this.user = {};
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com'
        };
      }
    }
    ```

1. Откройте `./src/app/home/home.component.html` файл и замените его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Создайте простой `Alert` класс. Создайте новый файл в `./src/app` каталоге `alert.ts` и добавьте указанный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate service alerts
    ```

1. Откройте `./src/app/alerts.service.ts` файл и замените его содержимое на приведенный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Создание компонента оповещений для отображения оповещений. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate component alerts
    ```

1. После выполнения команды откройте `./src/app/alerts/alerts.component.ts` файл и замените его содержимое на следующее:

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Откройте `./src/app/alerts/alerts.component.html` файл и замените его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Откройте `./src/app/app-routing.module.ts` файл и замените `const routes: Routes = [];` строку приведенным ниже кодом.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Откройте файл `./src/app/app.component.html` и замените его содержимое приведенным ниже кодом.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

Сохраните все изменения и обновите страницу. Теперь приложение должно выглядеть по-другому.

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)
