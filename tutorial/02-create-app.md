<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="87552-101">Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить [угловую](https://www.npmjs.com/package/@angular/cli) утилиту CLI и создать новое угловое приложение.</span><span class="sxs-lookup"><span data-stu-id="87552-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

<span data-ttu-id="87552-102">В разделе CLI будет предложено ввести дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="87552-102">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="87552-103">Ответьте на приглашения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="87552-103">Answer the prompts as follows.</span></span>

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

<span data-ttu-id="87552-104">После завершения выполнения команды перейдите к `graph-tutorial` каталогу в CLI и выполните следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="87552-104">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
ng serve --open
```

<span data-ttu-id="87552-105">Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="87552-105">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="87552-106">Если браузер не открыт, откройте его и перейдите к [https://localhost:4200/](https://localhost:4200) странице, чтобы убедиться, что новое приложение работает.</span><span class="sxs-lookup"><span data-stu-id="87552-106">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

<span data-ttu-id="87552-107">Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="87552-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="87552-108">[Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.</span><span class="sxs-lookup"><span data-stu-id="87552-108">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="87552-109">[NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.</span><span class="sxs-lookup"><span data-stu-id="87552-109">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="87552-110">[угловой фонтавесоме](https://github.com/FortAwesome/angular-fontawesome) используется для использования значков Фонтавесоме в радиальном.</span><span class="sxs-lookup"><span data-stu-id="87552-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="87552-111">[фонтавесоме – SVG — основной](https://github.com/FortAwesome/Font-Awesome), [бесплатный — SVG — значки](https://github.com/FortAwesome/Font-Awesome)и [свободные сплошные SVG значки](https://github.com/FortAwesome/Font-Awesome) для значков фонтавесоме, используемых в примере.</span><span class="sxs-lookup"><span data-stu-id="87552-111">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="87552-112">[время для форматирования](https://github.com/moment/moment) даты и времени.</span><span class="sxs-lookup"><span data-stu-id="87552-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="87552-113">[msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="87552-113">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="87552-114">[рксжс — совместимость](https://github.com/ReactiveX/rxjs/tree/master/compat), обязательный для `msal-angular` пакета.</span><span class="sxs-lookup"><span data-stu-id="87552-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), required for the `msal-angular` package.</span></span>
- <span data-ttu-id="87552-115">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="87552-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="87552-116">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="87552-116">Run the following command in your CLI.</span></span>

```Shell
npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.5.0 @fortawesome/fontawesome-svg-core@1.2.25
npm install @fortawesome/free-regular-svg-icons@5.11.2 @fortawesome/free-solid-svg-icons@5.11.2
npm install moment@2.24.0 moment-timezone@0.5.27 @ng-bootstrap/ng-bootstrap@5.1.4
npm install @azure/msal-angular@0.1.4 rxjs-compat@6.5.3 @microsoft/microsoft-graph-client@2.0.0
```

## <a name="design-the-app"></a><span data-ttu-id="87552-117">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="87552-117">Design the app</span></span>

<span data-ttu-id="87552-118">Для начала добавьте файлы начальной загрузки CSS в приложение, а также некоторые глобальные стили.</span><span class="sxs-lookup"><span data-stu-id="87552-118">Start by adding the Bootstrap CSS files to the app, as well as some global styles.</span></span> <span data-ttu-id="87552-119">Откройте `./src/styles.css` и добавьте следующие строки.</span><span class="sxs-lookup"><span data-stu-id="87552-119">Open the `./src/styles.css` and add the following lines.</span></span>

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

<span data-ttu-id="87552-120">Затем добавьте в приложение модули начальной загрузки и Фонтавесоме.</span><span class="sxs-lookup"><span data-stu-id="87552-120">Next, add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="87552-121">Откройте `./src/app/app.module.ts` и добавьте приведенные `import` ниже операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="87552-121">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

<span data-ttu-id="87552-122">Затем добавьте следующий код после всех `import` операторов.</span><span class="sxs-lookup"><span data-stu-id="87552-122">Then add the following code after all of the `import` statements.</span></span>

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

<span data-ttu-id="87552-123">В `@NgModule` объявлении замените существующий `imports` массив приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="87552-123">In the `@NgModule` declaration, replace the existing `imports` array with the following.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

<span data-ttu-id="87552-124">Теперь создайте угловой компонент для верхней панели навигации на странице.</span><span class="sxs-lookup"><span data-stu-id="87552-124">Now generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="87552-125">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="87552-125">In your CLI, run the following command.</span></span>

```Shell
ng generate component nav-bar
```

<span data-ttu-id="87552-126">После выполнения команды откройте `./src/app/nav-bar/nav-bar.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="87552-126">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-127">Откройте `./src/app/nav-bar/nav-bar.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="87552-127">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-128">Затем создайте домашнюю страницу для приложения.</span><span class="sxs-lookup"><span data-stu-id="87552-128">Next, create a home page for the app.</span></span> <span data-ttu-id="87552-129">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="87552-129">Run the following command in your CLI.</span></span>

```Shell
ng generate component home
```

<span data-ttu-id="87552-130">После выполнения команды откройте `./src/app/home/home.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="87552-130">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-131">Затем откройте `./src/app/home/home.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="87552-131">Then open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-132">Теперь создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю.</span><span class="sxs-lookup"><span data-stu-id="87552-132">Now create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="87552-133">Для начала создайте простой `Alert` класс.</span><span class="sxs-lookup"><span data-stu-id="87552-133">Start by creating a simple `Alert` class.</span></span> <span data-ttu-id="87552-134">Создайте новый файл в `./src/app` каталоге `alert.ts` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="87552-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

<span data-ttu-id="87552-135">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="87552-135">In your CLI, run the following command.</span></span>

```Shell
ng generate service alerts
```

<span data-ttu-id="87552-136">Откройте `./src/app/alerts.service.ts` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="87552-136">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-137">Теперь создайте компонент оповещений для отображения оповещений.</span><span class="sxs-lookup"><span data-stu-id="87552-137">Now generate an alerts component to display alerts.</span></span> <span data-ttu-id="87552-138">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="87552-138">In your CLI, run the following command.</span></span>

```Shell
ng generate component alerts
```

<span data-ttu-id="87552-139">После выполнения команды откройте `./src/app/alerts/alerts.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="87552-139">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="87552-140">Затем откройте `./src/app/alerts/alerts.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="87552-140">Then open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

<span data-ttu-id="87552-141">Теперь, когда эти базовые компоненты определены, обновите приложение, чтобы использовать их.</span><span class="sxs-lookup"><span data-stu-id="87552-141">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="87552-142">Сначала откройте `./src/app/app-routing.module.ts` файл и замените `const routes: Routes = [];` строку приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="87552-142">First, open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

<span data-ttu-id="87552-143">Откройте файл `./src/app/app.component.html` и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="87552-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

<span data-ttu-id="87552-144">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="87552-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="87552-145">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="87552-145">Now, the app should look very different.</span></span>

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)
