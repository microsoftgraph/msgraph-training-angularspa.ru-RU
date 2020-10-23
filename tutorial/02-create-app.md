<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы создадите новый угловой проект.

1. Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить [угловую](https://www.npmjs.com/package/@angular/cli) утилиту CLI и создать новое угловое приложение.

    ```Shell
    npm install -g @angular/cli@10.1.7
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

1. Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию. Если браузер не открыт, откройте его и перейдите к странице, чтобы [https://localhost:4200/](https://localhost:4200) убедиться, что новое приложение работает.

## <a name="add-node-packages"></a>Добавление пакетов узлов

Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:

- [Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.
- [NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.
- [время для форматирования](https://github.com/moment/moment) даты и времени.
- [Windows — IANA](https://github.com/rubenillodo/windows-iana)
- [msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.

1. Выполните следующие команды в командной панели CLI.

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. Выполните следующую команду в командной панели управления, чтобы добавить угловой пакет локализации (требуется для NG-начальной загрузки).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе описывается создание пользовательского интерфейса для приложения.

1. Откройте **./СРК/стилес.КСС** и добавьте следующие строки.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Добавьте модуль начальной загрузки в приложение. Откройте **/СРК/АПП/АПП.модуле.ТС** и замените его содержимое приведенным ниже.

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

1. Создайте новый файл в папке **./СРК/АПП** с именем **User. TS** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Создайте угловой компонент для верхней панели навигации на странице. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate component nav-bar
    ```

1. После выполнения команды откройте **./СРК/АПП/нав-бар/нав-бар.компонент.ТС** и замените ее содержимое на следующий.

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

1. Откройте **./срк/апп/нав-бар/nav-bar.component.html** и замените его содержимое приведенным ниже.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Создайте домашнюю страницу для приложения. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate component home
    ```

1. После выполнения команды откройте **./СРК/АПП/Хоме/Хоме.компонент.ТС** и замените ее содержимое на следующий.

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

1. Откройте **./срк/апп/хоме/home.component.html** и замените его содержимое приведенным ниже.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Создайте простой `Alert` класс. Создайте новый файл в каталоге **./СРК/АПП** с именем **Alert. TS** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate service alerts
    ```

1. Откройте **/СРК/АПП/алертс.сервице.ТС** и замените его содержимое приведенным ниже.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Создание компонента оповещений для отображения оповещений. В интерфейсе командной строки выполните следующую команду.

    ```Shell
    ng generate component alerts
    ```

1. После выполнения команды откройте **./СРК/АПП/алертс/алертс.компонент.ТС** и замените ее содержимое на следующий.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Откройте **./срк/апп/алертс/alerts.component.html** и замените его содержимое приведенным ниже.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Откройте **/СРК/АПП/АПП-раутинг.модуле.ТС** и замените `const routes: Routes = [];` строку приведенным ниже кодом.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Откройте **./срк/апп/app.component.html** и замените все его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. Добавьте файл изображения с выбранным именем **no-profile-photo.png** в каталог **./СРК/Ассетс** . Это изображение будет использоваться в качестве фотографии пользователя, когда у пользователя нет фотографии в Microsoft Graph.

Сохраните все изменения и обновите страницу. Теперь приложение должно выглядеть по-другому.

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)
