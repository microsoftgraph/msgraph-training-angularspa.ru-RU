<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ee9fe-101">В этом разделе вы создадите новый угловой проект.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="ee9fe-102">Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить [угловую](https://www.npmjs.com/package/@angular/cli) утилиту CLI и создать новое угловое приложение.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. <span data-ttu-id="ee9fe-103">В разделе CLI будет предложено ввести дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="ee9fe-104">Ответьте на приглашения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="ee9fe-105">После завершения выполнения команды перейдите к `graph-tutorial` каталогу в CLI и выполните следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="ee9fe-106">Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="ee9fe-107">Если браузер не открыт, откройте его и перейдите к [https://localhost:4200/](https://localhost:4200) странице, чтобы убедиться, что новое приложение работает.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="ee9fe-108">Добавление пакетов узлов</span><span class="sxs-lookup"><span data-stu-id="ee9fe-108">Add Node packages</span></span>

<span data-ttu-id="ee9fe-109">Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="ee9fe-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="ee9fe-110">[Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="ee9fe-111">[NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="ee9fe-112">[угловой фонтавесоме](https://github.com/FortAwesome/angular-fontawesome) используется для использования значков Фонтавесоме в радиальном.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="ee9fe-113">[фонтавесоме – SVG — основной](https://github.com/FortAwesome/Font-Awesome), [бесплатный — SVG — значки](https://github.com/FortAwesome/Font-Awesome)и [свободные сплошные SVG значки](https://github.com/FortAwesome/Font-Awesome) для значков фонтавесоме, используемых в примере.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-113">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="ee9fe-114">[время для форматирования](https://github.com/moment/moment) даты и времени.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-114">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="ee9fe-115">[msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="ee9fe-116">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="ee9fe-117">Выполните следующие команды в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-117">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. <span data-ttu-id="ee9fe-118">Выполните следующую команду в командной панели управления, чтобы добавить угловой пакет локализации (требуется для NG-начальной загрузки).</span><span class="sxs-lookup"><span data-stu-id="ee9fe-118">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="ee9fe-119">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="ee9fe-119">Design the app</span></span>

<span data-ttu-id="ee9fe-120">В этом разделе описывается создание пользовательского интерфейса для приложения.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-120">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="ee9fe-121">Откройте `./src/styles.css` и добавьте следующие строки.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-121">Open the `./src/styles.css` and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="ee9fe-122">Добавьте в приложение модули начальной загрузки и Фонтавесоме.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-122">Add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="ee9fe-123">Откройте `./src/app/app.module.ts` и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-123">Open `./src/app/app.module.ts` and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="ee9fe-124">Создайте новый файл в `./src/app` папке с именем `user.ts` и добавьте приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-124">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="ee9fe-125">Создайте угловой компонент для верхней панели навигации на странице.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-125">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="ee9fe-126">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-126">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="ee9fe-127">После выполнения команды откройте `./src/app/nav-bar/nav-bar.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="ee9fe-127">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="ee9fe-128">Откройте `./src/app/nav-bar/nav-bar.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-128">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="ee9fe-129">Создайте домашнюю страницу для приложения.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-129">Create a home page for the app.</span></span> <span data-ttu-id="ee9fe-130">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-130">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="ee9fe-131">После выполнения команды откройте `./src/app/home/home.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="ee9fe-131">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="ee9fe-132">Откройте `./src/app/home/home.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-132">Open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="ee9fe-133">Создайте простой `Alert` класс.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-133">Create a simple `Alert` class.</span></span> <span data-ttu-id="ee9fe-134">Создайте новый файл в `./src/app` каталоге `alert.ts` и добавьте указанный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="ee9fe-135">Создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-135">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="ee9fe-136">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-136">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="ee9fe-137">Откройте `./src/app/alerts.service.ts` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-137">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="ee9fe-138">Создание компонента оповещений для отображения оповещений.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-138">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="ee9fe-139">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-139">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="ee9fe-140">После выполнения команды откройте `./src/app/alerts/alerts.component.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="ee9fe-140">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="ee9fe-141">Откройте `./src/app/alerts/alerts.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-141">Open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="ee9fe-142">Откройте `./src/app/app-routing.module.ts` файл и замените `const routes: Routes = [];` строку приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-142">Open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="ee9fe-143">Откройте файл `./src/app/app.component.html` и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

<span data-ttu-id="ee9fe-144">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="ee9fe-145">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="ee9fe-145">Now, the app should look very different.</span></span>

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)
