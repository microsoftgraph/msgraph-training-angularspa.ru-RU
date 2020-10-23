<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0d179-101">В этом разделе вы создадите новый угловой проект.</span><span class="sxs-lookup"><span data-stu-id="0d179-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="0d179-102">Откройте интерфейс командной строки (CLI), перейдите к каталогу, в котором у вас есть права на создание файлов, и выполните следующие команды, чтобы установить [угловую](https://www.npmjs.com/package/@angular/cli) утилиту CLI и создать новое угловое приложение.</span><span class="sxs-lookup"><span data-stu-id="0d179-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. <span data-ttu-id="0d179-103">В разделе CLI будет предложено ввести дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="0d179-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="0d179-104">Ответьте на приглашения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0d179-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="0d179-105">После завершения выполнения команды перейдите к `graph-tutorial` каталогу в CLI и выполните следующую команду для запуска локального веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="0d179-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="0d179-106">Браузер, используемый по умолчанию, открывается [https://localhost:4200/](https://localhost:4200) со радиальной страницей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0d179-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="0d179-107">Если браузер не открыт, откройте его и перейдите к странице, чтобы [https://localhost:4200/](https://localhost:4200) убедиться, что новое приложение работает.</span><span class="sxs-lookup"><span data-stu-id="0d179-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="0d179-108">Добавление пакетов узлов</span><span class="sxs-lookup"><span data-stu-id="0d179-108">Add Node packages</span></span>

<span data-ttu-id="0d179-109">Прежде чем переходить, установите несколько дополнительных пакетов, которые будут использоваться позже:</span><span class="sxs-lookup"><span data-stu-id="0d179-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="0d179-110">[Начальная](https://github.com/twbs/bootstrap) Загрузка стилей и общих компонентов.</span><span class="sxs-lookup"><span data-stu-id="0d179-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="0d179-111">[NG — начальная](https://github.com/ng-bootstrap/ng-bootstrap) загрузка для использования компонентов начальной загрузки из радиальной.</span><span class="sxs-lookup"><span data-stu-id="0d179-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="0d179-112">[время для форматирования](https://github.com/moment/moment) даты и времени.</span><span class="sxs-lookup"><span data-stu-id="0d179-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="0d179-113">Windows — IANA</span><span class="sxs-lookup"><span data-stu-id="0d179-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="0d179-114">[msal — угловой](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) для проверки подлинности в Azure Active Directory и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="0d179-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="0d179-115">[Microsoft — Graph — клиент](https://github.com/microsoftgraph/msgraph-sdk-javascript) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0d179-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="0d179-116">Выполните следующие команды в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="0d179-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. <span data-ttu-id="0d179-117">Выполните следующую команду в командной панели управления, чтобы добавить угловой пакет локализации (требуется для NG-начальной загрузки).</span><span class="sxs-lookup"><span data-stu-id="0d179-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="0d179-118">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="0d179-118">Design the app</span></span>

<span data-ttu-id="0d179-119">В этом разделе описывается создание пользовательского интерфейса для приложения.</span><span class="sxs-lookup"><span data-stu-id="0d179-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="0d179-120">Откройте **./СРК/стилес.КСС** и добавьте следующие строки.</span><span class="sxs-lookup"><span data-stu-id="0d179-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="0d179-121">Добавьте модуль начальной загрузки в приложение.</span><span class="sxs-lookup"><span data-stu-id="0d179-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="0d179-122">Откройте **/СРК/АПП/АПП.модуле.ТС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="0d179-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="0d179-123">Создайте новый файл в папке **./СРК/АПП** с именем **User. TS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="0d179-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="0d179-124">Создайте угловой компонент для верхней панели навигации на странице.</span><span class="sxs-lookup"><span data-stu-id="0d179-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="0d179-125">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="0d179-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="0d179-126">После выполнения команды откройте **./СРК/АПП/нав-бар/нав-бар.компонент.ТС** и замените ее содержимое на следующий.</span><span class="sxs-lookup"><span data-stu-id="0d179-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="0d179-127">Откройте **./срк/апп/нав-бар/nav-bar.component.html** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="0d179-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="0d179-128">Создайте домашнюю страницу для приложения.</span><span class="sxs-lookup"><span data-stu-id="0d179-128">Create a home page for the app.</span></span> <span data-ttu-id="0d179-129">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="0d179-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="0d179-130">После выполнения команды откройте **./СРК/АПП/Хоме/Хоме.компонент.ТС** и замените ее содержимое на следующий.</span><span class="sxs-lookup"><span data-stu-id="0d179-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="0d179-131">Откройте **./срк/апп/хоме/home.component.html** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="0d179-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="0d179-132">Создайте простой `Alert` класс.</span><span class="sxs-lookup"><span data-stu-id="0d179-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="0d179-133">Создайте новый файл в каталоге **./СРК/АПП** с именем **Alert. TS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="0d179-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="0d179-134">Создайте службу оповещений, которую приложение может использовать для отображения сообщений пользователю.</span><span class="sxs-lookup"><span data-stu-id="0d179-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="0d179-135">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="0d179-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="0d179-136">Откройте **/СРК/АПП/алертс.сервице.ТС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="0d179-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="0d179-137">Создание компонента оповещений для отображения оповещений.</span><span class="sxs-lookup"><span data-stu-id="0d179-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="0d179-138">В интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="0d179-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="0d179-139">После выполнения команды откройте **./СРК/АПП/алертс/алертс.компонент.ТС** и замените ее содержимое на следующий.</span><span class="sxs-lookup"><span data-stu-id="0d179-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="0d179-140">Откройте **./срк/апп/алертс/alerts.component.html** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="0d179-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="0d179-141">Откройте **/СРК/АПП/АПП-раутинг.модуле.ТС** и замените `const routes: Routes = [];` строку приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="0d179-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="0d179-142">Откройте **./срк/апп/app.component.html** и замените все его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="0d179-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. <span data-ttu-id="0d179-143">Добавьте файл изображения с выбранным именем **no-profile-photo.png** в каталог **./СРК/Ассетс** .</span><span class="sxs-lookup"><span data-stu-id="0d179-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="0d179-144">Это изображение будет использоваться в качестве фотографии пользователя, когда у пользователя нет фотографии в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0d179-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="0d179-145">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="0d179-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="0d179-146">Теперь приложение должно выглядеть по-другому.</span><span class="sxs-lookup"><span data-stu-id="0d179-146">Now, the app should look very different.</span></span>

![Снимок экрана с переработанной домашней страницей](images/create-app-01.png)
