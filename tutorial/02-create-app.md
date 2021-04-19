<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы создадим новый проект Angular.

1. Откройте интерфейс командной строки (CLI), перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующие команды для установки инструмента [Angular CLI](https://www.npmjs.com/package/@angular/cli) и создания нового приложения Angular.

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. Angular CLI будет подсказок для получения дополнительных сведений. Ответьте на запросы следующим образом.

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. После завершения команды измените каталог в CLI и запустите следующую команду, `graph-tutorial` чтобы запустить локальный веб-сервер.

    ```Shell
    ng serve --open
    ```

1. Браузер по умолчанию открывается [https://localhost:4200/](https://localhost:4200) на странице Angular по умолчанию. Если браузер не открыт, откройте его и просмотрите, чтобы убедиться, [https://localhost:4200/](https://localhost:4200) что новое приложение работает.

## <a name="add-node-packages"></a>Добавление пакетов node

Прежде чем двигаться дальше, установите дополнительные пакеты, которые вы будете использовать позже:

- [bootstrap](https://github.com/twbs/bootstrap) для укладки и общих компонентов.
- [ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) для использования компонентов Bootstrap из Angular.
- [время](https://github.com/moment/moment) форматирования дат и времени.
- [Windows-iana](https://github.com/rubenillodo/windows-iana)
- [msal-angular для](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) проверки подлинности в Azure Active Directory и получения маркеров доступа.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.

1. Запустите следующие команды в CLI.

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.1
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. Запустите следующую команду в CLI, чтобы добавить пакет локализации Angular (необходимый ng-bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе вы создадим пользовательский интерфейс для приложения.

1. Откройте **./src/styles.css и** добавьте следующие строки.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Добавьте модуль Bootstrap в приложение. Откройте **./src/app/app.module.ts** и замените содержимое следующим.

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. Создайте новый файл в **папке ./src/app** с именем **user.ts** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. Создание компонента Angular для верхней навигации на странице. В CLI запустите следующую команду.

    ```Shell
    ng generate component nav-bar
    ```

1. После завершения команды откройте **./src/app/nav-bar/nav-bar.component.ts** и замените содержимое на следующее.

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean = false;
      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

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
          avatar: '',
          timeZone: ''
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = undefined;
      }
    }
    ```

1. Откройте **./src/app/nav-bar/nav-bar.component.html и замените** его содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Создание домашней страницы для приложения. Запустите следующую команду в CLI.

    ```Shell
    ng generate component home
    ```

1. После завершения команды откройте **./src/app/home.component.ts** и замените содержимое на следующее.

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }
    }
    ```

1. Откройте **./src/app/home/home.component.html** и замените его содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Создайте простой `Alert` класс. Создайте новый файл в **каталоге ./src/app** с именем **alert.ts** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. Создайте службу оповещения, которую приложение может использовать для отображения сообщений пользователю. В CLI запустите следующую команду.

    ```Shell
    ng generate service alerts
    ```

1. Откройте **./src/app/alerts.service.ts** и замените содержимое на следующее.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Создание компонента оповещений для отображения оповещений. В CLI запустите следующую команду.

    ```Shell
    ng generate component alerts
    ```

1. После завершения команды откройте **./src/app/alerts/alerts.component.ts** и замените содержимое на следующее.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. Откройте **./src/app/alerts/alerts.component.html** и замените его содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. Откройте **./src/app/app-routing.module.ts** и замените `const routes: Routes = [];` строку следующим кодом.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Откройте **./src/app/app.component.html** и замените все содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. Добавьте файл изображений выбора с именем **no-profile-photo.png** в **каталоге ./src/assets.** Это изображение будет использоваться в качестве фотографии пользователя, если у пользователя нет фотографии в Microsoft Graph.

Сохраните все изменения и обновите страницу. Теперь приложение должно выглядеть совсем по-другому.

![Снимок экрана: обновленная домашняя страница](images/create-app-01.png)
