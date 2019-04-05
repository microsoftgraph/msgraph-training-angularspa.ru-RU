<!-- markdownlint-disable MD002 MD041 -->

Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить угловую утилиту [CLI](https://www.npmjs.com/package/@angular/cli) и создать новое угловое приложение.

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

В разделе CLI будет предложено ввести дополнительные сведения. Ответьте на приглашения следующим образом.

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

После завершения выполнения команды перейдите к `graph-tutorial` каталогу в CLI и выполните следующую команду для запуска локального веб-сервера.

```Shell
ng serve --open
```

Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию. Если браузер не открыт, откройте его и перейдите к [https://localhost:4200/](https://localhost:4200) странице, чтобы убедиться, что новое приложение работает.

Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:

- [Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.
- [NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.
- [угловой фонтавесоме](https://github.com/FortAwesome/angular-fontawesome) используется для использования значков Фонтавесоме в радиальном.
- [фонтавесоме – SVG — основной](https://github.com/FortAwesome/Font-Awesome), [бесплатный — SVG — значки](https://github.com/FortAwesome/Font-Awesome)и [свободные сплошные SVG значки](https://github.com/FortAwesome/Font-Awesome) для значков фонтавесоме, используемых в примере.
- [](https://github.com/moment/moment) время для форматирования даты и времени.
- [msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.
- [рксжс — совместимость](https://github.com/ReactiveX/rxjs/tree/master/compat), обязательный для `msal-angular` пакета.
- [Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.

Выполните следующую команду в командной панели CLI.

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.15
npm install @fortawesome/free-regular-svg-icons@5.7.2 @fortawesome/free-solid-svg-icons@5.7.2
npm install moment@2.24.0 moment-timezone@0.5.23 @ng-bootstrap/ng-bootstrap@4.1.0
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.4.0 @microsoft/microsoft-graph-client@1.4.0
```

## <a name="design-the-app"></a>Проектирование приложения

Для начала добавьте файлы начальной загрузки CSS в приложение, а также некоторые глобальные стили. Откройте `./src/styles.css` и добавьте следующие строки.

```CSS
@import "~bootstrap/dist/css/bootstrap.css";

/* Add padding for the nav bar */
body {
  padding-top: 4.5rem;
}

/* Style debug info in alerts */
.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

Затем добавьте в приложение модули начальной загрузки и Фонтавесоме. Откройте `./src/app/app.module.ts` и добавьте приведенные `import` ниже операторы в начало файла.

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

Затем добавьте следующий код после всех `import` операторов.

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

В `@NgModule` объявлении замените существующий `imports` массив приведенным ниже.

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

Теперь создайте угловой компонент для верхней панели навигации на странице. В интерфейсе командной строки выполните следующую команду.

```Shell
ng generate component nav-bar
```

После выполнения команды откройте `./src/app/nav-bar/nav-bar.component.ts` файл и замените его содержимое на следующее:

```TypeScript
import { Component, OnInit } from '@angular/core';

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
  user: any;

  constructor() { }

  ngOnInit() {
    this.showNav = false;
    this.authenticated = false;
    this.user = {};
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
      email: 'AdeleV@contoso.com'
    };
  }

  signOut(): void {
    // Temporary
    this.authenticated = false;
    this.user = {};
  }
}
```

Откройте `./src/app/nav-bar/nav-bar.component.html` файл и замените его содержимое на приведенный ниже код.

```html
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container">
    <a routerLink="/" class="navbar-brand">Angular Graph Tutorial</a>
    <button class="navbar-toggler" type="button" (click)="toggleNavBar()" [attr.aria-expanded]="showNav"
    aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" [class.show]="showNav" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a routerLink="/" class="nav-link" routerLinkActive="active">Home</a>
        </li>
        <li *ngIf="authenticated" class="nav-item">
          <a routerLink="/calendar" class="nav-link" routerLinkActive="active">Calendar</a>
        </li>
      </ul>
      <ul class="navbar-nav justify-content-end">
        <li class="nav-item">
          <a class="nav-link" href="https://docs.microsoft.com/graph/overview" target="_blank">
            <fa-icon [icon]="[ 'fas', 'external-link-alt' ]" class="mr-1"></fa-icon>Docs
          </a>
        </li>
        <li *ngIf="authenticated" ngbDropdown placement="bottom-right" class="nav-item">
          <a ngbDropdownToggle id="userMenu" class="nav-link" href="javascript:undefined" role="button" aria-haspopup="true"
            aria-expanded="false">
            <div *ngIf="user.avatar; then userAvatar else defaultAvatar"></div>
            <ng-template #userAvatar>
              <img src="user.avatar" class="rounded-circle align-self-center mr-2" style="width: 32px;">
            </ng-template>
            <ng-template #defaultAvatar>
              <fa-icon [icon]="[ 'far', 'user-circle' ]" fixedWidth="true" size="lg"
                class="rounded-circle align-self-center mr-2"></fa-icon>
            </ng-template>
          </a>
          <div ngbDropdownMenu aria-labelledby="userMenu">
            <h5 class="dropdown-item-text mb-0">{{user.displayName}}</h5>
            <p class="dropdown-item-text text-muted mb-0">{{user.email}}</p>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="javascript:undefined" role="button" (click)="signOut()">Sign Out</a>
          </div>
        </li>
        <li *ngIf="!authenticated" class="nav-item">
          <a class="nav-link" href="javascript:undefined" role="button" (click)="signIn()">Sign In</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

Затем создайте домашнюю страницу для приложения. Выполните следующую команду в командной панели CLI.

```Shell
ng generate component home
```

После выполнения команды откройте `./src/app/home/home.component.ts` файл и замените его содержимое на следующее:

```TypeScript
import { Component, OnInit } from '@angular/core';

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

Затем откройте `./src/app/home/home.component.html` файл и замените его содержимое на приведенный ниже код.

```html
<div class="jumbotron">
  <h1>Angular Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API from Angular</p>
  <div *ngIf="authenticated; then welcomeUser else signInPrompt"></div>
  <ng-template #welcomeUser>
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  </ng-template>
  <ng-template #signInPrompt>
    <a href="javascript:undefined" class="btn btn-primary btn-large" role="button" (click)="signIn()">Click here to sign in</a>
  </ng-template>
</div>
```

Теперь создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю. Для начала создайте простой `Alert` класс. Создайте новый файл в `./src/app` каталоге `alert.ts` и добавьте указанный ниже код.

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

В интерфейсе командной строки выполните следующую команду.

```Shell
ng generate service alerts
```

Откройте `./src/app/alerts.service.ts` файл и замените его содержимое на приведенный ниже код.

```TypeScript
import { Injectable } from '@angular/core';
import { Alert } from './alert';

@Injectable({
  providedIn: 'root'
})
export class AlertsService {

  alerts: Alert[] = [];

  add(message: string, debug: string) {
    this.alerts.push({message: message, debug: debug});
  }

  remove(alert: Alert) {
    this.alerts.splice(this.alerts.indexOf(alert), 1);
  }
}
```

Теперь создайте компонент оповещений для отображения оповещений. В интерфейсе командной строки выполните следующую команду.

```Shell
ng generate component alerts
```

После выполнения команды откройте `./src/app/alerts/alerts.component.ts` файл и замените его содержимое на следующее:

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AlertsService } from '../alerts.service';
import { Alert } from '../alert';

@Component({
  selector: 'app-alerts',
  templateUrl: './alerts.component.html',
  styleUrls: ['./alerts.component.css']
})
export class AlertsComponent implements OnInit {

  constructor(private alertsService: AlertsService) { }

  ngOnInit() {
  }

  close(alert: Alert) {
    this.alertsService.remove(alert);
  }
}
```

Затем откройте `./src/app/alerts/alerts.component.html` файл и замените его содержимое на приведенный ниже код.

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

Теперь, когда эти базовые компоненты определены, обновите приложение, чтобы использовать их. Сначала откройте `./src/app/app-routing.module.ts` файл и замените `const routes: Routes = [];` строку приведенным ниже кодом.

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

Откройте файл `./src/app/app.component.html` и замените его содержимое приведенным ниже кодом.

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

Сохраните все изменения и обновите страницу. Теперь приложение должно выглядеть по-другому.

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)